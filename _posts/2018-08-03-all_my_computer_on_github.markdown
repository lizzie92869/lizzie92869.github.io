---
layout: post
title:      "All my computer on github"
date:       2018-08-03 19:10:48 +0000
permalink:  all_my_computer_on_github
---


Yesterday I ran into a puzzle that challenges my patience quite a bit. I wanted to push my project on github and all my computer ended upin my repo. 

I could see all those files tracked while running "git status" but it wasn't making any sense. How did they ended up there? I tried to find a way to remove those files from the staging area with several git commands which did nothing. So I tried to create another React project and pushed it unto github. My all computer loaded again on github! What's this madness???

I ended to get help from Cernan, the lead session for Javascript. He made me run git status from the root of my computer. Here they were again. I was explained, I may have done, at some point,  a git init from the root of my computer causing all those files to be staged. 
We ran "rm -rf .git" from that root to remove the ".git" from there, then we went in my project folder and initiate this ".git" from there with "git init". Looking at "git status" shown that the problem was solved. From there, I only need to create my repo on github and hook it up as directed by the instructions. 

I hope this post can save some time to other git users ! Happy coding!
