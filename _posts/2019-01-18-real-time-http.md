---
layout: post
title: Real Time HTTP Strategies
categories:
- blog
---

Web applications were originally developed around a client/server model, where the Web client is always the initiator of transactions, requesting data from the server. Thus, there was no mechanism for the server to independently send, or push, data to the client without the client first making a request.

Then how to resolve the situation when real data is needed without client reloading the page? There are 3 strategies to achieve real-time data.

### 1. Polling
Client polls server for new information once in a while.

Pros:  
Easy to program server.  
  
Cons:  
1. it might be too frequent causing unnecessary network traffic
2. it might be too infrequent causing missing information
3. requests and responses contain exactly same header data causing unnecessary data transmission

Application:  
Small applications  
  
### 2. Long polling
When client polls server requesting new information, the server holds the request open until new data is available. Once available, the server responds and sends the new information. When the client receives the new information, it immediately sends another request, and the operation is repeated. This effectively emulates a server push feature.

Pros:  
Reduces a large amount of useless pollings when new information are infrequent.  

Cons:  
1. Hard to program
2. Connections are hold by server when no new information causing resources waste.  

Application:  
WebQQ, Facebook IM  

### 3. Streaming
Browser only sends one request.
Drawbacks:
Proxy servers buffer long responses likely causing missing information.


### Sources:
1. https://www.pubnub.com/blog/2014-12-01-http-long-polling/