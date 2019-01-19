---
layout: post
title: Iterable and Iterator
categories:
- Java
---


## Iterable
A representation of a group of data that can be iterated.  
The data structure of this group of data needs to implement the Iterable interface.   
Method: **iterator()**  

## Iterator
Iterator is to provide an access to a group of data that is private. It is usually a private inner class (non-static nested class) of the class implementing Iterable.  
Methods: **next(), hasNext()**

## Example for LinkedList
{% highlight java %}
public class LinkedList<T> implements Iterable<T> {
    private Node<T> head;

    @Override
    public Iterator<T> iterator() {
        return new LinkedListIterator();
    }

    // non-static nested class 
    // because Iterator has to access its parent class's members - head
    private class LinkedListIterator implements Iterator<T> {
        private Node<T> nextNode;

        LinkedListIterator() {
            nextNode = head;
        }

        @Override
        public boolean hasNext() {
            return nextNode != null;
        }

        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }

            T result = nextNode.data;
            nextNode = nextNode.next;
            return result;
        }
    }

    // static nested class 
    // because Node doesn't have to access its parent class's members
    private static class Node<T> {
        T data;
        Node<T> next;

        Node(T d, Node<T> n) {
            data = d;
            next = n;
        }
    }
}

{% endhighlight %}   


How to use it?
{% highlight java %}
List<Integer> numbers = new LinkedList<>();
numbers.add(1);
numbers.add(2);

Iterator<Integer> itr = numbers.iterator();
while (itr.hasNext()) {
    System.out.println(itr.next());
}
{% endhighlight %}   

Note:  
Non-static nested class: have access to all members of enclosing class (including private).  
Static nested class: have access to static members of enclosing class.  
Why do wu use nested classes?   
1. group classes that are only used in one place
2. increases encapsulation