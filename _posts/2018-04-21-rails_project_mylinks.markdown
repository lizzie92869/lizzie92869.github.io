---
layout: post
title:      "Rails project: MyLinks"
date:       2018-04-21 18:36:51 +0000
permalink:  rails_project_mylinks
---


Looking at my navigator toolbar makes me want to close my window. I didn't open anything yet but my tab looks overcrowed and don't even get me started on the hidden part of it. When I click on the arrow of the drop menu, more clutter shows up. It's like trying to hide my mess in a drawer. I needed something to tidy things up; I needed a whole closet with more drawers and sliding doors. 
So I started to design an app to keep track of links. These links would be organized by list and a window would allow me to read the content of the page linked through a frame. Bye bye toolbar that displays logos and titles spanning the length of the browser. 

My association logic has been stretched a little bit by the requirement of a joining table that would have its own attribute modifiable by the user. Here how it goes:
https://creately.com/diagram/jg904upo2/o0x1eO2bVLKdaT8JJX6o87GBwY%3D
A user has_many links, a list has_many links. To keep the system as simple as possible, the links table becomes the joining table, linking together the user and his lists. I pursued this pattern as it became a challenge for me to make it work until the end and it did a fantastic job!

I felt like I should dive deeper in Devise to better understand its avantages and limitations, so I decided to implement it in my application. Overall, I really feel like that devise is worth trying, saving you a lot of time and that there is nothing to be afraid of. Even Omniauth has specific instructions to lead through the implementation of their authentification system with Devise. In addition to having my authentification logic already set for me, I had access to methods like authenticate_user! and current_user. Sweet!   

The CSS was the most difficult part as I don't know javascript yet. Bootstrap also gave me some frustration but the simplicity of the design helped my limitations. 

I added a nice filter relaying on scope methods to order the links by priority: most recent or oldest first. 

I had to do a lot of clean up everywhere. 
- The controllers received some new helpers and before_action
- The CSS stylesheet was full of useless classes, so I separated functionalities in different spreadsheets and removed inline styling (when possible)
- The views needed an insane amount of cleaning and organizing so I made good use of partials. 

I have to say that before starting this project I was kind of bummed to have to learn javascript after having learned my precious and beautiful Ruby for so long, but when I saw how much trouble I had to run through with my view I certainly recognized the value of Javascript. 

Now I am happy to jump into this next chapter! 

