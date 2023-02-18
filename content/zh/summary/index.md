---
title: "课程总结"
menu:
  main:
    weight: 7
---


“数据库系统概论”是一门面向本科生的数据库入门课程，开课目的在于通过课程讲解，让学生对数据库的相关概念有一定理解，学习如何使用关系型数据库理论分析现实中的问题，并给出对应的解决方案。本课程的主要学习重点概括如下：

1. [ ] 数据库系统基本概念
1. [ ] 关系数据库相关概念
    1. [x] 关系、域、笛卡儿积、元组、分量、基数、主码、主属性、非主属性等相关概念
    1. [x] 笛卡儿积的表示和计算
    1. [ ] 关系模型三类完整性约束的概念，及对应案例中的意义。
        1. [x] 实体完整性
        1. [x] 参考完整性
        1. [ ] 用户定义完整性
    1. [x] 关系代数
        1. [x] 集合运算符（并，差，交，笛卡儿积）
        1. [x] 专门的关系运算符（选择，投影，连接）
        1. [ ] 除运算（学生课下扩展自学）
1. [x] SQL语句
    1. [x] SELECT
        1. [x] 单表查询
        1. [x] 多表查询
        1. [x] 排序
        1. [x] 分组
        1. [ ] EXIST
    1. [x] UPDATE
    1. [x] INSERT
        1. [x] 难点：插入子查询结果
    1. [x] DELETE
    1. [x] 空值处理
    1. [x] 掌握索引的应用和原理
1. [x] 数据库设计
    1. [x] 范式概念
        1. 掌握使用范式分析数据库系统的设计
        1. 重点学习教材示例：Student关系模式如何通过模式分解由1NF升级为2NF
    1. [x] 升级范式
    1. [x] 掌握使用ER图描述一个系统的设计
        1. 重点学习教材示例：工厂物资管理系统的ER图表示方法