---
layout: post
title: Memory Leaks in Node.js!
comments: true
---

One of the main problems which I encountered while writing Node.js applications in Walmart Labs is Memory Leaks.

### But why? Is it important?

Javascript was basically written for frontend, wherein the code ran for a very short duration(till the page was live). So there was not much scope to check for Memory Leaks. The page had to be refreshed anyway, resetting all the created internal heap and stack. But since the advent of Node.js, javascript is widely used on the server side. These apps are supposed to be run for an indefinite time, unless you want it to stop or ofcourse, it crashes due to an error. It's grave impact can be judged by the famous [Walmart Node.js Memory leak](https://www.joyent.com/blog/walmart-node-js-memory-leak). Similar to other applications, the heap memory occupied by the applicaton continued to rise until the maximum limit was reached. And the application crashed.

![Memory Leak in Progress]({{ site.baseurl }}/post_images/memory_consumption_nodejs.png "Memory Leak in Progress")

Before diving deep into Node.js code, let's briefly look into the garbage collection.  
A programmer is ideally responsible for both creation and destruction of objects. We do it in C/C++ by alloc(), malloc() and free() methods. But this process is automatically taken care of in modern languages using the garbage collector. The main objective of Garbage Collector is to free heap memory by destroying unreachable objects. If a memory segment is not referenced from anywhere, GC assumes that it is not used and, therefore, can be freed. However, retrieving and maintaining this information is quite complex as there may be chained references and indirections that form a complex graph structure. It might also occur that there exist a group of objects that reference each other but they are not referenced by any active object in the application. They need to be freed too.

Node.js uses V8 engine as its runtime and all the processes which we discuss are native to V8 engine and not actually Node.js
Garbage collection is a rather costly process because it interrupts the execution of an application, which naturally impacts its performance. To remedy this situation, V8 uses two types of garbage collection:
- Scavenge: fast but incomplete
- Mark-Sweep: relatively slow but frees all non-referenced memory

![Types of GC]({{ site.baseurl }}/post_images/Duration-and-frequency-of-GC-runs.png "Types of Garbage Collection")

We can see that Scavenge Compact runs at a much higher frequency than Mark-Sweep. Depending on the complexity of an application, the durations will vary. You might want to take a look at [this](http://jayconrod.com/posts/55/a-tour-of-v8-garbage-collection) link for detailed explanation of V8 garbage collection.

### Lets see it happen

Some leaks are pretty obvious — like storing data in process-global variables. A garbage collector would not know when to clear that information. But that is not what we are focussing at. 
> There can be times when we assume that some temporary data will be deleted but it actually does not. 

![A memory Leak]({{ site.baseurl }}/post_images/DK_8.png "A Memory Leak")

This looks OK at first glance. We could think that theThing get’s overwritten with every invocation of replaceThing(). The problem is that someMethod has its enclosing scope as context. This means that unused() is known within someMethod() and even if unused() is never invocated, it prevents the garbage collector from freeing originalThing.  There are simply too many indirections to follow.  This is not a bug in your code but it will cause a memory leak that is difficult to track down.

### What to do?

Use the [V8-profiler](https://www.npmjs.com/package/v8-profiler) and chrome developer tools to look for memory consumption over time.  It provides a convenient option to take a heap snapshot and compare different versions of them. Run ``` node --inspect server.js ``` and then navigate to chrome://inspect and you can remotely debug your node process. If you see heap memory increasing and slowly trying to take up all your memory, look for object references that might not be removed by GC. Still confused, here's a pro tip: just add ``` theThing = null;``` at the end of the function, and your day is saved.