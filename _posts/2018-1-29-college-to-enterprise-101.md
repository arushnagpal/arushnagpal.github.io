---
layout: post
title: College to enterprise 101!
comments: true
---

Transition from college to Corporate life is a big change in all aspects, be it technology, atmosphere, culture, people etc.
In this post, I will try to describe the basic technical terminology in terms a layman will understand. The examples might not _only_ do the work mentioned in the tool description but a major usage will involve the functionality described. 

## VCM tools
While building a software, did you sometimes want to go back at a time when you made a major change and your software crashed? They have a fancy word for it: Version control management(VCM) tools. It allows you to save the code at a point in time and rever back whenever you want to.
What's more? When 2 or more people work on a single project, it allows multiple people to work on the same code and automatically merge the changes. So if one team member is trying to work on one feature, another one can pick a different one and later merge it into the main working version of code known as 'master'.
- Examples: Github, Bitbucket

## CI Tools and build jobs
After writing code, what do you do? You run the compiler to build an executable file. That may be building an apk in case of Android, building a jar in case of java or an binary executable in case of linux/windows. After building it, you try to move it to the actual server where it will be running at all the times. But the question is Do you have such time in big companies to manually build and deploy? No, right? CI tools come to the rescue. CI means Continuous integration. As soon as new code is pushed to the master branch(in any VCM tool), we write a script to build the executable and move it to the deployment server where it is actually run. This is taken care by the build tools which actually automate it by automatic building and deploying.
- Examples: Jenkins, Travis CI

## dependency managers
Let us say you start writing a python code to scrap a website. For a new user to run your code, he will have to first install a scraping library, a utility library, then a data processing library and then a UI library and so on. What to do? you make a list of all the dependencies along with the version of the library that works exactly fine for your code. This is exactly what is done by a dependency manager. This will even help you to run you code on a new machine with just a single command to setup the dependencies.
- Examples: npm in Nodejs, bundler in ruby, maven in java, pipenv in python

## Dockers/Containers
We discussed about dependency managers. Now what if you want to run 2 different projects in your system but they both use a common library (lets call it express). One project uses version 1.0.0 of express but the other uses 1.4.1. But you can only install a single version of a library at a time. Does that mean you won't be able to run both of them together? Thats where containers come to rescue. You might have heard of Virtual machines which allow you to run different OS's on top of a single OS. dockers  
Docker Containers are better than VMs in the following three parameters:
- Size – This parameter will compare Virtual Machine & Docker Container on their resource they utilize.
- Startup – This parameter will compare on the basis of their boot time.
- Integration – This parameter will compare on their ability to integrate with other tools with ease.

Read this [link](https://www.quora.com/What-is-Docker-Please-explain-it-in-simple-terms) for more details.

## Jira
In an enterprise, there can be 2 to more than 60 people working on a single component of a project. How do you keep a track of what tasks are there, who is doing what task, how complex is a task or how many days a task should take. Jira is a tool which helps you keep a track of all these things. But who decides the tasks, when do they do that? Read about sprint in the next paragraph.

## Sprint and Agile Methodology
In an enterprise, things have to be run in an orderly fashion. The work to be done has to be planned earlier according to the deadlines planned by the higher management. 

You will hear the term Sprint a lot. A sprint is a set period of time during which specific work has to be completed and made ready for review. Each sprint begins with a planning meeting. During the meeting, the product owner (the person requesting the work) and the development team agree upon exactly what work will be accomplished during the sprint. The development team has the final say when it comes to determining how much work can practically be accomplished during the sprint, and the product owner has the final say on what criteria need to be met for the work to be approved and accepted. After a sprint begins, the product owner must step back and let the team do their work. During the sprint, the team holds daily stand up meeting to discuss progress and brainstorm solutions to challenges.

## Frameworks
You might already know frameworks but why to use them? Isn't it better to write the code from scratch? I'll learn more and I'll write the code only needed to suit my requirements? When you are working in a large company, the clear answer is NO. There are a lot of tasks that need to be done in almost all the applications. Should you waste your time in writing them again and again? No. That is why companies use frameworks, to save time. If you ask me for examples, lets see. Most of the code involves writing REST APIs. It is probably not a very good idea to every time write the code to create a server, expose an endpoint and route it for processing. We should focus more on the part that involves the business logic. Express is a framework in Node.js that handles all this and all you have to do is write 4-5 lines instead of 200-300 lines. Most of the work in large companies is done in modules. You might need to create an object in one module and use it in another. You would like to reuse an object which was created in another module. Similarly, you might want to create some objects at startup or create a global object for whole application. Frameworks like Spring in Java help you with that. I'll suggest you to read [this](http://www.tutorialsteacher.com/ioc/dependency-injection) link for more clarification on this topic.

## Writing code
When you are new to a company, you might ask many questions as to why this? why not that? Why write very long names and more documentation instead of doing the job in less time. Companies involve many many people working on the same code. People leave and their code has now to be handled has to be read and maintained by some other person. Do you know what is better code? Which is not a pain in the ass for the maintainer. The amount of time which you would spend in rewriting the code is huge. Therefore the code should be self explanatory. You might need to take care of petty issues like using StringBuffer(Mutable) instead of String(Immutable) in Java. This might not make much difference in  Much of the code written in a company has to be run at a very large scale. Even a small performance lag might cause significant loss because of the fact that the same code which could be 0.1 seconds faster, when run by 1000 users simultaneously, will take a delay of 100 seconds.  
So then how to write code? Read [this](https://12factor.net/)

## Scaling Up
In personal projects, you might have almost never felt the need to use 2 machines to do the same work. That's not what happens in an enterprise. You will need to scale up. There are 2 tyes of scaling: vertical scaling as well as horizontal scaling. Horizontal scaling means that you scale by adding more machines into your pool of resources whereas Vertical scaling means that you scale by adding more power (CPU, RAM) to an existing machine. There are various challenges which you will face while scaling up, different architectures are present which might help you do that. You will hear about cassandra, Kafka, MongoDB etc. All these databases use the same concepts of distributed processing. You'll hear a lot of buzz words like master slave architecture/ slaveless architecture. I'll leave those concepts for your projects to explain :P  
Read [this](http://highscalability.com/) amazing website to see how companies have scaled up. 

## Microservices
I'll leave [this](https://dzone.com/articles/microservices-vs-soa-2) website with the job of explaining this to you guys. 
With great software we have different types of components: queues, databases, processing components etc. What your task should be is not to learn how to use them, but how they were built.

Have a great day!!