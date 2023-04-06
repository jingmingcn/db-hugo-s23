---
title:  "SQL实践"
tags: ["作业"]
summary: "按照步骤操作，实践使用SQL语句操作数据库的操作，包括：创建数据库、创建表、对数据进行增删改查，另外，针对性掌握Group by、Order by等语句的应用场景。"
---

已知使用以下SQL语句创建了数据库表`employees`.

```sql
CREATE TABLE `employees` (
  `emp_no` int NOT NULL,
  `birth_date` date NOT NULL,
  `age` int NOT NULL,
  `name` varchar(14) NOT NULL,
  `gender` varchar(2) NOT NULL,
  `hire_date` date NOT NULL,
  PRIMARY KEY (`emp_no`)
);
```

问题一：用表格或其他方式描述以上语句创建的表结构。

```
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| emp_no     | int         | NO   | PRI | NULL    |       |
| birth_date | date        | NO   |     | NULL    |       |
| age        | int         | NO   |     | NULL    |       |
| name       | varchar(14) | NO   |     | NULL    |       |
| gender     | varchar(2)  | NO   |     | NULL    |       |
| hire_date  | date        | NO   |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
```

> 只需要标记出字段名，类型，和是否为空，是否为主键。


问题二：使用 `INSERT`语句在`employees`表中插入以下数据：

```text
emp_no : 1001
birth_date : 2000-01-01
age : 21
name : 张三
gender : 男
hire_date : 2021-01-01
```

```sql
INSERT INTO employees (emp_no,birth_data,age,name,gender,hire_date)
VALUES (1001,'2000-01-01','21','张三','男','2021-01-01')
```

> 日期格式一般使用 yyyy-MM-dd 的格式， yyyy代表4位年份，如2000，MM代表2位月份，如01代表1月，dd代表2位日期，如01代表第1天。



问题三：使用 `UPDATE`语句更新`employees`表中的`age`即年龄字段都增加一岁

```sql
update employees set age = age + 1
```

问题四：使用 `SELECT`语句查询`employees`表中年龄在18至30岁之间的员工信息

```sql
select * from employees where age between 18 and 30
```

> 这个问题主要考查between and语法，并且包含给出的两个值。

问题五：使用 `DELETE`语句删除`employees`表中未满18岁（不含）的员工信息

```sql
delete from employees where age < 18
```

> 注意 delete from 中间没有 *

问题六：现在创建一个员工薪资表 `employee_salary`， 员工薪资表记录每个员工每个月发放的薪资金额（月薪），要求包括以下字段：员工编号 `emp_no`, 发放日期 `salary_date`, 发放金额 `salary`， 请给出创建表的`CREATE`语句，并结合对业务的理解创建合适的主码和索引。

```sql
CREATE TABLE `employee_salary` (
  `emp_no` int NOT NULL,
  `salary_date` date NOT NULL,
  `salary` float NOT NULL,
  PRIMARY KEY (`emp_no`,`salay_date`)
);
```

> 注意组合主键的声明方式

问题七：计算员工薪资表中最低、最高、平均的月薪金额。

```sql
select min(salary), max(salary), avg(salary) from employees
```

问题八：查询2021年1月所有员工的薪资表，包括：员工编号、员工姓名、薪资，并按薪资降序排序。（假设每个员工每个月都会领薪水）。

```sql
select t.emp_no, name, salary
from employees t, employee_salary m
where t.emp_no = m.emp_no and m.salary_date = '2021-01-01'
order by salary desc
```

> 考查多表查询的语法，以及order by语法

问题九：计算每个员工领取的薪资总和，给出以下数据：员工编号、员工姓名、总薪资。

```sql
select t.emp_no, name, sum(salary)
from employees t, employee_salary m
where t.emp_no = m.emp_no
group by t.emp_no
```

> 考查Group By语法