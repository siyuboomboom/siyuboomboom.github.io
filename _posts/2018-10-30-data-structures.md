---
layout: post
title: Data Structures for Application Programmers - Note
categories:
- blog
---


---
## Manipulating arrays

### 1. one method - `clone()` + one immutable field - `length`
{% highlight java %}
int[] g = {1, 2, 3};
int h = g.clone() // shallow copy
int len = h.length; // 3
{% endhighlight %}
<br>
### 2. static methods in java.util.Arrays class

Compare two arrays:
{% highlight java %}
int[] a, b;
boolean b = Arrays.equals(a, b);
{% endhighlight %}

Sort an array:
{% highlight java %}
Arrays.sort(a);
{% endhighlight %}

Copy an array:
{% highlight java %}
int[] d = Arrays.copyOf(a, a.length); // deep copy
{% endhighlight %}

Print an array:
{% highlight java %}
int[] array = {1, 2};
System.out.println(Arrays.toString(array)); // [1, 2]
{% endhighlight %}

A comparison: ArrayList and LinkedList can be printed directly:
{% highlight java %}
List<Integer> list = new ArrayList<>();
list.add(1);
List.add(2);
System.out.println(list); // [1, 2]
{% endhighlight %}
<br>
### 3. System.arraycopy()

Another more flexible way to copy an array:
{% highlight java %}
int[] e = {1, 2, 3, 4};
int[] f = new int[e.length];
System.arraycopy(e, 0, f, 0, 3) // deep copy, f: {1, 2, 3, 0}
{% endhighlight %}
<br>

