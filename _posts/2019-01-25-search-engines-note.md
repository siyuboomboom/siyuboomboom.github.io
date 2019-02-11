---
layout: post
title: Introduction to Information Retrieval - Reading Summary
categories:
- Information Retrieval and Machine Learning
---

This article is my reading summary and understanding of the book *Introduction to Information Retrieval*  

## 1 Boolean Retrieval

To search for words in a number of documents, a most intuitive way is to scan through the documents and check if the words being searched exist. However, if the number of documents is very big, or if we want to do more flexible search, this is not very efficient.   

One way to speed up the process is to make changes to the documents themselves, i.e. indexing. We can extract words from all the documents to form a set of words. A table can be used to describe the relationship between these words and all the documents, where rows are words, columns are documents, entries denote whether the word appears in the document. 

However, this table will be too sparse since the collection of words is very large, most words don’t appear in a certain document. An alternative way is to store the relationship in a more unstructured way than a table. Each word can be mapped to a list in which elements denote the ID of the document that the word appears. After all, there’s no need to store both kinds of documents that whether or not contain the word. One kind is stored while the other is implicitly suggested as it’s a Boolean relationship. 

These two approaches suit well on Boolean queries where terms are organized by operators OR, AND, NOT. Take AND as an example. If a list of query terms is organized by AND operators, the most efficient way to get the result documents is to make the intermediate result as small as possible. The intermediate result is calculated as the intersection of two lists of two words. To make the intersection small, we need to pick two shortest lists. 

## 2 Statistical properties of terms
Heap’s Law states that the relationship between log of distinct terms and log of all terms is linear. It indicates that as the size of collection grows, the size of dictionary can grow to a very large size instead of to an upper limit.

Zipf’s Law states the relationship between frequency of a term and its frequency rank. 


## 3 Positional postings and phrase queries
Considering only single words in queries is not enough. It’s more likely for people to search for a phrase or a combination of words. 

To take such phrase queries into consideration, one intuitive thought is to expand the dictionary and add phrases into it.  We can consider each pair of consecutive words in the document as a phrase and add it into the dictionary. This is biword indexing. Of course, the phrase length can be greater than 2, so we can add those phrases of variable length as well. Sometimes there are function words in phrases such as “of”, “on”, “for”, like “votes for the treaty”. We can classify words as function words and nonfunctional words. When extracting phrases from a document, just skip over the nonfunction words.

Obviously, this approach expands the dictionary to a very large extent. Here’s where positioning index comes in. Keeping the dictionary containing only single words, instead of just recording document ID for each appearance of the word, we can also record the positions and frequency of the word in each document. In this way, by calculating the offset of the words in a query phrase and comparing it with the offset between positions of the words in dictionary, we get to know whether this phrase appears in the document or not. To optimize the merge process, we should start with merging two least frequent words so that the intermediate result is small. 

Positioning index also expands the size of the index. Instead of recording appearances of the word in documents, we also record each appearance of the word within one document. Hence, the size of the index is proportional to the size of the total number of words in all documents instead of total number of documents. 

A combination can be utilized of positioning index and phrase index. Merging two words in a commonly used phrase is expensive since we need to find the intersection of the posting lists of the two words and compare the offset of the positions of the two words in the intersection documents with the offset in the query. If each word in a phrase is highly frequent, this process can be extremely slow since the posting list of each word is quite long. If each single word in a phrase is frequent while the phrase itself is less frequent, like Michael Jackson, it’s more efficient to include the phrase in dictionary. In contrast, it’s less efficient to include a phrase with infrequent single word, like Barack Obama, since the appearance of either Barack or Obama highly likely suggests the appearance of the phrase Barack Obama. 

   
## 4 Term frequency and weighting
Score plays an important role in determining whether a document should be selected for a certain query. It’s natural to assign a weight to a query term in a document to measure how important it is to the document and sum up the weight to get the score. A simple way to get the weight is to make it equal to the frequency of the term occurrence in the document. By getting a set of term frequency, we treat the collection as a bag of word without taking care of the order. 

But not every term is equally important. If a term appears too often in all documents, it barely suggests relevance between query and document. Therefore, we measure a term’s collection frequency to get a sense about how common a term is. However, it’s more common to use document frequency to avoid the situation where a term appears too often in only a few documents, which generates high collection frequency while still remain meaningful. 

Rare terms have high inverse document frequency while frequent term has low idf. By multiplying idf and tf, we get an unbiased weight for query term in each document. Idf-tf prefers those terms with high frequency in few documents. By adding up idf-tf for each query term in one document, we get the score for this document in terms of this query. 

## 5 The vector space model for scoring
Each document can be represented as a vector with terms and their weights. Hence a collection of documents is a set of vectors, which can be viewed as a term-document matrix. We can use cosine similarity of two vectors to measure the similarity of the two documents. For cosine similarity, by dividing dot product of vectors by their Euclidean lengths, we are able to avoid the bias caused by difference of document length. Cosine similarity is useful in providing more similar content based on user history. 

Queries can also be represented as vectors so that we can find cosine similarity between query and document. 



### Sources:  
*Introduction to Information Retrieval*, Christopher D. Manning, Prabhakar Raghavan, and Hinrich Schutze, Cambridge University Press. 2008. <https://nlp.stanford.edu/IR-book/>