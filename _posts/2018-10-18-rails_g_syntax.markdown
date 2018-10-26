---
layout: post
title:      "Rails g syntax"
date:       2018-10-18 22:24:08 -0400
permalink:  rails_g_syntax
---


So many times I have been looking for how to generate migrations models or controllers from the command lines. Is it singular or plural? Camel or snake case? What is it generating exactly? So here are a Cheat Sheet post for my blog. 


```
rails generate scaffold TeamMate game:string score:integer

rails destroy scaffold TeamMate

rails generate model TeamMate game:string score:integer

rails destroy model TeamMate

rails generate controller TeamMates index show update delete

rails destroy controller TeamMates

rails generate migration CreateTeamMate game:string score:integer

rails generate migration AddNameTeamMate name:string
```

Overview of what those command lines are generating for us: 

![](https://imgur.com/L5t00Oy.png)	

Details of what those command lines are generating for us: 

*rails g scaffold:*
*  **table **team_mates
*  **model** TeamMate
* TeamMates**Controller**
* JavasScript, controller, model, helper, routing, views spec
* team_mates css
* TeamMatesHelper
* TeamMateSerializer
* team_mates **views** _form, edit, index, new and show

*rails g model:*
* ** table **team_mates
*  **model** TeamMate
* model spec

*rails g controller:*
* TeamMates**Controller** with the actions index, show, update, delete
* routes used by the controller (specified by you in the command line)
* team_mates css
* TeamMatesHelper
* JavaScript, controller, helper, routing, views spec
* team_mates **views**  index, show, udate, delete (specified by you in the command line)

*rails g migration:  *
* **table** team_mates

There is of course more that Rails can generate for us, but I found those the most useful. If I can point to rules here to not have to refer to a cheat sheet each you want to use those command lines, it would be: 
1. Everything in CamelCase
2. rails g model + rails g controller =(almost) rails g scaffold

Happy coding!

