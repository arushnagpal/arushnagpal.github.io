---
layout: post
title: Designing applications in Node.js!
comments: true
---

The motive of this post is to discuss how to design an application written in Node.js in a scalable fashion. On a wider scope, these principles apply to any event driven programming language. I'll discuss where we went wrong in an Walmart's internal application using a similar analogy.

To give an overview of Node.js, it is event based. You can write event emitters and event listeners wich listen to those events. Theoretically, there is no limit to the number of event emitters and subscribers that can be written.

## Design

Let us try to design an application which will scrap a list of webpage and send the downloaded post to another server for processing based on a certain logic. Let us think intuitively on how to design such an application.

Let us consider that we have a list of webpages which are required to be scraped stored in a database.

Now let us decide the broad components and give a name to them for easier interpretation:
1. One to fetch the list of websites from the database (name: Fetcher)
2. Another to actually scrape the data from the website (name: Processor)
3. Another to send the data to a different server if the logic requires (name: Dispatcher)

- For each website in the list, the fetcher will dispatch an event '1' to the processor  
- Each event '1' received from the fetcher will trigger a web scraping task and after scraping is done, Processor will send another event '2' to the Dispatcher with the scraped data.  
- Each event '2' from the Dispatcher will trigger a dispatch only if required by the written logic.

Simple, right? The app is working pretty fast because the list is small. Assume that the processing part is majorly taken care by different servers and there is no load on by the nodejs application on the server.

## Where can we go wrong
Things start to go wrong when the list of websites increases. Lets start by using both horizontal as well as vertical scaling:

- Horizontal scaling  
    Let us deploy the same code in 2 different servers. But wait, Fetcher is the same in both the servers. If both the Fetchers will perform the task of fetching the same data, we will end up in processing the same data twice.

- Vertical Scaling  
    Lets use cloning by pm2 or inbuilt module 'cluster' to spawn multiple processes for the same application. But even in this case, we tend to process the same data twice.

## What happens if we leave it as it is?
Node.js is single threaded. If the number of items in the list increases, the time spent and the processing done in the event loop will start taking more time. The whole event based programming will become slow. Instead of the event '1' triggering simultaneously from Fetcher to Processor, it will take some more milliseconds than it was required to do so. As a result, there will be a delay built up in the processing time. An event triggered at a time might be processed 30 seconds later.

### How to solve this issue?
There are a numerous ways to solve it. 
1. Let us consider the Fetcher as a different component. Its only task is to fetch the list and then distribute it to the processor. Now we have 2 servers intially. which can be scaled up independently. It is the task of the Fetcher to distribute the websites to different servers. This becomes similar to a master slave system. The number of masters and slaves can be different.
2. Let us take a different design. Each node can be programmed to solve only an allotted number of websites i.e. a server can take up processing of top 500 items in the list and another might take the rest.
You might think of another ways to solve this concern. Solving this issue is easy but time is a major concern. Design is a major concern in large scale applications.

## Summary
Intuitively we don't tend to do a proper segregation of components and look how it would scale because nodejs doesn't wait. Every request is supposed to be asynchronous (writing synchronous code in nodejs is a bad idea since the whole principle is *don't ever write blocking code*). So the application might not suffer when it is working on a small scale but complications can occur when we need to scale up. the whole code should be written in such a way that each component is independent and ready to scale up when required. 
> The key is to write every component as if you were writing a microservice and break each component minutely