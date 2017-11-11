---
title: 通过MySQL 了解B-Tree(多路搜索树，不是二叉树)
date: 2017-10-31 22:30:09
categories:
- MySQL
comments: true
---

MySQL支持多种存储引擎（PRIMARY KEY, UNIQUE, INDEX, and FULLTEXT），但是绝大多数都是以B-Tree最为底层数据结构；特殊的是Memory引擎同时支持B-Tree和Hash；另外在Innodb中全文索引（FULLTEXT）使用的是倒排链表（inverted lists）。

# MySQL为什么选择B-Tree
在了解B-Tree特性之后，这个问题就会变得比较容易回答：答案是B-Tree的综合搜索性能高。   
B-Tree能够满足绝大部分需求，并且满足时间要求苛刻的场景；当然每一种继续都有他擅长的一面，在关系型数据库中大部分索引都在建立在一个或多个列的基础之上的，所以使用B-Tree是合适的。   
B-Tree不适合全文检索的需要，所以在Innodb中MySQL使用了其他的数据结构来处理全文检索。   
同样在大部分搜索引擎中（Lucene，Elasticsearch）也能看到倒排索引的身影。


# 什么是B-Tree



# Hash与B-Tree的对比
