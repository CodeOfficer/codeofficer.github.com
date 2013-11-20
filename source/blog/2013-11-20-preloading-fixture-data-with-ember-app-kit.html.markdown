---
title: Preloading Fixture Data with Ember App Kit
date: 2013-11-20 16:11 EST
comments: true
tags: Ember Javascript
---

I'm currently using [Ember App Kit](https://github.com/stefanpenner/ember-app-kit) on a client project and really enjoying the environment. Javascript build tools are what they are but Stefan Penner and crew have done a phenomenal job at making them not suck. I am ever so grateful.

Application needs vary of course, but mine delivers 85% of its data up front in one big payload. It's safe for me to assume in production that this data will be there when the app is run, and data that isn't will be fetched as needed via the RESTAdapter. In development and testing however, I'm using the FixtureAdapter which unfortunately doesn't autoload non-async relationships defined in the model, even though the fixture data is there. I needed this data to behave in development and testing as it would in production. Fortunately, I stumbled upon [a blog post](http://amalgamated.io/emberjs-and-fixtures/) over at **Amalgamated Magic**.

Below, I've made their solution work for Ember App Kit. I believe there are plans to make EAK's resolver better at loading initializers for you, but for now this works.

In `app/initializers/force_load_fixtures`:

``` javascript
var initializer = {
  name: "force load fixtures",
  after: "store",
  initialize: function(container, application) {
    var store = container.lookup('store:main');

    Ember.keys(requirejs._eak_seen).filter(function(key) {
      return !!key.match(/^appkit\/models\//) && DS.Model.detect(require(key));
    }).map(function(key){
      var type = require(key);
      var typeKey = key.match(/^appkit\/models\/(.*)/)[1];
      if (type.FIXTURES) {
        store.pushMany(typeKey, type.FIXTURES);
      }
    });
  }
};

export default initializer;
```

Then in `app.js` load and execute our initializer:

``` javascript
import forceLoadFixturesInitializer from 'appkit/initializers/force_load_fixtures';

var App = Ember.Application.extend();

Ember.Application.initializer(forceLoadFixturesInitializer);

export default App;
```

Enjoy.
