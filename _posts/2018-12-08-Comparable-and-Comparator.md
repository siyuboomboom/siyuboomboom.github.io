---
layout: post
title: Camparable and Comparator
categories:
- Java
---

To sort an array of **primitives**, we can simply use Arrays.sort method.  
To sort an array of **objects**, the objects have to be **Comparable**.  

## Comparable
Method: `compareTo()`


{% highlight java %}
public class Card implements Comparable<Card> {
    private String suit;
    private int rank;
    
    public Card(String s, int r) {
        suit = s;
        rank = r;
    }

    public String getSuit() {
        return suit;
    }

    public int getRank() {
        return rank;
    }

    /** 
    * returns negative if this rank < other's rank
    * returns 0 if this rank == other's rank
    * returns positive if this rank > other's rank
    */
    @Override
    public int compareTo(Card other) {
        return Integer.compare(rank, other.rank);
    }
}
{% endhighlight %}   

Note: 
1. `compareTo()` should be consistent to `equals()` and `hashCode()` such that   
`x.equals(y) == true <===> x.compareTo(y) == 0`    
`===> x.hashCode() == y.hashCode()`   
Therefore, we need to implement `compareTo()`, `equals()` and `hashCode()` at the same time. 

2. `compareTo()` method defines the **NATURAL ORDER** of the type
{% highlight java %}
List<Card> cards = new ArrayList<>();
Collections.sort(cards);
{% endhighlight %}    


## Comparator
Method: `compare()`

Compare by suit:
{% highlight java %}
public class CompareBySuit implements Comparator<Card> {
    @Override
    public int compare(Card x, Card y) {
        return x.getSuit().compareTo(y.getSuit());
    }
}
{% endhighlight %}   

Compare by suit first and break the tie by rank:
{% highlight java %}
public class CompareBySuitRank implements Comparator<Card> {
    @Override
    public int compare(Card x, Card y) {
        int suitResult = x.getSuit().compareTo(y.getSuit());
        if (suitResult != 0) {
            return suitResult;
        }
        return Integer.compare(x.getRank(), y.getRank());
    }
}
{% endhighlight %}   

Note:
1. `compare()` method defines different custom orders of the type
2. How to sort cards using this Comparator instead of its natural ordering (provided by the Comparable interface)?
{% highlight java %}
List<Card> cards = new ArrayList<>();
Collections.sort(cards, new CompareBySuit());
{% endhighlight %}   

We can also use anonymous class if we are only going to use the Comparator once. Don't forget the type parameter `<T>` after `new Comparator`.   
{% highlight java %}
PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>(new Comparator<Integer>() {
    public int compare(Integer n1, Integer n2) {
        return map.get(n1) - map.get(n2);
    }
});
{% endhighlight %} 