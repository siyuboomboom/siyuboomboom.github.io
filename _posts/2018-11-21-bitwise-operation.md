---
layout: post
title: Bitwise Operation Tricks
categories:
- blog
---

### 1. For any number n with only one bit of '1' except 0
`n & (n - 1) == 0`  
Can be used to check if a positive number is a power of 2.
