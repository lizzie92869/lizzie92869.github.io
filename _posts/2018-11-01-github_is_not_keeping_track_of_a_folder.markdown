---
layout: post
title:      "Github is not keeping track of a folder"
date:       2018-11-01 17:36:40 +0000
permalink:  github_is_not_keeping_track_of_a_folder
---



Recently, I have been asked to work on an interview exercise where the company asked me to scaffold a blog inside a blog folder - that I had to create with the commend line rails new blog - which was part of a repository I had to fork. So the overall path for my scaffolded folders and files was company_name_exercise/blog/blog.

If you are a junior like me and you only had to create an application from the first level of your own repository, you never had a problem with commiting your work. 
Here adding, commiting and pushing doesn't keep track of anything inside the blog folder. 

But why? Well, if you come back to the basic you will remember that creating a new rails application is creating a .git file in your blog folder. The problem is that you already have a .git at the first level of the forked repo. 
The solution? Easy you need to remove the .git from the blog repository. This is a hidden file so you will need to go in the blog folder and do this from your command line : 
```
rm -rf .git
```
That easy? well, no. If you try to add and commit now you won't see more than before. You have to "force" it to see this new content.

For this you will need to 
1. remove the blog folder (move it temporarily to your desktop for example)
2. add and commit 
3. recreate the blog folder
4. add and commit 
5. move the folders and files back to the blog folder
6. add and commit

That's it.

Happy coding!
