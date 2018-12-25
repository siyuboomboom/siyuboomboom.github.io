---
layout: post
title: Bitwise Operation Tricks
categories:
- blog
---

1. For any number n with only one bit of '1' except 0
`n & (n - 1) == 0`  
Can be used to check if a positive number is a power of 2.

2. If n is a power of 2, for any integer i: `i % n = i & (n - 1)`   
Therefore, if a number mode a power of 2, it's just dropping its higher bits.  
eg. `86 % 8 = 6 ==> 86 & (8 - 1) = 6`    
&nbsp; &nbsp; 1 0 1 0 1 1 0  (86)    
& 0 0 0 0 1 1 1  (7)    
------------------------     
&nbsp; &nbsp; 0 0 0 0 1 1 0    
That's why Java HashMap's underlying array always has a length of a power of 2, that is, to speed up the modulo operation when finding the corresponding index in the array of a hashcode.   
