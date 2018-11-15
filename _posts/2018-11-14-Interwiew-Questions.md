---
layout: post
title: Inteview Questions
categories:
- blog
---

Toutiao:  
What are protocol layers?  
Why do we have layers?  
Do live streams all use UDP? UDP examples?  
> 目前行业里直播还是以TCP占绝对份额，rtmp或者hls都是基于TCP，但是从底层UDP和TCP的特性来看，还是UDP适合做实时性通信，比如直播。如果是为了快速出产品，到不必纠结是TCP还是UDP做直播，大佬们已经把协议、客户端、服务器端都准备好了，都是基于TCP，比如nginx，但是如果是想重造轮子，当然UDP好。为了能集TCP和UDP的有点，Google搞了个QUIC（可以看看 [科普：QUIC协议原理分析](https://zhuanlan.zhihu.com/p/32553477)），偶然机会看到国内公司大牛正在用QUIC实现直播，卡顿和延迟应该会有非常不错的结果。

> 目前常见的直播方案都是上行采用rtmp；下行采用http-flv或者hls，底层都是tcp。利用rtmp协议可以很快搭建一套直播系统，客户端、服务器都有成熟稳定的开源实现。UDP理论上更适合用于直播系统，但开发效率低，周期长。

What does sequence number in TCP header do?  
In TCP, how to differentiate client and server?  
How is hash table implemented? What data structure? How to solve collisions in close addressing?  
Concurrency in hash table. Segment the underlying array and synchronize the segments, how to rehash?  
Sort a singly linked list.  


Source:
1. <https://www.zhihu.com/question/267101516/answer/325732299>
2. <https://www.zhihu.com/question/267101516/answer/319115792>