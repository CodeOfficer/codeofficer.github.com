---
title: "I approve... approvable things ... and you can too!"
date: 2008-09-22 09:08
comments: true
tags: Rails
---

I recently built a website for a client that needed every type of content "approved" before it should appear on its public pages. I started out thinking that there may be many different states of approval and that perhaps I should use [acts_as_statemachine](http://github.com/freels/acts_as_state_machine/tree/master) to manage these state transitions. After a bit of thinking and frustration with how AASM saves and reloads each model after a transition, I decided I could create something much simpler for my own needs.

What I ended up with was an Approvable Module that I could add into any of my models that had this Approvable behavior. For tracking approvals, I simply use a datetime field called approved_at in each of my Approvable models, whos value would either be Nil or the datetime it was approved at.

In my initial use case, I had 3 models to consider ... Users, Affiliations and their has_many :through join ... Affiliations. A User has_many Affiliates :through their Affiliations. All content is created by a User ... in the case that content is created but an Admin User, I want that content approved by default. Both Affiliates and their Affiliations are approvable, so when an Admin creats an Affiliate, it was important to also approve his affiliation. I make use of [ActiveRecord Callbacks](http://api.rubyonrails.org/classes/ActiveRecord/Callbacks.html) for auto approving.

And this is how I did it. (this code is mostly written for rails 2.1)

First, a peak at my controller for Affilates. You can see in the index action that I've predefined a named_scope called 'approved' that filters a find to retrieve only approved Affiliates. I might have also used another called 'unapproved' in its place. My create action creates an Affiliate and assigns some of its attributes that are protected from mass-assignment in the normal Rails way. All the magic of approval is in the model's themselves. Not much else to see here.

Worth mentioning I suppose, I could also use any of the approving methods manually from my controller, but I prefer to keep my business logic in my models. Most of the methods are injected into the model objects themselves, so really they can be used anywhere.

~~~ ruby
class AffiliatesController < ApplicationController

  def index
    @affiliates = Affiliate.approved.find.all

    respond_to do |format|
      format.html # index.html.erb
    end
  end

  def create
    @affiliate = Affiliate.new(params[:affiliate])
    raise NoPermission unless @affiliate.is_createable_by?(current_user)
    @affiliate.contact = current_user
    @affiliate.creator = current_user
    @affiliate.users << current_user
    respond_to do |format|
      if @affiliate.save
        flash[:notice] = 'Affiliate was successfully created.'
        format.html { redirect_to(account_affiliations_path) }
      else
        format.html { render :action => "new" }
      end
    end
  end

end
~~~

This is my example migration for Affiliates. Note the t.datetime :approved_at field.

~~~ ruby
class CreateAffiliates < ActiveRecord::Migration
  def self.up
    create_table :affiliates do |t|
      t.references :creator, :null => false
      t.string :name
      t.text :description
      t.datetime :approved_at, :default => nil
      t.timestamps
    end
    add_index :affiliates, :creator_id
  end

  def self.down
    drop_table :affiliates
  end
end
~~~

This is my example migration for Affiliations. Note the t.datetime :approved_at field.

~~~ ruby
class CreateAffiliations < ActiveRecord::Migration
  def self.up
    create_table :affiliations do |t|
      t.references :affiliate, :null => false
      t.references :user, :null => false
      t.datetime :approved_at, :default => nil
      t.timestamps
    end
    add_index :affiliations, [:affiliate_id, :user_id]
  end

  def self.down
    drop_table :affiliations
  end
end
~~~

Here is my Approvable module which I've stuck in my RAILS_ROOT/lib folder. It's responsible for the magic of providing methods to my Approvable models to approve/unapprove them. You'll notice that there are all the usual accessors and a few methods to change the approval state of my models.

~~~ ruby
module Approvable

  def self.included(base)
    base.send :include, InstanceMethods
    base.named_scope :approved, lambda { |*args| {:conditions => ["#{base.to_s.tableize}.approved_at IS NOT NULL AND #{base.to_s.tableize}.approved_at < ?", (args.first || Time.now)]} }
    base.named_scope :unapproved, lambda { |*args| {:conditions => ["#{base.to_s.tableize}.approved_at IS NULL OR #{base.to_s.tableize}.approved_at > ?", (args.first || Time.now)]} }
  end

  module InstanceMethods

    def approved?
      !self.approved_at.nil?
    end

    def approve
      self.approved_at = Time.now
    end

    def approve!
      self.approved_at = Time.now
      save!
      reload
    end

    def approval_status
      approved? ? "Approved" : "Unapproved"
    end

    def unapproved?
      self.approved_at.nil?
    end

    def unapprove
      self.approved_at = nil
    end

    def unapprove!
      self.approved_at = nil
      save!
      reload
    end

    def toggle_approval!
      approved? ? unapprove! : approve!
    end

  end

end
~~~

Here is an example of how to use the Approvable module in your models. Simply include the module and you're good to go. In my example, a retrieved Affiliate object can be approved and saved by saying @affiliate.approve! ... or you can approve it without saving by means of @affiliate.approve. The creator.has_role?('admin') stuff is something I coded separately into my User model for permissioning.

~~~ ruby
class Affiliate < ActiveRecord::Base

  include Approvable

  belongs_to :creator, :class_name => 'User'
  has_many :affiliations
  has_many :users, :through => :affiliations

  before_create :auto_approve_affiliate_if_creator_is_an_admin

  private

    def auto_approve_affiliate_if_creator_is_an_admin
      approve if creator.has_role?('admin')
    end

end
~~~

Here is the same example, but on the Affiliations join model.

~~~ ruby
class Affiliation < ActiveRecord::Base

  include Approvable

  belongs_to :affiliate
  belongs_to :user

  before_create :auto_approve_affiliation_if_user_is_an_admin

  private

    def auto_approve_affiliation_if_user_is_an_admin
      approve if user.has_role?('admin')
    end

end
~~~

And that's basically it, if you have any questions please feel free to ask. I've found this module pretty reusable accross multiple applications. I hope you do as well. I've posted the official code over on github as usual: [useful-modules-for-rails](http://github.com/CodeOfficer/useful-modules-for-rails/tree/master)

