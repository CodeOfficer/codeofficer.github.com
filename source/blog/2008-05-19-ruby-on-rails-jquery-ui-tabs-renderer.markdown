---
title: "Ruby on Rails, JQuery UI … and TabsRenderer!"
date: 2008-05-19 09:54
comments: true
categories: Rails
---

I'm a big fan of [JQuery](http://ui.jquery.com/) UI and have been using the [Tab plugin](http://ui.jquery.com/functional_demos/#ui.tabs) a lot recently.

However, I found the code to create each Tab a bit repetitious. It became especially ugly in situations where you might want to display a tab conditionally ... first displaying the tab header, then the tab body, and having to re-use the same condition in both places. yuck!

Sooo, since I had recently [given a presentation](http://www.codeofficer.com/blog/entry/my_ruby_design_patterns_talk_of_april_14th/) at our local [Ruby Users Group](http://groups.google.com/group/maine-ruby-users-group) on [Design Patterns in Ruby](http://amazon.com/dp/0321490452), I thought maybe a little template pattern would be a nice fit here. The result: TabsRenderer!!

TabsRenderer is a view helper that lets me build my tabs in a logical order, and then call a special method to return the final result. As well, creating a tab conditionally is a breeze!

I should mention, since I created TabsRenderer as a helper Class and not just a helper method, I had to address the issue of not being able to use Rail's other helpers within it, as they would be outside my class's scope. My solution was to borrow a technique I learned recently over at [railscasts](http://railscasts.com/) called "[Refactoring Out Helper Object](http://railscasts.com/episodes/101)".

On to the code ...

**5/20/08 - updated with Dietrich's 3rd param options hash suggestion**
**6/03/08 - updated with param for options hash on main div as well**
**6/22/08 - an official repository for this code has been set up at [github](http://github.com/CodeOfficer/jquery-ui-rails-helpers/tree/master)**

Plop this into your helpers directory, and name it tabs_renderer.rb

``` ruby
class TabsRenderer

  def initialize(template, options={})
    @template = template
    @options = options
    @tabs = []
  end

  def create(tab_id, tab_text, options={}, &block)
    raise "Block needed for TabsRenderer#CREATE" unless block_given?
    @tabs << [tab_id, tab_text, options, block]
  end

  def render
    content_tag(:div, (render_tabs + render_bodies), {:id => :tabs}.merge(@options))
  end

  private #  ---------------------------------------------------------------------------

  def render_tabs
    content_tag :ul do
      @tabs.collect do |tab|
        content_tag(:li, link_to(content_tag(:span, tab[1]), "##{tab[0]}") )
      end
    end
  end

  def  render_bodies
    @tabs.collect do |tab|
      content_tag(:div, capture(&tab[3]), tab[2].merge(:id => tab[0]))
    end.to_s
  end

  def method_missing(*args, &block)
    @template.send(*args, &block)
  end

end
```

Now, include your tab assets and initialize your tabs as usual. TabsRenderer assumes that you want to ID your tab's div as 'tabs'.

  ``` html
  <% javascript_include_tag("tabs/ui.tabs.pack.js") %>
  <% stylesheet_link_tag("tabs/ui.tabs.css") %>

  <script type="text/javascript" charset="utf-8">
      $(function(){
          $('#tabs > ul').tabs();
      });
  </script>
  ```

Now use TabsRenderer to create some tabs ...notice how you can put conditions on the tail end of the block. We pass 'self' to our helper so that it knows where to delegate method calls that aren't it's own. Creating a tab is as simple as specifying a div #id and an Tab title.

  ``` erb
  <% tabs = TabsRenderer.new(self) %>

      <% tabs.create('basic_tab', 'Basic Info') do %>
          # ...
      <% end %>

      <% tabs.create('gallery_tab', 'Gallery') do %>
          # ...
      <% end unless @images.blank? %>

      <% tabs.create('admin_tab', 'Admin') do %>
          <%= render :partial => "super_secret_stuff" %>
      <% end if current_user.has_role? 'admin' %>

  <%= tabs.render %>
  ```

And here is the code that would be generated from the above example.

  ``` html
  <div id="tabs">
      <ul>
          <li><a href="#basic_tab"><span>Basic Info</span></a></li>
          <li><a href="#gallery_tab"><span>Gallery</span></a></li>
          <li><a href="#admin_tab"><span>Admin</span></a></li>
      </ul>
      <div id="basic_tab">
          # ...
      </div>
      <div id="gallery_tab">
          # ...
      </div>
      <div id="admin_tab">
          # ... rendered partial here
      </div>
  </div>
  ```

Hope this is of use to someone else! Its sure been useful to me. I would love to hear your comments and criticisms. Please feel free to request changes to the class.

ps. Rails folks wanting to use JQuery in their projects should check out [jrails](http://ennerchi.com/projects/jrails), it wraps a lot of the prototype helpers.

pps. [JQuery](http://jquery.com/) Rocks@!
