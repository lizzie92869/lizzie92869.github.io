---
layout: post
title:      "Portfolio: GimmeMyLinks with Jquery"
date:       2018-06-16 21:02:53 +0000
permalink:  portfolio_gimmemylinks_with_jquery
---


When I started to work on my portfolio project I was really unsure about how querying an API to retrieve data. Now that my project is over I can say that I learned a tone. Not only I know how to retrieve those information but I also know how to create them, render them and create the API. Today I thought I would share with you what I learned through some of the patterns I used. 

My project can be seen as an evolved toolbar.  You save your links in lists, you can view the websites you saved through a frame making it easy to remember what the resource was. You can scroll through this resources in the application without having to visit the link which makes quick reads very convenient. 
I started this project reworking my relationships in my models and database. Those were set up to answer the requierements of the previous assignement but even if it was working great they were feeling awkward to work with. 
So a user has_many :lists, a list has_many :links, a user has_many :links through: :lists

# Connecting the .js
the first problem I encountered was that my javascript sheet wasn't connecting with my html. I knew my script was working as I inserted in the appplication.html in script tags and it was connecting ok.  After some debugging I found out my <%= javascript_include_tag 'application' %> was loaded before the jquery link to its cdn, which can't work for obvious reasons.

# Attaching Listeners
Then I started to display my lists names through AJAX, then I moved that list to a side bar which appears on click of a right chevron and... it wasn't working anymore. Well what happened is that you can not listen to an event on an element that is not visualy available! I could of course see my sidebar opening but if I listen to it from the document it is not available there. And that's when I really understood the use for attachListener functions. 
For the purpose of the demonstration let's take an easier example I encountered. I want to be able to close my side bar by clicking on the left chevron. Well I can't close that bar unless it is open (the DOM include the bar). Here a simplified version of the pattern:
```
function openBar(e) {
    e.preventDefault();
        let barData = `
            <div class="js-content-bar">
            <a href="#"><i class="close-bar material-icons">chevron_left</i></a>
            </div>`
        $(".js-bar").html(barData)
        attachCloseBarListener()  
};

function attachCloseBarListener() {
    $(".close-bar").on("click", closeBar);   
}

function closeBar(e){
    e.preventDefault();
    $(".js-bar").html(``)
};
```
# appending erb code through JavaScript
One of the first thing you will have to do when adding the jquery frontend to your rails project is to render those beautiful erb code through (less beautiful) plain html strings and no, unfortunatly there is no way to keep our erb tags and rails helpers. So everything need to be translated. At first I thought it would be impossible. Well, it is always possible. Let's take an example. Here a pattern I often encountered:  a route set dynamically using an helper:
Let's say you what to view a list content  when clicking on its name:
```
<a href=<%=list_path(@list) %>something</a>
```
* you will need to query your API while you are generating those names and then find the access to the id of your ressource. for me it is in a loop at data[i]["id"]  :
```
    for (let i=0; i<data.length; i++) {
<a href="/lists/`+data[i]["id"]+``+">` + data[i]["name"] + `</a>
```
* You could also remove the path and decide to create a listener which would take into account the id to know what to render. This makes use of attributes.
```
<a class="js-list"  href= "#" data-id=` +data[i]["id"]+`>something</a>
```
When the link is clicked you would then tell it to show the right resource through displayLinksFromBars :
```
$(".js-list").on("click", displayLinksFromBars)
```

# Creating a resource while using devise
This had been a puzzle for days for me so I am glad to have overcome it and share it with you. 
If you are using Devise for your user authentification you won't be able to create a resource in your API (in my case a list) as easily as it is done in all demonstrations you can find. The reason is that you need an authentification token while posting the resource. I am going to write another blog post on this specific issue to explain each step of the process. It may sounds complicated at first but it's pretty simple to solve.  
# My warning
Overall if there is one warning I could give it is to not get tricked to make everything display with AJAX (which can be tempting wen you encounter your link helpers and you have to translate the path) at the point of having a single page application. This is very time demanding and you will be asked to do this through React for the next project. My feeling about where to bring this project to is, as much as you need to feel like you master the requirements of this project. 


It has been a crazy journey when I think that only 3 weeks ago I thought I could never get my head around javacript and AJAX . I am glad to say today, it all makes sense and I gain the confidence I can learn anything as long as I take the necessary time, even if it means slowing down at the point of going back in my confort zone from time to time. 
