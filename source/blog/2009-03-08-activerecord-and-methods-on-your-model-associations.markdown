---
title: "ActiveRecord and methods on your model associations"
date: 2009-03-08 03:00
comments: true
categories: Rails
---

I left PHP to program in Ruby a little over a year ago now, though technically I think I focused more on learning Rails during my first month. Like most who made the journey, I found Ruby a joy to work with, and Rails to have everything I needed to make great web apps. But to this day, the part of Rails I struggle the most with is ActiveRecord. There's just so much magic in there ...

I want to share a syntax I learned this evening for declaring methods on an association in ActiveRecord.

In my system, Users can post content either as themselves, or as approved Affiliate identity. But since other Users might also want to post as this Affiliate, I use a has_many :through relationship to manage this. Imagine an Affiliate is a local business or organization, and Users might belong to multiple instances of them. This may explain things better.

~~~ ruby
class Affiliate < ActiveRecord::Base
  has_many :affiliations
  has_many :users, :through => :affiliations
  attr_accessor :status
end

class Affiliation < ActiveRecord::Base
  belongs_to :affiliate
  belongs_to :user
  attr_accessor :status
end

class User < ActiveRecord::Base
  has_many :affiliations
  has_many :affiliates, :through => :affiliations
end
~~~

I needed to create a select menu for Users creating content in my system so they could select an optional alternate Affiliate identity to post as. The thing is ... these relationships aren't cut and dry. Both the Affiliate and the Affiliation need to have a status of 'approved' or that User cant post as that Affiliate identity.

That said, I needed to populate this select menu for the User. At first I started thinking like this:

~~~ ruby
class User < ActiveRecord::Base
  has_many :affiliations
  has_many :affiliates, :through => :affiliations

  def approved_affiliates
    affiliates.with_status(:approved).find(
      :all,
      :include => [:affiliations],
      :conditions => ["affiliations.status = 'approved'"]
    )
  end
end

@affiliates = current_user.approved_affiliates.collect {|p| [ p.name, p.id.to_s ] }
~~~

But looking in my User model, this didn't seem very elegant and I wanted to find a way to keep the method definition attached to the code where I declare the association. I was already using a named_scope in my Affiliates model "with_status(:approved)" (not shown here) and so I thought I might be able to combine it with a similar named scope on the join.

So, really what I needed was an association method to scope the Affiliate AND the Affiliation to have statuses of 'approved'. Try as I might I couldn't figure out the syntax for this. If you know how it can be done, please do share.

What I ended up with, and am quite happy with is ...

~~~ ruby
class User < ActiveRecord::Base
  has_many :affiliations
  has_many :affiliates, :through => :affiliations do
    def with_combined_statuses(status)
      find(:all, :include => [:affiliations], :conditions => ["affiliations.status = ? AND affiliates.status = ?", status, status])
    end
  end
end

@affiliates = current_user.affiliates.with_combined_statuses('approved').collect {|p| [ p.name, p.id.to_s ] }
~~~

So, I learned you can pass a block to the association and declare methods there, as well, those methods can accept parameters.

Yay me
