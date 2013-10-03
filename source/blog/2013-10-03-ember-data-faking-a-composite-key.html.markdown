---
title: "Ember Data: Faking a Composite Key"
date: 2013-10-03 15:11 EST
comments: true
tags: Ember Javascript
---

I play a game in my spare time called Guild Wars 2 which recently made a public API for their community to build applications against. Among other things, it allows you to query for game events as they are happening in real time, which helps you play more efficiently. I started working on a little side project which takes it a step further and draws you a map of the surrounding area. It's a work in progress, but you can [check it out](https://github.com/CodeOfficer/guild-wars-2-event-notifier) on Github.

I've been working with [Ember](http://emberjs.com/) and [Ember Data](https://github.com/emberjs/data) for a little over a year now and use this project as a study to keep up on their latest and greatest features. I can't say enough good things about Ember as a framework for javascript web applications, but this post is really about Ember Data.

While Ember Data has had its growing pains as a persistance library it's shaping up quite nicely. I like how the new Beta's adapter and serializer allow you to customize data coming into and going out of your app on a per-model basis. Particularly when building for a 3rd party API, you really need proper hooks to fine-tune the data exchange with your RESTful backend.

In earlier versions of Ember Data I found myself having to copy/paste/edit sizable chunks of library code into my own custom adapter or serializer. The necessary hooks were not present. This time around, I'm finding hooks in all the right places.

Straight from my project, here's an example customization I did for the MapFloor model:

### MapFloor Model

The json for the MapFloor endpoint does not return an `id` attribute. If you insert a model into the store without an 'id', the store will not know how to uniquely identiy that model from others of that type. One solution is to sythensize your own 'id' in the returned json. This is something you would usually do in the serializer for the model.

Complicating matters, the MapFloor endpoint also requires two `ids` to fetch a single MapFloor, composed of a `continent_id` and a `floor` ... yet the json reponse does not return those values at all.

To clarify, this is what I need to hit:

```
https://api.guildwars2.com/v1/map_floor?continent_id=1&floor=2
```

And this is what I get back:

``` javascript
// notice there is no 'id' in this payload
{
  "texture_dims": [
    32768,
    32768
  ],
  "regions": {
    // lots of data map floor data
    }
  }
}
```

I felt I really wanted to be able to do something like:

``` javascript
store.find('mapFloor', '1.2');
```

Where `1.2` would expand to to the appropriate endpoint URL. My changes seemed appropriate for the adapter, especially since I also wanted to pass those values back to the serializer. This is the custom adapter I ended up with for my MapFloor model.

``` javascript
App.MapFloorAdapter = App.ApplicationAdapter.extend({

  parseId: function(id) {
    var values = id.split('.');
    return {
      continent_id: values[0],
      floor_id: values[1]
    };
  },

  buildURL: function(type, id) {
      var parseId = this.parseId(id);
      var params = "?continent_id=%@&floor=%@".fmt(parseId['continent_id'], parseId['floor_id']);
    return this._super(type) + params;
  },

  find: function(store, type, id) {
    var parseId = this.parseId(id);
    return this.ajax(this.buildURL(type.typeKey, id), 'GET').then(function(json){
      json.id = id;
      json.continent_id = parseId['continent_id'];
      json.floor_id = parseId['floor_id'];
      return json;
    });
  },

  pathForType: function(type) {
    return Ember.String.underscore(type);
  }

});
```

My custom implementation of `find` parses my fake composite `id` and remembers its `continent_id` and `floor_id` values. Those values are used to construct the endpoint URL and also set values on the returned json. All this will later get handed off to the serializer and because the payload now includes a sythesized `id` the store will be happy and able to uniquely identify this specific MapFloor in the future.

Lastly, I overrode `pathForType` since I needed to hit a singularized version of the resource URL.

Hope you find this approach to faking a composite key in Ember Data useful.

