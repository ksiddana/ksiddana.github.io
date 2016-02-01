---
layout: post
title: Module Exports with Node.js
date: 2016-01-31 16:00:00
---

###Understanding exports in Node.js

As your application begins grow and you are retrieving data from a database like Mongo and adding more functionality, your going to see the need to seperate your model from your database calls within a node module. 

Node allows you to modularize your code by using `module.exports` statements. Let's get into what are module.exports statements.

###Module Export Statments

All functions within a 


{% highlight javascript %}
//User.js Mongoose Model

var mongoose = require('mongoose');

var schema = new mongoose.Schema({
  username : { type : String },
  message : { type : String }
});

module.exports = mongoose.model('User', schema);
{% endhighlight %}