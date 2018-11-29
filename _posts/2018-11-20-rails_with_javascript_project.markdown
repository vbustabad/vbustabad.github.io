---
layout: post
title:      "Rails with Javascript Project"
date:       2018-11-20 13:06:19 -0500
permalink:  rails_with_javascript_project
---

I will dedicate this blog post to explaining some Javascript fundamentals as it relates to the Rails with Javascript project. As set forth in the project requirements, I added some dynamic features to the previous Rails application using Javascript. 

The first couple of features that I added were a link and a button to render some information regarding dogs. The link sent a fetch request to the Wikipedia API in order to retrieve a description of the breed and append it to the page. The button was designed for the dogs index page which shows a list of the breeds for all of the dogs that are available at the shelter. 

Firstly, I created some HTML elements as containers where the Javascript code could be added. This would allow the Javascript to be responsible for rendering the front-end of these features for the application. I selected the HTML elements by using the getElementbyId method of regular Javascript which allows you to select an HTML element by its id. I added event listeners using addEventListener which enables the dynamic functionality that you build for the link or button once it is clicked on by the user. See the code below:

*Dogs Show View*

```
<h1> Dog Information </h1>

<p><%= @dog.name %></p>
<p>Age: <%= @dog.age %></p>
<p>Breed: <span id="dog-breed" data-breed="<%= @dog.breed %>"><%= @dog.breed %></span></p>
<p>Gender <%= @dog.gender %></p>
<p>Weight: <%= @dog.weight %></p>
<p>Adopted Status: <%= @dog.adopted ? "Adopted" : "Available for adoption" %></p>

<a href="#" id="detailed-description">Breed Description</a><br></br>

<div id="dog-description"></div><br />

<a href="#" id="wikipedia-link"></a><br></br>
```

*Dogs Index View*

```
<h1> List of Dogs </h1>

<ul>
  <% @dogs.each do |dog| %>
    <li><%= dog.name %>: <%= link_to "Show #{dog.name}", dog_path(dog) %></li>
  <% end %>
</ul>
<br>

<div>
  <button id="show-breeds">Show Breeds</button>
  <ul id="list-of-breeds"></ul>
</div>
```

*Dogs.js File*

```
$('.dogs.show').ready(function(){
  console.log("Document has been fully loaded.");
 
  var descriptionLink = document.getElementById('detailed-description');
  if (descriptionLink != null) {
    descriptionLink.addEventListener("click", getBreedDescription);
  }
});

$('.dogs.index').ready(function(){
  console.log("Document has been executed."); 

  var showBreedsButton = document.getElementById('show-breeds');
  if (showBreedsButton != null) {
    showBreedsButton.addEventListener("click", getListOfBreeds);
  }
});
```

Furthermore, I created a couple of functions, getBreedDescription() and getListofBreeds(), which sent some requests using fetch and the JQuery .get() method in order to retrieve the relevant information. Note that only HTML strings can be sent as information over the Internet. Once the information is retrieved by the external or internal API, it must be converted back to JSON. The JSON format allows for easier manipulation of the data so that it can formatted and appended to the application.

I then proceeded to select only the information that I needed, such as the dog description or dog breed, and save the information into a variable. I used the .innerHTML and .href methods for Javascript in order to set the inner HTML content and url path for the link equal to the corresponding values of this information from the response. Another way to append information onto the application is to use the .append method which directly adds the argument that is passed in onto the respective HTML element.

*Dogs.js File continued*

```
function getBreedDescription(event) {
  event.preventDefault();
  const searchTerm = document.getElementById('dog-breed').dataset.breed;
  const searchURL = `https://en.wikipedia.org/w/api.php?action=opensearch&format=json&origin=*&search=${searchTerm}`;
  
  fetch(searchURL)
    .then(parseJSON)
    .then(displayData)
};

function displayData(data) {
  console.log(data);
  const description = data[2][0];
  document.getElementById('dog-description').innerHTML = description;
  const url = data[3][0];
  const wikipediaLink = document.getElementById('wikipedia-link');
  wikipediaLink.innerHTML = "See full information.";
  wikipediaLink.href = url;
  wikipediaLink.target = "_blank";
}

function getListOfBreeds() {
  $.get('/dogs.json', displayBreeds);
};

function displayBreeds(data) {
  const list = document.getElementById('list-of-breeds');
  data.dogs.forEach(function(dog){
    const listItem = document.createElement('li');
    listItem.textContent = dog.breed;
    list.append(listItem);
  })
};
```

As a result, I incorporated Javascript into the Rails with Javascript project which allowed me to add dynamic features, including a link and a button, to the application. I used AJAX (asynchronous Javascript and XML) as well as the fetch method in order to request and retrieve data from a server and use the data to update a page in the application without reloading the page. AJAX allows for a user request to be processed and the corresponding content to be rendered immediately on the page whereas an HTTP request requires a full page reload. AJAX is therefore a very powerful technology that is used for client-side web development and which can greatly enhance the users’ experience.

In order to view the Adopt-a-Dog Rails with Javascript application, please visit: https://github.com/vbustabad/rails-js-assessment-v-000. 
