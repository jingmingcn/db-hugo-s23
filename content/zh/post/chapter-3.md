---
date: 
description: ""
featured_image: ""
tags: ["知识点"]
title: "第三章 关系数据库标准语言SQL"
---

本章主要知识点如下：

* SQL概述
    * SQL (Structured Query Language), 结构化查询语言，是关系数据库的标准语言
* SQL的特点(了解)
    * SQL的动词 (熟记)
* SQL的基本概念
    * SQL支持关系数据库三级模式结构
* 学生-课程数据库 (学会使用数据库工具创建表)
* 数据定义 (了解, 数据定义可以通过可视化工具实现, 不需要记忆SQL语句)
    * 索引
        * B+树,Hash索引的特点
            * B+树索引具有动态平衡的优点
            * HASH索引具有查找速度快的特点
* 数据查询 (熟记并理解运用)
    * 语句格式
    * 单表查询
    * 连接查询
    * 嵌套查询 (项目开发中使用要注意性能问题)
        * IN 与 EXISTS 逻辑的转换
* 数据更新 (熟记, 并理解运用)
    * 插入
    * 更新
    * 删除
* 空值处理
    * 理解NULL的含义
    * NULL值在SQL中的运用
* 视图 (只需要也了解, 项目实践中使用比较少)


## MySQL

### 通配符 (wildcard)

MySQL provides two wildcard characters for constructing patterns: percentage % and underscore _ .

* The percentage ( % ) wildcard matches any string of zero or more characters.
* The underscore ( _ ) wildcard matches any single character.

## 相关知识

### SQL是不是编程语言?

* Turing Machine
* BrainFxxk 编程语言 [https://iot.qlu.edu.cn/brainfxxk](https://iot.qlu.edu.cn/brainfxxk)

Tower of Hanoi Algorithm:

```code
FUNCTION MoveTower(disk, source, dest, spare):
IF disk == 0, THEN:
    move disk from source to dest
ELSE:
    MoveTower(disk-1, source, spare, dest)
    move disk from source to dest
    MoveTower(disk-1, spare, dest, source)
END IF
```

[Tower of Hanoi 动画演示](https://iot.qlu.edu.cn/animation/web/TowerOfHanoieBook.html)

The following flowchart illustrates the execution of a recursive CTE:

![flowchart illustrates the execution of a recursive CTE](../../assets/figures/SQL-Server-Recursive-CTE-execution-flow.png)

> 以下代码可以在MySQL8+版本上运行

```sql
WITH RECURSIVE temp (n, fact) AS 
(SELECT 0, 1 -- Initial Subquery
  UNION ALL 
 SELECT n+1, (n+1)*fact FROM temp -- Recursive Subquery 
        WHERE n < 9)
SELECT * FROM temp;

+------+--------+
| n    | fact   |
+------+--------+
|    0 |      1 |
|    1 |      1 |
|    2 |      2 |
|    3 |      6 |
|    4 |     24 |
|    5 |    120 |
|    6 |    720 |
|    7 |   5040 |
|    8 |  40320 |
|    9 | 362880 |
+------+--------+
```

