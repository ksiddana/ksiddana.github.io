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

#####userModel.js
{% highlight javascript %}
//User.js Mongoose Model

var mongoose = require('mongoose');

var schema = new mongoose.Schema({
  username : { type : String },
  message : { type : String }
});

module.exports = mongoose.model('User', schema);
{% endhighlight %}


#####userController.js
{% highlight javascript %}
var User = require('../models/User.js')

exports.findByUserName = function (name, callback) {
  User.findOne({ username : name}, callback);
};
{% endhighlight %}

#####userControllerTest.js
{% highlight javascript %}
var expect = require('chai').expect;
var mongoose = require('mongoose');
var User = require('../models/User');
var UserController = require('./userController.js')

  it('should find the user based on the name', function (done) {
      
      UserController.findByUserName('John', function(err, data) {
        expect(data.message).to.equal('Learning Module Exports');
        done();
      });

  });

  {% endhighlight %}