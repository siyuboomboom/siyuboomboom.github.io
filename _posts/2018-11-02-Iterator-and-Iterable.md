---
layout: post
title: Iterable and Iterator
categories:
- blog
---


## Iterable
A representation of a group of data that can be iterated.  
The data structure of this group of data needs to implement the Iterable interface.   
It has to implement the method iterator().

## Iterator
Iterator is to provide an access to a group of data that is private. It is usually a private inner class (non-static nested class) implementing Iterator of the class implementing Iterable.  
It has to implement the methods next() and hasNext().

## Example for LinkedList
{% highlight java %}
public class LinkedList<AnyType> implements Iterable<AnyType> {
    private Node<AnyType> head;

    @Override
    public Iterator<AnyType> iterator() {
        return new LinkedListIterator();
    }

    // non-static nested class 
    // because Iterator has to access its parent class's members - head
    private class LinkedListIterator implements Iterator<AnyType> {
        private Node<AnyType> nextNode;

        LinkedListIterator() {
            nextNode = head;
        }

        @Override
        public boolean hasNext() {
            return nextNode != null;
        }

        @Override
        public AnyType next() {
            if (!hasNode()) {
                throw new NoSuchElementException();
            }

            AnyType result = nextNode.data;
            nextNode = nextNode.next;
            return result;
        }
    }

    // static nested class 
    // because Node doesn't have to access its parent class's members
    private static class Node<AnyType> {
        ...
    }

}

{% endhighlight %}   

  


