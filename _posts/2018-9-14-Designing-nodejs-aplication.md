---
layout: post
title: Designing applications in Node.js!
comments: true
---

The motive of this post is to discuss how to design an application written in Node.js in a scalable fashion. On a wider scope, these principles apply to any event driven programming language. I'll discuss where we went wrong in Walmart's internal monitoring application.

To give an overview of Node.js, it is event based. You can write event emitters and event listeners wich listen to those events. Theoretically, there is no limit to 