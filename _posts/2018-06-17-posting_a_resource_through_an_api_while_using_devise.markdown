---
layout: post
title:      "Posting a resource through an API while using Devise"
date:       2018-06-17 16:36:07 +0000
permalink:  posting_a_resource_through_an_api_while_using_devise
---

Today I wanted to share with you a little challenge I encountered. For the purpose of this demonstration I simplified the code I have to focus on the solution itself. 

Learning about how to create a resource through an API seemed pretty straight forward so I was confident about being able to implement this without too much difficulty. 

Here was my html form to create a list: 
```
<form action="/lists" method="post" >
    <input id="newListName" type="text" name="name" >
    <input type="submit">
</form>
```
Then I wrote in my .js:
```
$(function() {
    $("form").submit(createNewList)
});


function createNewList(event) {
      event.preventDefault();
      let values = $(this).serialize();
      let posting = $.post('/lists', values);
      posting.done(function(data) {
      $(".js-appendListName").append(list["name"]) 
      });
};
```
and in my controller: 
```
def create
    @list = List.new(list_params)
    current_user.lists<<@list
	    if @list.save
	    	render json: @list, status: 201
	    else
	    	render json: @list.errors, status: 422
	    end 
 end
```
I tested it out and I could see nothing was appending. From my rails console, I discovered that nothing was created; dropping in my browser console data wasn't even defined. Oddly enough, the let values looked ok yet my create action wasn't hit... 
I tried to debug the undebuggable for a long time until I talked to a student in slack who made me scroll up (and up and up) in my terminal to where the server was running. I had a CSRF token error. After some google research I understood I needed to pass the authenticity_token in my values. To do that, I had to restructure the whole hash params. 
Here's how it goes:
```
function createNewList(event) {
      event.preventDefault();
      var values = {
        "authenticity_token": $('meta[name=csrf-token]').attr('content'),
        "list": {
            "name": $("#newListName").val(),
      }};
      var posting = $.post('/lists', values);
      posting.done(function(data) {
      $(".js-appendListName").append(list["name"]) 
      });
};
```
And there it is! 
I hope this demonstration can be useful to others. Having lost so much time on it, perhaps this article will prevent some unecessary frustration.




