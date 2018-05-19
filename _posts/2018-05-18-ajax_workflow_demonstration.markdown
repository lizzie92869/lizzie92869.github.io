---
layout: post
title:      "AJAX workflow demonstration "
date:       2018-05-19 02:56:33 +0000
permalink:  ajax_workflow_demonstration
---


I just started to learn about the cool things JavaScript can do. Right now Ajax look like nothing short of magic so I choose to write an article to demystify it for all those who are taking the same journey.

**What is Ajax ? **
Very simply, it is the use of JavaScript (mostly implemented through JSON) to communicate with servers (request, receive and send data), to  update an html page without the need to reload it. 

**Where is Ajax used?**
Anywhere pieces of information are saved or retrieved without the need for the user to have another page or the same page loaded again.

*some examples: *
- data validation on save actions
- change the values in a drop down list-box based on other inputs
- text hints and autocomplete text boxes

**Demonstration of a pattern**

The implementation of a pattern is, of course, very dependent on the purpose of the code. Here, I am demonstrating a basic use of an API to display its informaton on an HTML page. 

*What do we need?*

In short:
1. a .html page, a .js page and a link between them.
2. a trigger for when to send our data request
3. a js function to send the request of data
4. a js function to organize the display the received data
5. a location in our HTML to display the info

In detail
1. Create an index.html, an index.js and add the script that attaches the js in the html.
> `<script src="index.js"></script>`
2. Create a trigger for when to send our data request (for example a link or a button the user would click). In the HTML page, insert:
> ` <a href="#" onclick="getRepositories()">Get Octocat Repositories</a>`
3. Build the function that make and send the data request. In the JavaScript:
```
function getRepositories() {
  const req = new XMLHttpRequest()
  req.addEventListener("load", displayRepositories);
  req.open("GET", 'https://api.github.com/users/octocat/repos')
  req.send()
}
```
First this function will create a new request. Then it will start listening for the response to be complete (which can't happen right then). Then we make the API request by calling open with the HTTP verb GET, and the URI for the request and finally we send this request. When the response is fully loaded the callback function of the EventListener will fire. The order of these steps is important as it is an asynchronous request. If we were setting the req.open before the listener, this would break. 

4. Build the function that display the data received:
```
function displayRepositories() {
  const repos = JSON.parse(this.responseText)
  const repoList = repos.map(repo => repo.name)
  document.getElementById("repositories").innerHTML = repoList
}
```
First we convert the response of the server from a string to a JavaSript Object. Then we map over all the repositories and append their name to the html div with the id "repositories". 

***Please note :***

 A/ [.name](https://developer.github.com/v3/repos/) is a method given for free by the Github API
 
 B/ [.getElementById](https://www.w3schools.com/jsref/met_document_getelementbyid.asp) and [.innerHTML](https://www.w3schools.com/js/js_htmldom_methods.asp) are HTML DOM methods given by JavaScript 
 
 C/ In the context, this is the const req. Why? because in the getRepositories function we attached the EventListener to req like this: req.addEventListener("load", displayRepositories);

5. We create this div with the id "repositories" where the repositories names will be display. In the HTML page, insert:
> ` <div id="repositories"></div>`

You should now see the name of the github repositories of Octocat !








