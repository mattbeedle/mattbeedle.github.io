---
layout: post
title: "Ember Data ISO Date Transform"
category: javascript
description: "How to define a ISO Date Transform in Ember Data"
keywords:
    - Ember Data
    - Ember Data Transform
    - Ember Data DateTime
    - Javascript
    - ISO Date
---

Most of my EmberJS projects use a datetime field somewhere on the backend.
Rather than use string attributes on the client side, it's nice to transform
those values to javascript date objects. This can be done easily using
an [Ember Data Transform](http://emberjs.com/api/data/classes/DS.Transform.html)
and [MomentJS](http://momentjs.com)

{% highlight bash %}
bower install --save moment
{% endhighlight %}

```app/transforms/isodate.coffee```
{% highlight coffeescript %}
`import DS from 'ember-data'`

IsodateTransform = DS.Transform.extend
  deserialize: (serialized) ->
    if serialized
      return moment(serialized).toDate()
    return serialized

  serialize: (deserialized) ->
    if deserialized
      if deserialized instanceof Date
        return moment(deserialized).toISOString()
      else
        # I'm using the European date format here. US readers may be able to
        # leave this second argument out
        return moment(deserialized, 'DD/MM/YY h:mm a').toISOString()
    return deserialized

`export default IsodateTransform`
{% endhighlight %}


Now in any models where it is needed, you can use your isodate transform.

```app/models/user.coffee```
{% highlight coffeescript %}
`import DS from 'ember-data'`

User = DS.Model.extend
  createdAt: DS.attr('isodate')

`export default User`
{% endhighlight %}