---
title: 通过MySQL 了解B-Tree(多路搜索树，不是二叉树)
date: 2017-10-31 22:30:09
categories:
- MySQL
comments: true
---

MySQL支持多种索引类型（PRIMARY KEY, UNIQUE, INDEX, and FULLTEXT），但是绝大多数索引都是以B-Tree最为底层数据结构；特殊的是Memory引擎同时支持B-Tree和Hash；另外在Innodb中全文索引（FULLTEXT）使用的是倒排链表（inverted lists）。

# MySQL为什么选择B-Tree
在了解B-Tree特性之后，这个问题就会变得比较容易回答：答案是B-Tree的综合搜索性能高。   
B-Tree能够满足绝大部分需求，并且满足时间要求苛刻的场景；当然每一种技术都有他擅长的一面，在关系型数据库中大部分索引都建立在一个或多个列的基础之上，所以使用B-Tree是合适的。   
B-Tree不适合全文检索的需要，所以在Innodb中MySQL使用了其他的数据结构来处理全文检索。   
同样在大部分搜索引擎中（Lucene，Elasticsearch）也能看到倒排索引的身影。


# 什么是B-Tree
B-Tree中的B表示Balance，所以又叫平衡树；记住平衡这个特性会更容易理解，更多的简介可以百度/Google得到很多答案，这里指出几个关键的需要掌握的概念。
## B-Tree特性


## 演示B-Tree
假设需要将一组元素（1-20）进行索引，入库的顺序是无序的[13,1,3,2,18,20,4,12,11,10,5,8,7,6,14,9,16,15,17,19]；通过下面几幅图演示一下平衡数是如何平衡的。   

![](/assets/img/database/b-tree-process.png)


进过一段平衡之后，假设我们得到如下一棵B-Tree（只做演示，不一定是最优平衡）
![](/assets/img/database/b-tree.png)

考虑一下经过几步就能找到元素11？

## B-Tree与二分法
B-Tree一定程度上和二分法类似，但是需要搞清楚最根本的区别。B-Tree是数据结构，二分法是算法；数据结构面向的是存储，算法面向的是计算；


# Hash与B-Tree的对比
