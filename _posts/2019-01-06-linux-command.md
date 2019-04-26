---
layout: post
title: Linux (MacOS) Command
categories:
- Computer Systems
---

### 1. Check which process is using a certain port number
` netstat -vanp tcp | grep <port>`
Then kill: `sudo kill -9 <pid>`

### 2. Check what port number is being used by a process
1. Get pid
`ps -aux | grep <process name>`
2. Get port number using pid
`netstat -nap | grep <pid>`

## 3. nohup
a POSIX command to ignore the HUP (hangup) signal.  

`nohup command &`

## 4. ls
`ls -a` list files including hidden ones starting with '.'   
`ls -l` list files with long format including permission, owner, size, date updated   
`ls -lh` like the previous one but with readable size in B, K, M, ..  
