---
layout: post
title: Google's Map Reduce Engineer Round Table Discussion
categories:
- Big Data
---

Map: Applicational function to every single item

Reduce: Aggregation of those results of the applicational function

### Main point of implementing MapReduce library:
Most complicated part in parallel computing is failure handling. MapReduce solves this problem and hide the detail in the library.  
Handling failure without MapReduce is like:  
Write checkpoints like load descriptors about how far I've got in processing each input and generating output. If there's a failure, try to find the right checkpoint to restart.

