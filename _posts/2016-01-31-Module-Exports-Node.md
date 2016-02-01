---
layout: post
title: Module Exports with Node.js
date: 2016-01-31 16:00:00
---

###Understanding exports in Node.js

As an application begins to scale, the complexity leads to modularizing your code in a way that it can be comprehended by other users. MVC frameworks like Backbone have already introduced us to cocepts of Models, Collections and Views on the front end.

Node allows you to modularize your code at the backend by using `module.exports`. In the example shown below, there is a user Model with a `userSchema` that uses a userController and a Test that can be written for unit test purposes.

###Module Export Statments

#####userModel.js
{% highlight javascript %}
// userModel.js Mongoose Model

var mongoose = require('mongoose');

var schema = new mongoose.Schema({
  username : { type : String },
  message : { type : String }
});

module.exports = mongoose.model('User', schema);
{% endhighlight %}


#####userController.js
{% highlight javascript %}
// userController.js
var User = require('../models/User.js')

exports.findByUserName = function (name, callback) {
  User.findOne({ username : name}, callback);
};
{% endhighlight %}

#####userControllerTest.js
{% highlight javascript %}
// userControllerTest.js
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