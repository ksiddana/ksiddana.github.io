---
layout: post
title: Backbone Introduction
---

Your probably wondering, why do we have to write so much code for displaying HTML elements that don't currently have a feature rich component. These are called single page applications. Backbone consists of building web pages using the following architectures:

- Models
- Views
- Collections
- Routers

In order to undersand the ease with which Backbone lets you render your page, you have to have built a large project using jQuery that eventually becomes difficult to manage. Backbone allows developers to write feature rich code that can similarly be written in jQuery in addition to keeping your code organized. 

I personally took a long time to understand the use of Backbone for Single Page Application due to its structure. It's specially helpful when your using underscore on a regular basis. jQuery can provide you with the same feature rich functionality, however as the project becomes larger it can soon become sluggish.

Refer to the following links to guide you through a beginners introduction to Backbone.

1. [The Single Page Mindset](https://singlepagebook.supportbee.com/chapters/single-page-mindset/)
2. [Backbone Tutorials](https://backbonetutorials.com/)
3. [Thomas Davis - Why would you use Backbone](https://cdnjs.com/libraries/backbone.js/tutorials/why-would-you-use-backbone)

###Section 01###

I'm going to start of with a simple code block for a Backbone Model with a class **Person**.
{% highlight javascript %}
var Person = Backbone.Model.extend({
  
  // Define default attributes on the Model
  defaults: {
    name: 'Brian Williams',
    age: 30,
    occupation: 'Web Developer'
  },

  // Define a method called work
  work: function() {
    return this.get('name') + ' is working.';
  },
  
  // Special function provided by Backbone, lets you check the validity of its
  // attributes
  validate: function(attrs) {

    if (attrs.age < 0) {
      return 'Age must be positive';
    }
  }

});
{% endhighlight %}

A **View** allows you to listen to events from the DOM and also render events to the DOM. In this case we want to render the attributes of the instance of Person to the DOM.
{% highlight javascript %}
var PersonView = Backbone.View.extend({
  
  // PersonView will use a <li> HTML tag
  tagName: 'li',
  
  // Define the <li> with a class of person
  className: 'person',
  
  // Use the template feature of backbone to render the attributes
  template: _.template("<%= name %> (<% age %>) - <%= occupation %>"),
  
  // When the PersonView is created, the initialize function is run at least once.
  initalize: function() {

  },
  
  // Renders the model on the DOM
  render: function() {  
    this.$el.html(this.model.get('name') + '(' + this.model.get('age') + ')');
    return this;
  }

});
{% endhighlight %}

The code simply above will not render anything on the DOM, since we have not called the *render* method. We need to create an instance of **Person** and an instance of **PersonView** and append the data from **Person** to the DOM using the **PersonView** instance.

{% highlight html %}

<!DOCTYPE html>
<html>
<body>
    <head>
       <link rel="stylesheet" href="bower_components/bootstrap.css">
   </head>
    <script src="bower_components/jquery/jquery.js"></script>
    <script src="bower_components/underscore/underscore.js"></script>
    <script src="bower_components/backbone/backbone.js"></script>

   <script id="personTemplate" type="text/template">
      <%= name %> (<%= age %>) - <%= occupation %>
   </script>
    
    <div class="tasks">
      <script id="taskTemplate" type="text/template">
        <span><%= title %></span> <button class="edit">Edit</button> <button class="delete">Delete</button>
      </script>
    </div>
    <script>
    var person = new Person;
    var personView = new PersonView({ model: person });
    personView.render();
    $(document.body).html(personView.el)
    </script>


    
    <script src="Person-01.js"></script>
    <!-- // <script src="Person-02.js"></script> -->
    <!-- // <script src="Person-03.js"></script> -->
    <!-- // <script src="Person-04.js"></script> -->

</body>
</html>
{% endhighlight %}

Notice how person and personView have been instantiated. Instance personView is of type PersonView that takes in the Backbone Model *person*.

###Section 02###

Now lets talk about collections, a collection is a filtered list of people that you would like to view on the DOM, from your data model. In case you invoke a filter function that only wants to see the people older than 21 you would want to refer to that list as a collection that is rendered to the DOM using a Backbone Collection View.

In the example above, we have made one person and rendered them on to the DOM, in the case that we want to put a list of people on the DOM, we can create a Collection which is essentially an Array of models and render them on the DOM. We will also need to create a View for the Collection and invoke the method for rendering the list of people on the DOM. The list could be the entire list of people or a subset of the list. 

{% highlight javascript %}
// Create a list of People that takes in a model of Person.

var PeopleCollection = Backbone.Collection.extend({ model: Person })

// Instead of adding people individually to the PeopleCollection, we are going to 
// add people in JSON format, just as we may expect from an external API. Variable
// peopleCollection is an Array of JSON Objects.

var peopleCollection = new PeopleCollection([
  {
    name: 'William Doe',
    age: 35,
    occupation: 'Product Manager'
  },
  {
    name: 'John Doe',
    age: 30,
    occupation: 'Web Designer'
  },
  {
    name : 'Sally Doe',
    age: 20,
    occupation: 'Graphics Designer'
  }
]);
{% endhighlight %}

Now lets create a View for the Collection and render the list of People on the DOM. In order to stay modular, you will notice, the data does not change a lot, however, we may have several Views, using the same data, but manupilating different parts of the DOM. Essentially, you have a single model, and single collection but several views, rendering different parts of the DOM.

{% highlight javascript %}
var PeopleView = Backbone.View.extend({
  
  tagName: 'ul',

  initalize: function() {
  },

  render: function() {
    
    // filter through all items in a collection
    this.collection.each(function(person) {
      
      // for each model/person, create a new Person View
      var personView = new PersonView({ model : person });
      
      // append to root element
      this.$el.append(personView.render().el);
      
    }, this);

    return this;
  }
});
{% endhighlight %}

Use the Chrome Console to navigate the attributes of Person, PersonView and PeopleCollection. Again, an instance of the PeopleView has not been created, and the render function has also not been invoked, therefore this code above alone will not run. Use the HTML code below or create instances in the Chrome Developer's console to invoke the render method on JSON data.

{% highlight HTML %}
<!DOCTYPE html>
<html>
<body>
    <head>
       <link rel="stylesheet" href="bower_components/bootstrap.css">
   </head>
    <script src="bower_components/jquery/jquery.js"></script>
    <script src="bower_components/underscore/underscore.js"></script>
    <script src="bower_components/backbone/backbone.js"></script>

   <script id="personTemplate" type="text/template">
      <%= name %> (<%= age %>) - <%= occupation %>
   </script>
    
    <div class="tasks">
      <script id="taskTemplate" type="text/template">
        <span><%= title %></span> <button class="edit">Edit</button> <button class="delete">Delete</button>
      </script>
    </div>
    <script>
      
      <!-- Create an instance of Person as a Backbone Model -->
      var person = new Person;
      
      <!-- Create a View for the Person as a Backbone View -->
      var personView = new PersonView({ model: person });
      
      <!-- Create a View for the list of People as Backbone Collection -->
      var peopleView = new PeopleView( { collection : peopleCollection });
      
      <!-- Check the DOM node of peopleView before rendering -->
      peopleView.el;
      
      peopleView.render();
      
      <!-- Check the DOM node of peopleView after rendering -->
      peopleView.el;
      
      <!-- Append the people View nodes to the DOM -->
      $(document.body).append(peopleView.el);
      
    </script>
    
    <!-- // <script src="Person-01.js"></script> -->
    <script src="Person-02.js"></script>
    <!-- // <script src="Person-03.js"></script> -->
    <!-- // <script src="Person-04.js"></script> -->

</body>
</html>
{% endhighlight %}