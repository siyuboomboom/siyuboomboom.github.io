---
layout: post
title: Technical Inteview 101 - Computer Network
categories:
- blog
---

## 1. TCP v.s. UDP
Both send packets to IP address.
TCP: Connection oriented. Guarantee order of packets, reliable, not missing or duplicate. Guarantee by sending aknowledgement. Resend if incorrect response.
UDP: Quick but not reliable. No aknowledgement. 

## 2. TCP 3-way handshake
TCP Header format:
<img src="https://s3.notfalse.net/wp-content/uploads/2016/12/12003643/TCP-Header-Format.png" height="400" />

**SYN** (Synchronize sequence numbers), **ACK** (Acknowledgment field significant): **control bit** in TCP packet header (0 or 1)
<br><br>
1st way: SYN = 1 && ACK = 0: a connection request, SYN is to synchronise the ISN of the data the client is going to send (client's ISN)   
2nd way: SYN = 1 && ACK = 1: a connection request acception, SYN is server's ISN
3rd way: SYN = 0 && ACK = 1    
After that: SYN = 0 && ACK = 1
<br><br>
**Sequence Number**  
32 bit  
if SYN = 0: sequence number of the first octet (one byte) of data.   
if SYN = 1: ISN (initial sequence number, unique to each connection and device), first octet's sequence number is set to ISN + 1.   
   
**Acknowledgment Number**
If ACK = 1: Ack number = ISN + 1 (first octet's seq, i.e. the sequence number of the data it's expecting to receive)

<img src="https://s3.notfalse.net/wp-content/uploads/2016/12/24160105/Three-way-Handshake-ex2.png" height="400" />

Why 3-way?  
1st and 2nd: to ensure the server can receive data and respond correctly.    
2nd and 3rd: to ensure the client can receive data and respond correctly.    
To prevent a stale (eg. delayed) connection request from establishing connection.

## 3. TCP 4-way termination
1. Client tells server he wants to close the connection
2. Server tells client he get the idea but he's not ready and he needs to ensure data are all sent
3. Server is ready and tells client he can close the connection
4. Client tells server he get the idea. After waiting a while without receiving response, client close the connection.

<img src="https://raw.githubusercontent.com/HIT-Alibaba/interview/master/img/tcp-connection-closed-four-way-handshake.png" height="400" />

Why one more way than 3-way handshake?  
Because in 3-way handshake, server can send ACK and SYN at the same time. ACK to aknowledge that he gets the info, SYN to synchronize the ISN of the data the server is going to send.

## 4. Socket
Socket contains information about:
1. Connection protocol
2. Source IP address
3. Source process port
4. Destination IP address
5. Destination process port

## 5. Why TCP is stateful and HTTP is stateless?
**TCP** maintains state in the form of a **window size** (endpoints tell each other how much data they're ready to receive) and **packet order** (endpoints must confirm to each other when they receive a packet from the other). This state (how much bytes the other guy can receive, and whether or not he did receive the last packet) allows TCP to be **reliable** even over inherently non-reliable protocols.
   
**HTTP** is stateless to improve performance, intended to minimize the time that'd otherwise be spent reestablishing a connection for each request. The stateless design simplifies the server design because there is no need to dynamically allocate storage to deal with conversations in progress. If a client session dies in mid-transaction, no part of the system needs to be responsible for cleaning up the present state of the server. 
  
A **disadvantage of statelessness** is that it may be necessary to include additional information in every request, and this extra information will need to be interpreted by the server.
  
While HTTP and HTTPS are essentially stateless (each request is a valid standalone request per the protocol), **applications** built on top of HTTP and HTTPS aren't necessarily stateless. For instance, a website can require you to visit a login page before sending a message. Even though the request where the client sends a message is a valid standalone request, the application will not accept it unless the client authenticated herself before. This means that the application *implements* state over HTTP.



## Source:
1. <https://notfalse.net/7/three-way-handshake>
2. <https://hit-alibaba.github.io/interview/basic/network/TCP.html>
3. <https://stackoverflow.com/questions/19899236/is-tcp-protocol-stateless-or-not>
4. <https://stackoverflow.com/questions/13200152/why-is-it-said-that-http-is-a-stateless-protocol>
5. <https://en.wikipedia.org/wiki/Stateless_protocol>
