---
title: "My Poor Use of Git Commit Messages"
date: 2008-08-28 09:20
comments: true
categories: Git
---

I began using git to manage the source code for my current project, a Rails app which I should hopefully be launching shortly. Yahoo! Now, I hadn't actually been using source control for very long before I began, and wasn't able to focus on best practices. With a wife, 2 kids and 2 jobs, I was lucky to get the coding of the apps themselves under control. That said, its funny now to look back at all my commit messages for this project.

So it wasn't until recently I was able to focus my commits to specific features, changes to the code-base that stood on their own. More often, I found myself working "everywhere" and on "everything" all at once. I wonder how many other people start off this way.

Well, 127 commits behind me, I took a moment to browse back through the messages of my project. Its amazing to see how useless they are, time to get better at it I guess.

* initial rails import
* first rails 2.1 import
* adding in the input_css plugin
* configuring for a git deploy
* some deploy edits
* removing fast sessions
* removing vendor/rails
* adding back in rails 2.1RC
* fixed categorizable
* testing
* in progress
* tagging works as best it can now until better searching is implemented
* before trying permalink_fu
* removing tm project file
* annotating models
* ajaxified some pagination and refactored page_titles and will_paginate helpers
* retooling index views on my resources
* just before changing categories to be acts_as_list
* massive changes to how we store categories, now a list and not a tree
* simplifying event searches
* starting to add thumbnail support to listings
* most image thumbs appear as expected
* cached category list to db, fixes to admin and public pages in regards to ajaxification of pagination
* refactoring my tabs renderer
* working on what can be deleted and its consequences
* removing deleted_at from affiliates
* adding in a fixtures dump task
* renaming the rake task
* refactored affiliates and listings
* altering the display of list view for events and such
* before meeting
* adding in the textmate footnotes plugin
* bug fix in lib/mappable.rb
* new tabs_renderer, accepts options now on the main div
* removing rails 2.1 RC
* last minute adds
* adding rails 2.1 offical
* adding in ultrashinx, about to take out rails 2.1 final and put in edge to get rid of depreciation warnings
* removed rails 2.1
* rails edge for 2.1+ frozen
* updated gitignore
* gonna play with ez-where
* removing ultrasphinx, adding ez_where
* removing aatos
* LOTTA STUFF - EEEEEEK
* nuttin
* before trying a fulltext solution
* mid stride with ez_where and tsearch
* about to go back to mysql!!!! >.<
* before revamping categories again!
* caring for old deletions
* categories now work via categorizations!!!
* annotated my models
* spacial search, pagination, and conditional finds working well
* now using ez::where
* mostly removed old search
* getting there!!!
* closer!!!!
* well i thought i was getting somewhere
* many to many with users-affiliates
* slowly getting there .. hope i am ready by tuesday
* mid stride ... as usual
* lambda in perm check
* quick fix?
* disabling perm checks
* broken as all hell
* added new jrails
* on my way
* before removing acts_as_paranoid
* replaced acts_as_paranoid
* added scope builder, removing the admin namespace slowly
* temp commit ... nothing special
* updating model annotations
* changing the deploy file
* test
* large overhaul to partials etc
* securing the approvals controller
* correcting a find more by affiliate/user link issue
* done refactoring partials, except maybe for locations and attachments
* adding an indexing rake task for deployment
* small edits
* fixing listings views with affiliates
* tag 'listings' are not more specific
* highlighting for section and category navigation
* nothing
* adding css to space out forms
* 1st round of changes
* no really, 1st round ..... >.<
* removing query trace plugin
* 20 of 50 changes
* 20 more taken care of
* removing most dumb files
* adding full address abilities to user affiliate job even classified
* same as before
* adding in addresses
* working with addresses
* removing sti on addresses
* removing billing and shipping formats
* yo mamma
* added status notifications to listings#show too
* lots of changes, biggest was switching attachment_fu to only use core_image for resizing
* fix the comments view, forgot a div end
* removing attachement_fu's test db
* tweaking affiliate and user show pages
* jobs are now 'available' or 'wanted'
* jobs are now fully defined as wanted or available
* nothing much
* altering user perms
* removing some files
* added in my asset manager plugin (gulp)
* removing jrails
* removing input css
* annotating models
* moving things
* adding new date and time formats, a few other initializers
* re-arranging helpers for file attachments
* adding thickbox dependencies for a few view helpers
* attachment validations still arent working >.< WTF!
* adding sensible page titles
* some view edits for how minical icons display in the events section
* small perm check on users display
* basic history of listings
* lots of permissions changes
* commit before i play with superfish
* added superfish
* adding new user tab info
* bout to get serious!!!
* removing vendor/query_analyzer
* updating some tabs helpers
* does this work?
* fixed the user_tab parial
* added the ability for a user to change their default address
* corrected a bug in making an address default for a user
