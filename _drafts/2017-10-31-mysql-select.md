---
title: MySQL Select性能优化
date: 2017-10-31 22:30:09
categories:
- MySQL
comments: true
---

# 通用查询优化
 1. 提高SELECT ... WHERE的查询速度的一个万能钥匙就是加索引(Index)。但是每张表的索引数量以及单个索引的列数是有限制的，所以索引不在多而在于合理。
 2. 利用EXPLAIN查看执行计划
 3. 分段优化，一些复杂SQL通常需要根据实际的结构进行分段优化
 4. 尽最大努力减少全表扫描(full table scan)
 5. 利用ANALYZE TABLE及时刷新表静态数据以便查询优化器(optimizer)得到最优的执行计划
 6. 深入学习优化技术，存储引擎的内部优化技术以及MySQL系统级参数调优等
 7. 检查锁(locking)问题,确保查询速度不是受到其他回话在同一时刻对表的访问
 8. 调整MySQL内存缓存大小


# Where条件优化
MySQL对WHERE条件是有自动优化的，并且对SELECT、DELETE、UPDATE都生效。所以建议你把精力放在SQL语句的整洁性和易读性方面，优化的工作留给MYSQL。
 1. 去除没有必要的括号结构

 ```MySQL
 ((a AND b) AND c OR (((a AND b) AND (c AND d))))
-> (a AND b AND c) OR (a AND b AND c AND d)
 ```

# 范围查询优化

# 多值相等优化（in 查询 or 查询对比）

# 索引合并优化

# Null查询优化

# left join与right join优化
