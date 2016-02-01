---
layout: post
title: Module Exports with Node.js
date: 2016-01-31 16:00:00
---

###Understanding exports in Node.js

As an application begins to scale, the complexity can be reduced by modularizing your code in a way that it can be comprehended by other users. MVC frameworks like Backbone.js have already introduced cocepts of Models, Collections and Views on the front end.

Node allows you to modularize your code at the backend by using `module.exports`. 



A file in Node is called a module. Functions and variables within a file cannot be accessed directly by using inheritence or dependency injection. However, it would be rather useful to expose the functionality of a file while keeping it modular at the same time. 

#####The Module Object

Module is a reference to the object representing the current module or file. `module.exports` is property that is accessible via the exports module-global.

#####module.exports
In order to export the functions and variables of a file with the function and a variable as shown below in example 1 we can use the `module.exports` object.

####Example 1

{% highlight javascript %}
// mathFunctions.js
var number = 5;

var add = function(value) {
  return value + number;
};

var power = function(base, exponent) {
  return base * exponent;
}

// use the module.exports object to expose the functions to another module.
module.exports.number = number;
module.exports.add = add;
module.exports.power = power;

// Alternate way to export the properties which is more commonly used
module.exports = {
  number : number,
  add : add,
  power : power
}
{% endhighlight %}

Now to load the mathFunctions.js module into another module we can use the `require` statement. So for instance if we wanted to use add2 in another file we would write the following code.

{% highlight javascript %}
// function2.js
var math = require('./mathFunctions.js')

console.log("Adding %d to 10 gives us %d", math.number, math.add(10));
{% endhighlight %}

####Example 2

In the example shown below, there is a user Model with a `userSchema` that uses a userController and a Test that can be written for unit test purposes.

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