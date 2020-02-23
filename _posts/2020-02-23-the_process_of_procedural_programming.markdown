---
layout: post
title:      "The Process of Procedural Programming"
date:       2020-02-23 20:11:48 +0000
permalink:  the_process_of_procedural_programming
---


Today I'm going to talk a little about my approach to working with procedural Ruby. First, let’s cover what I mean by “procedural”—as programmers, we often take for granted an understanding of the various conceptual models behind different programming languages, but let’s take a step back and clarify the core differences between procedural and object oriented programming.

Procedural programming is based on procedures (such insight, much wow). The program relies on running procedures (or functions, routines, subroutines, etc) which are simply a series of computational steps. When a program is executed, any procedure in the program may be called at any point, including by other procedures within the program. Basically the entire program is based on variables and instructions about what to do with those variables. Examples of procedural programming languages include FORTRAN, ALGOL, COBAL, etc.

Object oriented programming is like procedural programming that has been bound up in the structure of an object—data and behavior are contained within an object that can in turn interact with other objects, each containing both information and functionality. Objects contain data in the form of attributes and behavior in the form of methods. The most popular object oriented languages are class based, meaning objects are instances of a class that determines their type and core behaviors. Examples of object oriented languages include Ruby, Python, C#, Objective-C, etc.

Using Ruby in a procedural manner doesn’t take advantage of its full potential. When Yukihiro "Matz" Matsumoto developed Ruby in the mid-1990’s, he wanted to make a programming language that would make programmers happy and productive. It was intended to mirror the real world in that it was natural, but not simple; therefore he created an object oriented language in order to better reflect how information, functions, and tangible objects operate in the real world. Ruby is powerful as an object oriented language that is able to abstract complex functionality into elegant code. The powerful abstraction comes in large part from the consistency within Ruby, from individual instances to class methods to vast data structures, that allows programmers with a solid grasp of the language and logical thinking to extrapolate how Ruby programs will behave in novel situations.

But setting aside object oriented Ruby (but why would you tho it’s frickin’ great, ok), let’s look at procedural Ruby. When using Ruby in a procedural manner, it has all the functionality you would expect in procedural language like Pascal. You have variables and ways of storing information that can be retrieved within the program by other functions, the have data structures like arrays and hashes, and many methods that rely on boolean states of truthiness or falsiness, logic, loops, iteration, and enumeration.

When I’m working in procedural programming, I start by writing out what my code will do in plain English. It’s kind of like the exercise of explaining how to make a peanut butter and jelly sandwich, but this jerk follows your *literal* directions rather than helping a sister out and just making the sandwich.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ct-lOOUqmyY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

As you begin to actually write out the code, you’ll often notice gaps in your logic that you need to go back and fill in (like take the bread out of the bag first, duh). So the process is iterative and requires moving back and forth between the code, the final goal, and the steps your program is going to need to move through in order to work.

In my next blog post, I’ll further illustrate my approach to procedural programming by walking through the process of writing one of my favorite little procedural Ruby exercises, the prime?() lab.

