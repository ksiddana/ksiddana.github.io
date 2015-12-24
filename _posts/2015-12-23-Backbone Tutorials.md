---
layout: post
comments: true
title: Backbone Soundcloud Example
---

Initially I would say, start off by building small applications in Backbone, such as a an API, that is similar to a jQuery AJAX call to an external API. In this example I am going to walk you through some sample code to get you started by building a Single Page Application that brings up the Titles from your Soundcloud Account. 

If you don't already have a [sound cloud developers Client ID](https://developers.soundcloud.com/docs/api/guide), you can get that by referring to their Developers API Guide.

###Step 1###

Lets define our Backbone Model for a single **Track**. Since we are getting JSON data back from the API, there is no need to setup any defaults. 
{% highlight javascript %}
// Define the Backbone Model for a Single Track
App.Models.Track = Backbone.Model.extend({});

// Define the Backbone Model for several tracks or a collection of Tracks
App.Models.Tracks = Backbone.Model.extend({
  
  // The Collection takes in a model of type Track
  model : App.Models.Track,
  
  // Fetch the data from the external API end point using your own clientID
  url: 'https://api.soundcloud.com/tracks?client_id=',
  
  // Define a function that invokes the fetch method
  loadMessages: function() {
    this.fetch({});
  }
});
{% endhighlight %}

###Step 2###

Now that we have our Backbone Models defined, lets make some views to render the data to the Page. Create a View for the Track Model. This View consists of the template which lays out how we would like to define our tracks to be displayed. I am not doing anything to complicated, I'm using Backbone's template engine to define a span that contains the title and has a class of the same name.

{% highlight javascript %}
// Define a View for a Single Track
App.Views.Track = Backbone.View.extend({
  
  // Define a tempalate for the title in a span element
  template: _.template('<span class="title"><%- title %></span>'),
  
  // Define the initialize function incase you want to see the value of this.
  initialize: function() {
    console.log(this.model);
  },
  
  // Define the events object, that is going to take in button click from
  // Display.
  events: {
    'click .display' : 'render'
  },
  
  // Render the information on the DOM.
  render: function() {
    this.$el.html( this.template(this.model) );
    return this.$el;
  }
});
{% endhighlight %}

###Step 3###

Now, lets go ahead and define a view from our collection of Tracks. Remember that a Backbone Collection is an array containing a single track model. Currently I'm not doing anything too complicated, like sorting the tracks according to popularity and playcounts, but once you get the data and render it on the page, you can certainly make more functions that listen for those and render them on the page as so.

{% highlight javascript %}
// Define a Backbone Collection View for the Tracks
App.Views.Tracks = Backbone.View.extend({
  
  // Define the initialize function, that listens for a 'sync' method from the
  // fetch method.
  initialize: function() {
    console.log(this);
    this.collection.on('sync', this.render, this);
  },
  
  events: {
    'click .display' : 'render'
  },
  
  // Using the _.each function we can iterate over the collection and 
  // pass in a single model/track to the renderTracks method.
  render: function() {
    _.each(this.collection.attributes, this.renderTracks, this);
  },
  
  // Define the method, that makes a view of each track and renders it
  // We then want to append that element to the DOM.
  renderTracks: function(track) {
    var trackView = new App.Views.Track({model : track})
    var $html = trackView.render();
    $('#soundCloud').append($html);
  }
});
{% endhighlight %}

###Step 4###

We're going to render the Tracks to the HTML element DIV with an id of *soundCloud*. Make sure you include the config.js file in the .gitignore file, so that the API clientID can be kept local for each user. 

{% highlight HTML %}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Soundcloud</title>
  <link rel="stylesheet" href="styles/styles.css">

  <!-- dependencies -->
  <script src="bower_components/jquery/jquery.min.js"></script>
  <script src="bower_components/underscore/underscore-min.js"></script>
  <script src="bower_components/backbone/backbone.js"></script>
  <script src="env/config.js"></script>
  <script src="scripts/backbone-solution-01.js"></script>
</head>
<body>

    <h1>Displaying Sound Cloud Data</h1>
    

    <button class='display'>Display</button>
    <div id="soundCloud"></div>

</body>
</html>
{% endhighlight %}

###Step 5###

We haven't created an instances of the Track Collection or the Views. Before going forward, I want to let you know, I usually wrap my entire code in a self invoking function as demonstrated below. It allows me to keep my code organized and keep my naming conventions consistent. In addition I make the App available to the window, so I can debug it using the Chrome Developer Tools.
{% highlight javascript %}

(function() {

// Define an object App, that will take in objects Models, Views and
// Collections.
window.App = {
  Models: {},
  Views: {},
  Collections: {}
};

... // code from step 1
... // code from step 2
... // code from step 3

})();

// Define an instance of tracks 
window.tracks = new App.Models.Tracks();

// Load the messages into tracks
tracks.loadMessages();

// Define an instance of the Tracks View and pass the tracks object as a
// collection to the App
window.tracksView = new App.Views.Tracks({collection : tracks})

{% endhighlight %}
