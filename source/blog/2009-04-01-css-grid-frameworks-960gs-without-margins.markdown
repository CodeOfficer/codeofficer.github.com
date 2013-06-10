---
title: "CSS Grid Frameworks: 960gs Without Margins"
date: 2009-04-01 02:39
comments: true
tags: CSS
---

I've been using the [Blueprint](http://www.blueprintcss.org/) and [960gs](http://960.gs/) CSS Frameworks for a while now and have had but one complaint: they both assume you want to create your layouts with margins between the columns. Unfortunately, in my own use of these frameworks I've found that I work more with inner columns than exterior ones, and the default margin styles were a nuisance then.

So ... I forked [Nathan Smith's 960gs](http://github.com/nathansmith/960-grid-system/) and recalculated the grid styling to remove the column margins. Now when I declare any two columns side by side, they will be flush against each other. Perhaps others will find this useful as well. I left the reset.css and text.css styles as they were.

You can find my code [here](http://github.com/CodeOfficer/960-grid-system-without-margins/).

One final note: I've been using 960gs a lot more than Blueprint lately because I've found Blueprints text styles make too many assumptions on my behalf for how I would like my text to layout. 960gs in comparison is quite minimal.

![960 Grid](/images/mine/960cssgrid.gif)
