---
title: "Form builders in Rails: field names and ids for Javascript"
date: 2010-09-27 03:50
comments: true
categories: Rails
---

I came up with a few convenience methods today which will help you discover what Rail's generated NAME and ID attributes will be for a specific field on a given form builder object. Actually, I didn't come up with them so much as extract them from the built in form builder. It was useful enough to me that I thought I might share my discovery. I can't tell you how many times I've lazily hardcoded an ID onto a form element just to bypass the defaults.

Throw these 2 methods in your ApplicationHelper module:

~~~ ruby
def field_id_for_js(builder, attribute)
  "#{builder.object_name}[#{attribute.to_s.sub(/\?$/,"")}]".gsub(/\]\[|[^-a-zA-Z0-9:.]/, "_").sub(/_$/, "")
end

def field_name_for_js(builder, attribute)
  "#{builder.object_name}[#{attribute.to_s.sub(/\?$/,"")}]"
end
~~~

Here is an example using Rails nested attribute forms:

~~~ erb
<%= form_for(@submission) do |f| %>
  <%= f.fields_for :project, @submission.project do |builder| %>

    <div class="field">
      <%= builder.label :classified, "Classified?" %><br/>
      <%= builder.radio_button :classified, false %> <%= builder.label :classified_false, "No" %>
      <%= builder.radio_button :classified, true %>  <%= builder.label :classified_true, "Yes" %>
    </div>

    <%= content_for(:head) do %>
      <script>
        $(function() {
          // get our value by name
          $("input[name='<%= field_name_for_js(builder, :classified) %>']").val();
          // set our value by ID
          $("#<%= field_id_for_js(builder, :classified_true) %>").attr('checked', true);
        });
      </script>
    <% end %>

  <% end %>
<% end %>
~~~

Would output:

~~~ html
<script>
  $(function() {
    // get our value by name
    $("input[name='submission[project_attributes][classified]']").val();
    // set our value by ID
    $("#submission_project_attributes_classified_true").attr('checked', true);
  });
</script>
~~~

In case you missed it ...

~~~ erb
<%= field_name_for_js(builder, :classified) %>
<%= field_id_for_js(builder, :classified_true) %>
~~~

... produced ...

~~~
submission[project_attributes][classified]
submission_project_attributes_classified_true
~~~

Pretty handy stuff
