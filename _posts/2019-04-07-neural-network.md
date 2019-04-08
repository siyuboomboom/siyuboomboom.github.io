---
layout: post
title: Build a Handwriting Recognition System using Neural Netowrk
categories:
- Information Retrieval and Machine Learning
---

### Goal
In the below picture, every image in the same row represents a same letter. The goal is to label these images of handwritten letters by implementing a Neural Network from scratch.
<img src="/assets/images/i12.png" width="300"/>


{% highlight python %}
def sigmoid(x):
    return 1 / (1 + math.exp(-x))

def sigmoid_forward(a):
    sig = np.vectorize(sigmoid, otypes=[np.float])
    return sig(a)

def sigmoid_backward(a, z, g_z):
    g_a = []
    for i, z_i in enumerate(z):
        g_a_i = g_z[i] * z_i * (1 - z_i)
        g_a.append(g_a_i)
    return np.array(g_a)

{% endhighlight %}


{% highlight python %}

{% endhighlight %}


{% highlight python %}

{% endhighlight %}