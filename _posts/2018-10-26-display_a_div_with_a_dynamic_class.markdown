---
layout: post
title:      "Display a div with a dynamic class"
date:       2018-10-26 17:15:09 +0000
permalink:  display_a_div_with_a_dynamic_class
---


Setting a class dynamically is very useful in the case you are displaying custom content for your user. 
For example, let's say you have a contact list and you want to display only the contacts for the company name the user is clicking on.
![](https://imgur.com/De269PX.png)
![](https://imgur.com/ZfLSoI9.png)

First you will have to indicate a class on the html element to click:
```
<a href="#" class="company-name"><%= company %></a>
```
(In this project I am using Ruby on Rails, that is why you will see those erb tags to communicate with the back end. However, we are only focusing on the JavaScript demonstration here.)

You can can then write in your JavaScript document a function listening for the click as soon as the document has finish to load.
```
$(function() {
    attachListener();
});

function attachListener() {
	$(".company-name").on("click", filterContactList);
}
```
Then You will want to indicate which element to show. if you are filtering those elements depending on the name of the company, you could have a dynamic class like this in your html: "<%=contact.company%>-contact
This is taking the name of the company for this contact and appending "-contact. So in the browser developer tool you would see: class="printABC-contact"
```
<% @contacts.each do |contact| %>
          <% if contact.company == company %>
          <div class="<%=contact.company%>-contact hidden">
            <%= contact.name %>
            </div>
          <% end %>
        <% end %>
 ```
 
 We are adding a "hidden" class which will be defined like this in the CSS stylesheet:
 ```
 .hidden {
  display: none;
}
```
So by default this content is hidden until this class is removed. 
Now we want to remove this class but we need to specifically target the element we want to hide.
Adding a dynamic class on the company name we display allow us to have access to this value when cliked.
We could also have achieved this with a value attribute instead of a class.
```
<a href="#" class="<%=company%> company-name"><%= company %></a>
```
Now we can get the company name by looking at the value of the target of the event and use it to listen to the element hidden which as a class of the company name + "-contact".
```
function filterContactList(e){
	let dynamicClassCompanyName = e.target.className.split(' ')[0]
	e.preventDefault();
	// remove the hidden class for the names that are from this company
	$(`.${dynamicClassCompanyName}-contact`).removeClass("hidden");
	attachHideFilteredContacts()
}
```
 
That's it! Now if you click the company name, the contacts will display.
I added a a listener 	attachHideFilteredContacts() to be defined in the same logic to hide the element again when the name of the company is clicked. Your turn!

Happy coding!
 

