---
title: "acts_as_fulltext_indexed and geokit"
date: 2008-07-01 09:27
comments: true
tags: Rails
---

I created a [quick fork](http://github.com/CodeOfficer/acts_as_fulltext_indexed/tree/master) on github of the [acts_as_fulltext_indexed](http://blog.antiarc.net/2007/05/01/introducing-acts_as_fulltext_indexed/) plugin made by [Chris Heald](http://blog.antiarc.net/) of antiarc.net.

Acts_as_fulltext_indexed addresses a limitation of MySQL's [INNODB table format](http://dev.mysql.com/doc/refman/5.0/en/innodb.html) (which supports [transactions](http://en.wikipedia.org/wiki/Database_transaction) but not [fulltext indices](http://dev.mysql.com/doc/refman/5.0/en/fulltext-search.html)) by using MySQL's [MYISAM table format](http://dev.mysql.com/doc/refman/5.0/en/myisam-storage-engine.html) (which supports fulltext indices, but not transactions).

Acts_as_fulltext_indexed solves this issue by creating a MYISAM table just for fulltext indexed searching that is separate from your regular INNODB tables. A link back to your main model is created polymorphically, and your models have automatic hooks that create and update these indices on after_save.

My fork simply lets you pass in two additional finder options that [Geokit](http://geokit.rubyforge.org/) requires, if you are also using Geokit. You would use it as such:

~~~ ruby
Model.search( params[:search_terms],
{
    :origin => etc,
    :within => etc,
    :conditions => etc,
    :order => etc,
    :limit => etc
  }
)
~~~

[refer to the original read me here](http://blog.antiarc.net/2007/05/01/introducing-acts_as_fulltext_indexed/)


[my fork is at](http://github.com/CodeOfficer/acts_as_fulltext_indexed/tree/master)
