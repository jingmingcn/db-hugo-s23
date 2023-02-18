---
title: "索引概念"
tags: ["知识点"]
---

## B-Tree和Hash索引比较

理解`B-Tree`和`Hash`数据结构可以帮助预测查询语句在使用这两种数据结构作为索引的不同存储引擎上的运行差别, 特别是在使用`MEMORY`存储引擎时, 你需要选择使用`B-Tree`或`Hash`索引.

### B-Tree索引特性

B-Tree索引可以作用于使用`=`, `>`, `>=`, `<`, `<=`或`BETWEEN`操作符的列比较表达式. 如果`LIKE`比较的参数是一个不以通配符(`%`,`_`)开始的常量字符串, B-Tree索引也可以作用于`LIKE`比较. 例如, 下面的`SELECT`语句在执行时会使用索引:

```sql
SELECT * FROM tbl_name WHERE key_col LIKE 'Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE 'Pat%_ck%';
```

上面的例子中, 第一条语句只会搜索满足条件 `'Patrick' <= key_col < 'Patricl'` 的行; 第二条语句只会搜索满足条件`'Pat' <= key_col < 'Pau' `的行.

而下面的`SELECT`语句在执行时不会使用索引.

```sql
SELECT * FROM tbl_name WHERE key_col LIKE '%Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE other_col;
```

在上面的例子中, 第一条语句中`LIKE`的参数值以通配符开始; 第二条语句中`LIKE`的参数不是常量.

如果查询语句中使用了 `... LIKE '%string%'`, 并且`string`的长度大于三个字符, MySQL会使用 `Turbo Boyer-Moore`算法初始化字符串的模式, 并使用这个模式让搜索执行的更快.

如果`col_name`列创建了索引, 那么使用`col_name IS NULL`条件的搜索也会引入索引.

在`WHERE`条件中没有跨度所有`AND`层级的索引不会被用于优化查询, 即, 为了使用某个索引, 必须在每个`AND`组中使用索引的前缀.

下列`WHERE`条件使用了索引:

```sql
... WHERE index_part1=1 AND index_part2=2 AND other_column=3

    /* index = 1 OR index = 2 */
... WHERE index=1 OR A=10 AND index=2

    /* optimized like "index_part1='hello'" */
... WHERE index_part1='hello' AND index_part3=5

    /* Can use index on index1 but not on index2 or index3 */
... WHERE index1=1 AND index2=2 OR index1=3 AND index3=3;
```

下列`WHERE`条件没有使用索引:

```sql
    /* index_part1 is not used */
... WHERE index_part2=1 AND index_part3=2

    /*  Index is not used in both parts of the WHERE clause  */
... WHERE index=1 OR A=10

    /* No index spans all rows  */
... WHERE index_part1=1 OR index_part2=10
```

在某些情况下, MySQL执行SQL时并不会使用索引, 即使条件中存在索引字段. 发生这种情况的一个场景是, 当优化器预判到使用一个索引可能需要遍历一个表中大部分的行.(这种情况下, 使用表扫描会更快一些, 因为表扫描需要很少的寻址). 然而, 如果一个查询使用了`LIMIT`限制只获取少量的行数据, 则MySQL仍会使用索引, 因为可以更快的找到这些少量的行数据然后返回结果.


### Hash索引特性

`Hash`索引跟`B-Tree`索引相比较, 存在一些不同的特性.

* `Hash`索引只会被用于使用`=`或`<>`运算符的等值比较(执行非常快); 不会被用于比较运算符, 例如: 使用`<`查找一定范围内的值. 依赖于这类单值查找的系统通常被称为: 键值存储; 使用MySQL作为这类应用的数据库, 要尽可能的使用`Hash`索引.
* 优化器不能使用`Hash`索引加速`ORDER BY`操作. (`Hash`索引不能被应用于顺序查找下一条记录)
* 使用`Hash`索引, MySQL不能判定两个值之间大约有多少行记录(区间优化器需要使用这个数值判断使用哪个索引). 如果把`MyISAM`或`InnoDB`表转化为基于Hash的`MEMORY`表, 可能会影响一些查询.
* 只有整键可以被用于查询行.(相比使用`B-Tree`, 任意最左前缀的键可以被用于查找行)

## 什么情况下可以使用低基数索引

1. 当一个可能值与其它值相比出现的频率非常低, 并且要查找这个值. 例如: 很少女性会是色盲, 所以下面的查询可以最大化的获益于`gender`索引.

    ```sql
    SELECT  *
    FROM    color_blind_people
    WHERE   gender = 'F'
    ```
1. 当数据趋向于按组存储在表里:

    ```sql
    SELECT  *
    FROM    records_from_2008
    WHERE   year = 2010
    LIMIT 1
    ```

    上面的例子中, 尽管只有3个不同的年份, 但是早期年份的数据很有可能更早的插入到数据表中, 所以如果不用年份索引, 在找到并返回第一条2010年的数据时, 需要扫描非常多的数据记录.

1. 当需要 `ORDER BY` 或 `LIMIT`的情况:

    ```sql
    SELECT  *
    FROM    people
    ORDER BY
            gender, id
    LIMIT 1
    ```

    如果没有索引则需要调用`filesort`指令, 尽管对于`LIMIT`操作会有一些优化, 但是仍然需要一次全表扫描.

1. 将索引覆盖了查询中用到的所有字段.

    ```sql
    CREATE INDEX (low_cardinality_record, value)

    SELECT  SUM(value)
    FROM    mytable
    WHERE   low_cardinality_record = 3
    ```

1. 将需要使用`DISTINCT`操作时.

    ```sql
    SELECT  DISTINCT color
    FROM    tshirts
    ```

    MySQL会使用`INDEX FOR GROUP-BY`, 并且如果只有很少的 color 值, 这个查询也会很快执行完成, 即使表里有百万条数量级的数据.

> 注意: 如果不需要担心DML操作的性能问题, 那么可以放心创建索引.
