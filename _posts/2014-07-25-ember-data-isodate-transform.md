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
Reading them from the API is easy using a 'date' attribute type, but this
doesn't serialize to a very API friendly format. For example:

```javascript
new Date().toString()
"Wed Aug 06 2014 12:39:29 GMT+0200 (CEST)"
```

My API expects
to receive date times in [ISO 8601
format](http://en.wikipedia.org/wiki/ISO_8601). We can easily achieve this using
[MomentJS](http://momentjs.com) and an
an [Ember Data
Transform](http://emberjs.com/api/data/classes/DS.Transform.html).

{% highlight bash %}
bower install --save moment
{% endhighlight %}

```app/transforms/isodate.coffee```
{% highlight coffeescript %}
`import DS from 'ember-data'`

IsodateTransform = DS.Transform.extend
  # The deserialize function just transforms the string it receives from the
  # server JSON response into a date.
  deserialize: (serialized) ->
    if serialized
      moment(serialized).toDate()

  # The serialize function takes the value of the attribute on the model and
  # transforms it into a nice ISO date to send to our server.
  serialize: (deserialized) ->
    if deserialized
      if deserialized instanceof Date
        moment(deserialized).toISOString()
      else
        # I'm using the European date format here. This is how date strings are
        # set on my models by my datetimepicker inputs. US readers may be able
        # to leave this second argument out
        moment(deserialized, 'DD/MM/YY h:mm a').toISOString()

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