---
layout: post
title:      "Quick introduction to Unity"
date:       2018-12-05 17:59:23 +0000
permalink:  quick_introduction_to_unity
---

I have recently ask to look at Unity and train myself to this tool. 
I started a week ago with a Udemy course for 2D in C# and I have to say this is the best investment that I have made on this website. 
Today I am trying to share some of the first things you will need to know to get started with a project. 

First of all Unity is widely used for 2 and 3D gaming applications. when you download this tool, the editor by default opening your script is visual studio and you don't need to download it separately.

![](https://imgur.com/a/JaCeZ3X.png)

Right now as a beginner, you want to focus on the asset part of the interface and the main camera. 

From the Unity website: 
"An asset is representation of any item that can be used in your game or project. An asset may come from a file created outside of Unity, such as a 3D model, an audio file, an image, or any of the other types of file that Unity supports. There are also some asset types that can be created within Unity, such as an Animator Controller
, an Audio Mixer or a Render Texture"

The first thing you are going to learn to create in the assets folder are: 
- text areas: to display static or dynamic text
- Scripts: that contain the logic of the program (choose from JavaScript or C#)
- Sprites: basic shapes to build a visual 

For this, you only need to right click the asset folder and choose from the dropdown menu.
Once your asset is created you will need to attach it to an Object. 
Those resources are useful only if an object is using them. 
You can create Object by right clicking "main camera" and choosing "create empty"

As a start you want to create a canvas under the main camera (right click on main camera and choose from the dropdown menu) in this canvas folder you want to create text areas by right clicking and choosing UI --> text.
You can also create images with UI --> images and give the rectangle block created a color in the inspector (under image(script) --> color)

When you will start to write your scripts and have them attached to objects you can play those scripts with the play button at the top of the interface. 

I hope this introduction can get you started to play with the different components on the unity platform.

Happy coding!






