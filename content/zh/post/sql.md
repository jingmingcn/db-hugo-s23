---
title: "SQL"
---
理解SQL查询的执行顺序可以帮助我们对查询任务进行优化, 特别是处理大型且复杂的查询时, 知道执行顺序可以避免处理大量不需要的数据, 帮助我们创建能快速执行的查询任务.

## select语句执行顺序

SQL语句的语法树如下所示:

```sql
SELECT DISTINCT <TOP_specification> <select_list>
FROM <left_table>
<join_type> JOIN <right_table>
ON <join_condition>
WHERE <where_condition>
GROUP BY <group_by_list>
HAVING <having_condition>
ORDER BY <order_by_list>
```

SQL语句中, 被执行的第一条句是 `FROM` 子句, 而 `SELECT` 子句虽然出现在第一行, 但是并不是最先执行. SQL查询的逻辑处理的各个阶段如下所示:

```sql
 1 FROM clause
 2 ON clause
 3 OUTER clause
 4 WHERE clause
 5 GROUP BY clause
 6 HAVING clause
 7 SELECT clause
 8 DISTINCT clause
 9 ORDER BY clause
10 TOP clause
```

在实践操作中, SQL语句的执行顺序会如上所示, 不太会发现改变, 所以我们通常基于以上的执行顺序对SQL语句进行性能上的优化.

> 但是需要注意的是, SQL语句的实际执行顺序是由数据库的查询处理器决定的, 不同DBMS的执行顺序也会不同.

注意事项:

* 在SELECT子句列表中定义的别名(`AS`)不能在之前的执行阶段中调用. 这个限制是强制性的, 因为当SELECT子句之前的子句(如:FROM子句)被执行时, 一些列值还没有确定.

* 在一些数据库中(如: MySQL), `GROUP BY`和`HAVING`子句允许使用`SELECT`子句中定义的别名, 尽管`GROUP BY`和`HAVING`子句的执行顺序在`SELECT`子句之前.

* 表达式别名不能被同一个`SELECT`列表中的其它表达式调用. 这是因为表达式执行的逻辑顺序是不确定的. 例如: `SELECT a + 1 AS x, x + 1 AS y`, 这条`SELECT`子句可能不按预期执行, 因此不被支持.

* 当使用`INNER JOIN`时, 在`WHERE`子句中使用逻辑表达式和在`ON`子句中使用没有区别, 这是因为`ON`和`WHERE`没有逻辑上的不同(使用`OUTER JOIN` 或 `GROUP BY ALL`选项的情况除外).

* 当使用`GROUP BY`时, `DISTINCT`子句是冗余的. 因此不会从结果集中移除行.