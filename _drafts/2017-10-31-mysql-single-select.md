---
title: MySQL 单表查询学优化
date: 2017-10-31 22:30:09
categories:
- MySQL
comments: true
---

这一章节会比较枯燥，但很重要；涵盖了MySQL查询调优的基础知识，很多是MySQL内部的优化器默认行为必须深入掌握。
```C
handle_select()
   mysql_select()
     JOIN::prepare()
       setup_fields()
     JOIN::optimize()            /* optimizer is from here ... */
       optimize_cond()
       opt_sum_query()
       make_join_statistics()
         get_quick_record_count()
         choose_plan()
           /* Find the best way to access tables */
           /* as specified by the user.          */
           optimize_straight_join()
             best_access_path()
           /* Find a (sub-)optimal plan among all or subset */
           /* of all possible query plans where the user    */
           /* controls the exhaustiveness of the search.   */
           greedy_search()
             best_extension_by_limited_search()
               best_access_path()
           /* Perform an exhaustive search for an optimal plan */
           find_best()
       make_join_select()        /* ... to here */
     JOIN::exec()
```
为了提升阅读兴趣，不妨查看上面MySQL查询的源码，从第五行JOIN::optimize()开始直到最后一行JOIN::exec() MySQL都是在做查询优化，那么到底优化了什么
就是下面需要学习的内容。

# 通用查询优化
 1. 提高SELECT ... WHERE的查询速度的一个万能钥匙就是加索引(Index)。但是每张表的索引数量以及单个索引的列数是有限制的，所以索引不在多而在于合理。
 2. 利用EXPLAIN查看执行计划 (详解EXPLAIN)
 3. 分段优化，一些复杂SQL通常需要根据实际的结构进行分段优化
 4. 尽最大努力减少全表扫描 (full table scan)
 5. 利用ANALYZE TABLE及时刷新表静态数据以便查询优化器(optimizer)得到最优的执行计划
 6. 深入学习优化技术，存储引擎的内部优化技术以及MySQL系统级参数调优等
 7. 检查锁(locking)问题,确保查询速度不是受到其他回话在同一时刻对表的访问
 8. 调整MySQL内存缓存大小


# MySQL如何优化Where条件
MySQL对WHERE条件是有自动优化的，并且这些优化规则对SELECT、DELETE、UPDATE都生效。所以建议你把精力放在SQL语句的整洁性和易读性方面，优化的工作留给MYSQL。
 1. 去除没有必要的括号结构

```
 ((a AND b) AND c OR (((a AND b) AND (c AND d))))
-> (a AND b AND c) OR (a AND b AND c AND d)
```
 2. 常量转换

```
(a<b AND b=c) AND a=5
-> b>5 AND b=c AND a=5
```

 3. 没有Where条件的查询
```MySQL
select count(*) from table;
```
上面的count(\*)在MyISAM和Memory引擎中是非常快的，从表的静态数据中直接返回

 4. 常量表
在多表查询中MySQL会先读常量表，因为常量的数据最快；那么常量表的形式包括以下两种：
  4.1 空表或只有一行的表
  4.2 表的Where条件中含有主键或唯一索引


# 范围查询优化

# 多值相等优化（in 查询 or 查询对比）

# 索引合并优化

# Null查询优化

# left join与right join优化
