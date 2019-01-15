---
layout: post
title: Common Linux Command
categories:
- blog
---

### 1. Check what processes are using a certain port number
`netstat -nap | grep <port>`

### 2. Check what port number is being used by a process
1. Get pid
`ps -aux | grep <process name>`
2. Get port number using pid
`netstat -nap | grep <pid>`

## 3. nohup
a POSIX command to ignore the HUP (hangup) signal.  

`nohup command &`