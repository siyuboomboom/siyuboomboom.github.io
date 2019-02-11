---
layout: post
title: Decision Tree Algorithm Implementation - Python
categories:
- Information Retrieval and Machine Learning
---

This is a Python implementation of the decision tree algorithm, including **training** on training data points, **predicting** on test data points, and **evaluating** the performance of the tree. Maximum **mutual information** is the criteria to split on a node. It limits the **maximum depth** of the learned tree in order to avoid **overfitting**. This implementation only works on **binary** datasets, i.e., both attributes and labels have at most two values.   
   
### Datasets
Data are stored in csv files, the first row is the names for each attributes and label, the last column is the label.  
In this data sample, there are 10 attributes and the label name is grade. The purpose is to predict if the grade is A or not A.  
{% highlight python %}
M1,M2,M3,M4,M5,P1,P2,P3,P4,F,grade   
notA,notA,A,notA,A,A,A,notA,notA,A,A   
notA,A,A,notA,A,notA,notA,notA,A,notA,notA   
notA,A,A,A,A,notA,A,notA,notA,A,A   
notA,notA,notA,notA,A,A,A,notA,notA,A,notA   
A,notA,A,A,A,A,A,notA,notA,A,A   
...
{% endhighlight %}

### Data Structure of the Tree
Here I use two different structures for internal nodes and leaf for the convenience of checking if reaching the leaf and store different data.  
{% highlight python %}
class Node:
    def __init__(self, data, attr, left_edge, depth):
        # a list of data for this node to split on
        self.data = data  
        # the name of the attribute on which to split the node's data
        self.attr = attr  
        # the value of the attribute that goes to the left child
        self.left_edge = left_edge  
        # the value of the attribute that goes to the right child
        self.right_edge = ""  
        # current depth of the node, root node has a depth of 1
        self.depth = depth  
        # pointer to left child
        self.left = None  
        # pointer to right child
        self.right = None   
        
class Leaf:
    def __init__(self, data, label):
        # a list of data that is partitioned to this leaf
        self.data = data
        # decision label for this leaf
        self.label = label
{% endhighlight %}

### Math
Calculate **entropy** of a vector based on the formula below.  

<img src="/assets/images/i12.png" width="300"/>
{% highlight python %}
def entropy(vector_y):
    if len(vector_y) == 0:
        return 0
    
    flag = vector_y[0]
    count = 0
    for y in vector_y:
        if y == flag:
            count += 1

    prob = count / len(vector_y)
    if prob == 0 or prob == 1:
        return 0
    return -(prob * math.log(prob, 2) +  (1 - prob) * math.log((1 - prob), 2))
{% endhighlight %}

Calculate **mutual information** of two vectors.  
vector_x: vector of one of the attribute.  
vector_y: vector of the label.   
entropy_y: entropy of the label. This argument is to save the repetitive work on calculating entropy of the label.   

{% highlight python %}
def mutual_info(vector_x, vector_y, entropy_y):
    flag = vector_x[0]
    flag_vector_y = []
    non_flag_vector_y = []

    for i in range(0, len(vector_x)):
        if vector_x[i] == flag:
            flag_vector_y.append(vector_y[i])
        else:
            non_flag_vector_y.append(vector_y[i])
    
    flag_cond_entropy = entropy(flag_vector_y)
    non_flag_cond_entropy = entropy(non_flag_vector_y)

    flag_prob = len(flag_vector_y) / len(vector_y)
    cond_entropy = flag_prob * flag_cond_entropy + \
                   (1 - flag_prob) * non_flag_cond_entropy
    return entropy_y - cond_entropy
{% endhighlight %}

### Train
To train a tree based on the original training data, I pass the data, current depth and remaining attributes that can be choosed to split on to the train() function. When the depth reaches the maximum depth or mutual information is zero, the tree will stop spliting and return a Leaf. Otherwise, it will call train() function recursively to get its left and right child, and finally return a node based on the argument data.  
{% highlight python %}
def train(data, depth, remain_attrs):
    if max_depth == depth:
        return Leaf(data, majority_label(data))

    entropy_y = entropy(data[-1])
    max_mutual_info = 0
    best_attr = 0

    for i in remain_attrs:
        info = mutual_info(data[i], data[-1], entropy_y)
        if info > max_mutual_info:
            max_mutual_info = info
            best_attr = i

    if max_mutual_info == 0:
        return Leaf(data, majority_label(data))
    
    node = Node(data, best_attr, data[best_attr][0], depth + 1)

    left_data = []
    right_data = []
    data_transpose = list(zip(*data))
    for line in data_transpose:
        if line[best_attr] == node.left_edge:
            left_data.append(line)
        else:
            node.right_edge = line[best_attr]
            right_data.append(line)

    child_remain_attrs = copy.copy(remain_attrs)
    child_remain_attrs.remove(best_attr)

    left_node = train(list(zip(*left_data)), depth + 1, child_remain_attrs)
    right_node = train(list(zip(*right_data)), depth + 1, child_remain_attrs)
    node.left = left_node
    node.right = right_node
    return node
{% endhighlight %}

This function returns the majority label value of the data, which is used to decide the decision label of a Leaf when reaching maximum depth or mutual information is zero. 
{% highlight python %}
def majority_label(data):
    flag = data[-1][0]
    non_flag = flag
    flag_count = 0
    for y in data[-1]:
        if y == flag:
            flag_count += 1
        else:
            non_flag = y
    
    label = non_flag
    if flag_count / len(data[0]) >= 0.5:
        label = flag

    return label
{% endhighlight %}

### Predict
This function takes each data entry, traverses the tree based on the attribute values, and writes the predicted label to the label_file. It also calculates the error rate of the prediction both on training and test data and writes it to metric_file. 
{% highlight python %}
def predict(root, data, label_file, metric_file, train_or_test):
    label_f = open(label_file, 'a')
    metric_f = open(metric_file, 'a')

    error_count = 0
    for line in data:
        curr = root
        while not isinstance(curr, Leaf):
            attr_val = line[curr.attr]
            if attr_val == curr.left_edge:
                curr = curr.left
            else:
                curr = curr.right
        
        label_f.write(curr.label + '\n')
        if curr.label != line[-1]:
            error_count += 1

    error_rate = error_count / len(data)
    metric_f.write("error(" + train_or_test + "): " + str(error_rate) + "\n")
    label_f.close()
    metric_f.close()
{% endhighlight %}

### Visualization
Finally, I write a print_tree function to visualize the learned tree alongside the training data contained by each node. 

{% highlight python %}
def print_tree(node):
    num1 = count_label1(node.data[-1])
    num2 = len(node.data[-1]) - num1

    print("[%d %s /%d %s]" % (num1, label1, num2, label2))
    if isinstance(node, Leaf):
        return
    
    prefix = ""
    for i in range(0, node.depth):
        prefix += "| "

    print(prefix + attrs[node.attr] + " = " + node.left_edge + ": ", end="")
    print_tree(node.left)
    print(prefix + attrs[node.attr] + " = " + node.right_edge + ": ", end="")
    print_tree(node.right)
{% endhighlight %}

This is the print result for the tree to predict whether a politician is a democrat or republican with maximum depth of 3. 
<img src="/assets/images/i15.png" width="500"/>

### Empirical Analysis
The following charts show the relationship between error rates on training and test data and different maximum depth limits. 
1. Training error is monotonically decreasing when maximum depth increases, which is obvious since as long as the mutual information is greater than zero, the tree will never make further more error on training data. 
2. Test error is greater than training error when maximum depth is large because of overfitting. The tree takes too much noise in the training data into account. 

<img src="/assets/images/i13.png" width="500"/>
<img src="/assets/images/i14.png" width="500"/>





