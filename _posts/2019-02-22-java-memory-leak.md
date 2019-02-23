---
layout: post
title: Memory Leak in Java
categories:
- Java
---

Memory leak happens when an object is no longer used but garbage collection is not able to remove it from the heap.  

## Symptoms of memory leak
1. Severe performance degradation when the application is continuously running for a long time
2. OutOfMemoryError
3. Spontaneous and strange application crashes
4. The application is occasionally running out of connection objects

## Causes of memory leak in Java
### 1. static variables
Static objects remain in heap throughout the whole lifetime of application. We should minimize the use of static field whenever possible. 
### 2. not closing resources
JVM allocates memory for resources such as database connection and input stream. If we don't close it after using it, GC can't remove it from the heap. We should always close resources in finally block or use try-with-resources in Java 7+.   
   
Note: Any object that implements java.lang.AutoCloseable, which includes all objects which implement java.io.Closeable, can be used as a resource.   
{% highlight java %}
try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
    return br.readLine();
}
{% endhighlight %}

### 3. non-static inner class
Non-static inner class holds an implicit reference to its enclosing class. If we use this inner class’ object in our application, then even after our containing class’ object goes out of scope, it will not be garbage collected. If inner class doesn't need access to enclosing class's members, we should declare it to be static.  

### 4. finalize()
If finalize() method is overriden in a class, it's called when JVM decided to garbage collect the instance. The main purpose of a finalizer is to release resources used by objects before they’re removed from the memory. A finalizer can work as the primary mechanism for clean-up operations, or as a safety net when other methods fail.   

However, using finalize() has several disadvantages:   
1. It's out of our control when the garbage collection happens. Thus, we may run out of memory before the resource is cleaned by GC. 
2. GC algorithm is JVM implementation dependent, so a program may run very well on one system while behaving differently at runtime on another.
3. JVM must perform much more operations when constructing and destroying objects containing a non-empty finalizer.
4. If a finalizer throws an exception, the finalization process is canceled, and the exception is ignored, leaving the object in a corrupted state without any notification.

Therefore, we should avoid overriding finalize().

One option is to implement AutoCloseable and override its close() method. 
{% highlight java %}
public class CloseableResource implements AutoCloseable {
    private BufferedReader reader;
 
    public CloseableResource() {
        InputStream input = this.getClass()
          .getClassLoader()
          .getResourceAsStream("file.txt");
        reader = new BufferedReader(new InputStreamReader(input));
    }
 
    public String readFirstLine() throws IOException {
        return reader.readLine();
    }
 
    @Override
    public void close() {
        try {
            reader.close();
        } catch (IOException e) {
            // handle exception
        }
    }
}
{% endhighlight %}

Then we can use try-with-resources to close the resources. 
{% highlight java %}
@Test
public void whenTryWResourcesExits_thenResourceClosed() throws IOException {
    try (CloseableResource resource = new CloseableResource()) {
        String firstLine = resource.readFirstLine();
        assertEquals("baeldung.com", firstLine);
    }
}
{% endhighlight %}

Source:  
1. <https://www.baeldung.com/java-memory-leaks>
2. <https://www.baeldung.com/java-finalize>