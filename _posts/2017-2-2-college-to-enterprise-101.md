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
Let us say you start writing a python code to scrap a website. For a new user to run your code, he will have to install all the 

Let us say you started writing a code in 2016 and pick it up again in 2018. You might have upgraded the softwares, or maybe changed the OS. So when you try to build 
- Examples: npm in Nodejs, bundler in ruby, maven in java, pipenv in python