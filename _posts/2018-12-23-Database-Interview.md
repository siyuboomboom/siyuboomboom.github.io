---
layout: post
title: Database
categories:
- Database
---

## 1. Composite index
MySQL can only use one index in one query. If multiple columns appear in the search condition, MySQL will pick the most strict index, that is, the one that will retrive the least records.   
We can add EXPLAIN before SELECT to see which index the db choose.  
**Leftmost prefix rule**: a composite index on columns (a, b, c) is like three indexes on (a, b, c), (a, b) and (a). If we search based on (a, c), only the index on a in the composite will be used. For OR clause between conditions on a, b, c, composite index will not be used. 

## 2. When to use index?
1. Indexes speed up where clauses, order by, group by, so add index on columns that will frequently appear in the where, order by and group by clause in select query.

## 3. When not to use index?
1. Indexes slow down inserts and updates, so you want to use them carefully on columns that are FREQUENTLY updated.
2. Tables with only few records, as database will search index table first and then data table.
3. Columns with lots of common data which distribute evenly
