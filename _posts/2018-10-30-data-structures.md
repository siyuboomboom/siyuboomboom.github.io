---
layout: post
title: Data Structures - Arrays
categories:
- blog
---


---
## Manipulating arrays

### 1. one method - `clone()` + one immutable field - `length`
{% highlight java %}
int[] g = {1, 2, 3};
int[] h = g.clone() // shallow copy
int len = h.length; // 3
{% endhighlight %}

clone() is **shallow copy**, which means every element's **address** in the original array is copied. If any changes made to the original array's object element, new array will also change.

{% highlight java %}
List<Integer> aList = new ArrayList<>();
aList.add(1);
Object[] a = {aList, 4};
Object[] b = a.clone();

@SuppressWarnings("unchecked")
List<Integer> bList = (List) b[0];
bList.add(2);
System.out.println(a[0]); // [1, 2]
{% endhighlight %}

If not using `@SuppressWarnings("unchecked")`, downcast will cause a warning:

{% highlight java %}
Note: HelloWorld.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.  
{% endhighlight %}

Arrays.equals() will use equals() method to check if elements are equal, not just compare the addresses of the elements
{% highlight java %}
String course = new String("DSAP");
String[] a = {course};
Object[] b = a.clone();

System.out.println(a[0] == b[0]); // true
b[0] = "DSAP";
System.out.println(a[0] == b[0]); // false

System.out.println(Arrays.equals(a, b)); // true
{% endhighlight %}
<br>
### 2. static methods in java.util.Arrays class

Compare two arrays:
{% highlight java %}
int[] a, b;
boolean res = Arrays.equals(a, b);
{% endhighlight %}
The two arrays are considered equal if both arrays contain the same number of elements, and all corresponding pairs of elements in the two arrays are equal.Two objects e1 and e2 are considered equal if (e1==null ? e2==null : e1.equals(e2)). Two array references are considered equal if both are null.

Sort an array:
{% highlight java %}
Arrays.sort(a);
{% endhighlight %}

Copy an array:
{% highlight java %}
int[] newArray = Arrays.copyOf(int[] original, int newLength); // shallow copy
{% endhighlight %}
newLength can be bigger than original.length. newArray will have length of newLength.

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
System.arraycopy(e, 0, f, 0, 3) // shallow copy, f: {1, 2, 3, 0}
{% endhighlight %}
<br>

Resource:
1. https://www.tutorialspoint.com/java/util/arrays_equals_object.htm