---
layout: post
title: MySQL · InnoDB ·概述
description: ""
modified: 2014-12-24
tags: [高级MySql]

---
### Innodb是什么？
- Innodb是事务安全的MySQL存储引擎。

最早,InnoDB是由Heikki Tuuri于1995年创立Innobase Oy公司开发并维护的，其被包含在MySQL的所有发行版中，从MySQL5.5开始，InnoDB代替MyIsam成为了MySQL的默认数据库。

### InndoDB特性
InnoDB是第一个完整支持ACID事务的存储引擎，其特点是：

- 行锁设计
- 支持MVCC
- 支持外键
- 提供一致性非锁定读

### InnoDB体系架构

宏观认识六个问题：1. delete table_name速度， 3MB/S 左右1.为什么delete from table_namealter一张表，有多快呢？rename 呢？
3. 宏观认识六个问题：2.为什么innodb 系统io2. innodb io io能力为 2001.本文所有内容，围绕mysql 5.1版本。Innodb版本为plugin版本。
4. 宏观认识六个问题：3.3.为什么机械磁盘的iops <=180 iops1.为什么移动硬盘，在copy数据，声音很大。有人用ssd的移动硬盘么？U盘没声音？
5. 宏观认识六个问题：4.Insert io4.Insert一条记录，会产生物理io io读操作么？1.Update呢，会有物理读io操作么..
6. 宏观认识六个问题：5.merge 为什么只适用于非唯一索引的insert insert insert？INSERT BUFFER AND ADAPTIVE HASH INDEX-------------------------------------Ibuf: size 738, free list len 16770, seg size 17509,28472919 inserts, 28349854 merged recs, 15294548 merges
7. 宏观认识六个问题：6. NULL6.为什么建立索引的字段，不允许默认为NULL NULL？













