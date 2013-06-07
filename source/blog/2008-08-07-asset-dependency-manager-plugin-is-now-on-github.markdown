---
title: "Asset Dependency Manager plugin is now on GitHub"
date: 2008-08-07 09:24
comments: true
categories: Rails
---

I use [jQuery UI](http://ui.jquery.com/) a lot, but I don't use all of it and I prefer not to install the entire library if I can help it. Its not enough to just manage the JS assets you need for a particular widget, you often have to juggle inclusion of its CSS as well. So, rather than separating out the plugins I use on each project, I thought it would be cooler (in development at least) to leave them as smaller files and just find a better way to manage which assets were needed, when they were needed!

In an effort to eliminate some development headache I've created Asset Dependency Manager. You can download it here:

[http://github.com/CodeOfficer/asset-dependency-manager/tree/master](http://github.com/CodeOfficer/asset-dependency-manager/tree/master)

Basically, it lets you declare dependency lookups in your application_helper.rb file ... Then in your views you say something akin to ... assets_for :tabs, :slider, :etc ... and the resources for said lookups with be included in your layout view auto-magically, removing duplicates and paying strict attention to your intended loading order.

The README has more info on how to use it. Let me know if you find it useful or have feature requests.

And if its of interest, have a look at my other Rails helper for JQuery UI called [TabsRenderer](http://github.com/CodeOfficer/jquery-ui-rails-helpers/tree/master)
