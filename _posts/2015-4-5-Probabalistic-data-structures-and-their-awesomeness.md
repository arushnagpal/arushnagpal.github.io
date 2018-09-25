---
layout: post
title: Probabilistic data structures and their Awesomeness!
comments: true
---

In college, there's a class known as Data Structures. But what they teach you is just the same old trivial data structures. The best you might have heard of are segment trees, interval trees, suffix trees. There are a lot of interesting but less  used data strctures which you might want to know out of cusiosity. Here are a few of them:
- Quadtrees
- Bloom filters
- Suffix trie
- Ropes
- Skip lists
- HyperLogLog
- Count-Min Sketch
- Zippers
- Spatial indexes
- etc etc.

Have you ever heard of probabilistic data structures? When processing large data sets, we often want to do some simple checks, such as number of unique items, most frequent items, and whether some items exist in the data set. The common approach is to use some kind of deterministic data structure like HashSet or Hashtable for such purposes. But when the data set we are dealing with becomes very large, such data structures are simply not feasible because the data is too big to fit in the memory. It becomes even more difficult for streaming applications which typically require data to be processed in one pass and perform incremental updates.

## Probabalistic data structures

Probabilistic data structures are a group of data structures that are extremely useful for big data and streaming applications. Generally speaking, these data structures use hash functions to randomize and compactly represent a set of items. Collisions are ignored but errors can be well-controlled under certain threshold. Comparing with error-free approaches, these algorithms use much less memory and have constant query time. They usually support union and intersection operations and therefore can be easily parallelized. 

In this post, we'll discuss an important data structure Bloom filter and how Cassandra uses it in its internal implementation.

### Bloom Filter

A Bloom filter is a bit array of m bits initialized to 0. To add an element, feed it to k hash functions to get k array position and set the bits at these positions to 1. To query an element, feed it to k hash functions to obtain k array positions. If any of the bits at these positions is 0, then the element is definitely not in the set. If the bits are all 1, then the element might be in the set. A Bloom filter with 1% false positive rate only requires 9.6 bits per element regardless of the size of the elements.

![A Bloom filter]({{ site.baseurl }}/post_images/bloomfilter.png "Bloom Filter")

For example, if we have inserted x, y, z into the bloom filter, with k=3 hash functions like the picture above. Each of these three elements has three bits each set to 1 in the bit array. When we look up for w in the set, because one of the bits is not set to 1, the bloom filter will tell us that it is not in the set.

Bloom filter has the following properties:
- False positive is possible when the queried positions are already set to 1. But false negative is impossible.
- Query time is O(k).
- Union and intersection of bloom filters with same size and hash functions can be implemented with bitwise OR and AND operations.
- Cannot remove an element from the set.

#### How cassandra uses it?

Cassandra uses bloom filters to test if any of the SSTables is likely to contain the requested partition key or not, without actually having to read their contents (and thus avoiding expensive IO operations).  
If a bloom filter returns false for a given partition key, then it is absolutely certain that the partition key is not present in the corresponding SSTable; if it returns true, however, then the SSTable is likely to contain the partition key. When this happens, Cassandra will resort to more sophisticated techniques to determine if it needs to read that SSTable or not. Note that bloom filters are consulted for most reads, and updated only during some writes (when a memtable is flushed to disk). You can read more about Cassandra's read path [here](http://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlAboutReads.html).

### HyperLogLog 
It is another data structure widely used in analytical systems for fast cardinality estimation. It is implemented in many popular databases and key-value storages: ClickHouse[1], Vertica[2], VoltDB, Redis, Druid etc.

### Count-min-sketch
It is often used to estimate statistics when you have tight memory constraints.

We'll probably leave their discussion for some other time.