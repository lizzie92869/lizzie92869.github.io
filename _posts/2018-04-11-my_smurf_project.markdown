---
layout: post
title:      "My smurf project"
date:       2018-04-12 03:18:23 +0000
permalink:  my_smurf_project
---


I was looking for ideas for this project and a friend of mine showed me those data sets, very smart stuff! Then I told him that instead I was going for a smurf avatar generator. Way less smart indeed. I wanted to have fun. I knew it's when I have fun that I am ready to take on the biggest challenges. First of all I realized I would need a user to have one avatar and my avatars would have several users. What? Yes, I had a smurf or a smurfette, you could change 2 different attributes for each of them. In other words, you have 32 possible avatars for all users so some users will end up to have the same avatars. As weird as it sounded to me I started to create my relationships: user belongs_to avatar, avatar has_many users. 
My project was different from what we had coded in previous labs because the 32 avatars were actually known from me. I knew that your avatar was one of those 32. On a very helpful chat with the support team I have been suggested to seed my database with those 32 avatars. Bingo! I just needed to add an avatar_id in my users table to indicate which avatar the user had chosen. So instead of having an Avatar.create upon saving the avatar I had an Avatar.find_by.
Another challenge I faced was to actually visually display those avatars in a manner that would let the user think he was customising the smurf or the smurfette. The only way I saw that possible was to display one of those 32 avatars each time the user would update an attribute, giving the illusion only the color of the hair of the smurfette changed, or only the color of the eyes of the smurf changed, redirecting trial after trial to the same view. 
I worked for days on the logic and its implementation but worked as much, if not more, on the html and css to have a beautiful finished app that I would be proud to insert into my portfolio. Is it the smartest application? Probably not, but it's definitely one of those projects that demonstrate some good junior developper skills and was, cherry on smurf, very fun to build.  

