---
layout: post
title: When should you use skip lists instead of binary search trees!
comments: true
---

The first question is what exactly are skip lists?
I'll suggest you to refer to [this](https://www.geeksforgeeks.org/skip-list/) page to see what are skip lists. I'll focus more on why and when are they important and better than binary search trees. 


We're used to applying normal considerations like Big-Oh complexity, and memory overhead, locality, and traversal order while comparing 2 data structures. But in concurrent code, we need to consider two additional things to help us pick a data structure that is also sufficiently concurrency-friendly:
- In parallel code, your performance needs likely include the ability to allow multiple threads to use the data at the same time. If this is (or may become) a high-contention data structure, does it allow for concurrent readers and/or writers in different parts of the data structure at the same time? If the answer is, "No," then you may be designing an inherent bottleneck into your system and be just asking for lock convoys as threads wait, only one being able to use the data structure at a time.
- On parallel hardware, you may also care about minimizing the cost of memory synchronization. When one thread updates one part of the data structure, how much memory needs to be moved to make the change visible to another thread? If the answer is, "More than just the part that has ostensibly changed," then again you're asking for a potential performance penalty, this time due to cache sloshing as more data has to move from the core that performed the update to the core that is reading the result.

It turns out that both of these answers are directly influenced by whether the data structure allows truly localized updates. If making what appears to be a small change in one part of the data structure actually ends up reading or writing other parts of the structure, then we lose locality; those other parts need to be locked, too, and all of the memory that has changed needs to be synchronized.

Skip lists are just more amenable to concurrent updates. The most frequently used implementation of a binary search tree is a red-black tree. The concurrent problems come in when the tree is modified it often needs to rebalance. The rebalance operation can affect large portions of the tree, which would require a mutex lock on many of the tree nodes. Inserting a node into a skip list is far more localized, only nodes directly linked to the affected node need to be locked.

### But why aren't they used more frequently?
As far as efficiency is concerned skip lists are comparable to balanced search trees and has a simpler implementation as well. However there are the following negatives (IMHO) that make them not as frequently used as trees (or even hashes):
- skip lists take more space than a balanced tree. I think a skip list takes 4 pointers per node on an average, which is twice as much as a tree.
- skip lists are not cache friendly because they don't optimize locality of reference i.e. related elements tend to stay far apart and not on the same page.
- lack of available implementations. There are far more optimized tree implementations than skip lists.

A perfect example is when MemSQL used Skip lists in its internal implementation. You can check out the lblog post [here](https://www.memsql.com/blog/the-story-behind-memsqls-skiplist-indexes/).