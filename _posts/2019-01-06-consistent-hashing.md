---
layout: post
title: Consistent Hashing
categories:
- Web Applications
---

## Problem of using hash
In a storage or cache cluster, like Redis, we might use sharding when a single table is too big. Then on which node can we find the value according to our key? Normal way is to calculate `hash(key) % node_number`, so that we don't iterate through all nodes to the find the key.  
But what if the node_number change, say adding a new node or remove an old node? Now every cache object needs to move, and in that period of data transfering, application can't get data from cache. Here comes in the consistent hashing.  

## How consistent hashing works?
1. assume hash value is an unsigned 32-bit integer, i.e. hash value is between 0 and 2^31 - 1. Imagine this circle starts from 0 to 2^31 - 1 and then comes back to 0.  
2. hash each server (or node), either using its IP or name, now we know each node's position on the circle.  
  
<img src="/assets/images/i6.jpg" width="400"/>

3. hash the key of data, so that we get the position of each data object.  

<img src="/assets/images/i7.jpg" width="400"/>
  
Now if we add a new node, only part of data on node C is moved to node X, other nodes unaffected.    

<img src="/assets/images/i8.jpg" width="400"/>


## Problem of consistent hashing
If nodes are few, it's easy to form this situation where imbalance occurs between nodes. Large amount of data are on node A while very few are on B.  
<img src="/assets/images/i9.jpg" width="300"/>

Virtual nodes solve the problem. For each node, we calculate multiple hash values by for example appending numbers to IP, to generate virtual nodes. Data objects that are mapped to the virtual nodes are mapped to the corresponding real nodes. By increasing node number, nodes are more evenly distributed on the circle. 
<img src="/assets/images/i10.jpg" width="400"/>





### Source:
<https://zhuanlan.zhihu.com/p/34985026>