title: 对于数据库的个人总结
date: 2021-06-02 21:21:06
categories:
- 数据库
tags:
- database
---

数据库的底层是存储引擎

数据库的索引方式主要有

* hash
* btree
* LSM tree

SQL语句与关系代数表达式是等价的，用bison来parse

Sqlite是可以参考源代码的SQL数据库实现

leveldb是谷歌开源的键值对存储库，也可以称之为存储引擎

rocksdb是Facebook参考leveldb实现的键值对存储库（引擎）

很多分布式数据库（例如tidb），底层都使用rocksdb，上层自己再使用一些技术（如raft，分区等）