---
title:  "实验"
menu:
  main:
    weight: 6
---

## 实验教学授课计划 

| 周次 | 星期 | 节次 | 实验地点 | 实验内容 |
| ----:| :---: | :---: |----|----|
|	|	|09-12||[MySQL数据库实践](#lab_1)|
|	|	|09-12||数据库表设计实践|
|	|	|09-12||SQL实践|
|	|	|09-12||[数据库索引实践](#lab_4)|
|	|	|09-12||[Java数据库操作实践](#lab_5)|
|	|	|09-12||信息管理系统Web应用开发实践|
|	|	|09-12||信息管理系统桌面应用开发实践|
|	|	|09-12||综合实验|

#### <a name="lab_1"></a>MySQL数据库实践

1. MySQL数据库开发环境搭建.
    1. 安装MySQL数据库.
    1. 安装Workbench客户端.
    1. 基于命令行和客户端操作实践.
        1. 启动或关闭MySQL服务.
        1. 创建数据库.
        1. 创建用户.
        1. 分配用户权限.
        1. 创建表结构.
        1. 添加测试数据.
1. 实践数据库和表结构的修改操作.
1. 实践数据库的备份和恢复.

MySQL数据库安装和相关注意事项, 参考官方网站

#### <a name="lab_4"></a>数据库索引实践

1. 下载 [Employees](/file/employees_db.zip) 数据集(约35M).

2. 安装数据集. 使用命令 `mysql -t < employees.sql`.

3. 练习以下命令:
    - SHOW DATABASES
    - USE `<database_name>`
    - SHOW TABLES
    - DESC `<table_name>`
    - SELECT * from `<table_name>` limit 0, 10
    - CREATE INDEX `<index_name>` on `<table_name>`(`<column_name>`)
    - DROP INDEX `<index_name>` on `<table_name>`
    - EXPLAIN SELECT * from `<table_name>` where `<conditional_expression>`

#### <a name="lab_5"></a>Java数据库操作实践

1. 使用Eclipse或其他开发工具创建Java Project

2. 下载 [mysql-connector-java-8.0.24.jar](/file/mysql-connector-java-8.0.24.jar), [protobuf-java-3.11.4.jar](/file/protobuf-java-3.11.4.jar), 添加到工程的包依赖中.

3. MySQL创建用户. 两种方式可选:
    - 命令行操作: 
        1. `CREATE USER 'employees'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yoursecretpassword';`
        1. `grant all on employees.* to 'employees'@'localhost';`
    - MySQL Workbench:
        1. 'Server' -> 'Users and Privileges'.
        1. 添加用户. 'Authentication Type' 选择 'Standard'.

4. 示例代码: Employee.java, App.java

Employee.java

```java
public class Employee {
	
	private String empNo;
	private String birthDate;
	private String firstName;
	private String lastName;
	private String gender;
	private String hireDate;
	
	public Employee() {	}
	
	public Employee(String empNo, String birthDate,
			String firstName, String lastName,
			String gender, String hireDate) {
		super();
		this.empNo = empNo;
		this.birthDate = birthDate;
		this.firstName = firstName;
		this.lastName = lastName;
		this.gender = gender;
		this.hireDate = hireDate;
	}

	public String getEmpNo() {
		return empNo;
	}
	public void setEmpNo(String empNo) {
		this.empNo = empNo;
	}
	public String getBirthDate() {
		return birthDate;
	}
	public void setBirthDate(String birthDate) {
		this.birthDate = birthDate;
	}
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	public String getHireDate() {
		return hireDate;
	}
	public void setHireDate(String hireDate) {
		this.hireDate = hireDate;
	}
}
```

App.java
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class App {
	public static void main(String[] args) {
		String sql = "select * from employees limit 0, 10";
		List<Employee> empList = new ArrayList<>();
		try (Connection conn = DriverManager.getConnection(
				"jdbc:mysql://localhost:3306/employees",
				"employees", "passwd");
				Statement stmt = conn.createStatement();
				ResultSet rs = stmt.executeQuery(sql)) {
			while (rs.next()) {
				String empNo = rs.getString("emp_no");
				String birthDate = rs
						.getString("birth_date");
				String firstName = rs
						.getString("first_name");
				String lastName = rs
						.getString("last_name");
				String gender = rs.getString("gender");
				String hireDate = rs
						.getString("hire_date");

				Employee emp = new Employee(empNo,
						birthDate, firstName, lastName,
						gender, hireDate);
				empList.add(emp);
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		for (Employee emp : empList) {
			System.out.println(emp.getEmpNo());
		}
	}
}
```

## Team Project

自选题目的信息管理系统，数据库自选，编程语言自选，推荐MySQL+Java。

4－6人一组。自由分组。

作业验收: 提交软件源码和项目说明书，以及10分钟讲解演示。

要求: 根据熟悉的业务场景自选题目, 要求有一定的开发工作量, 系统实现基本的业务流程, 涵盖需求分析, 数据库设计, 软件编程, 可视化界面设计等内容, 系统界面可以选择桌面应用或Web应用实现.

可选题目示例(不限以下题目): 

* 智能家居传感器监测数据管理系统. 可以通过生成数据的方式模拟实现传感器数据的上传和数据展示. 思考不同类型传感器数据的表结构设计以及数据汇总统计方式. 表字段的索引如何设计, 实现及使用.
* 超市售卖系统. 模拟实现商品的入库, 销售, 盘点库存等功能. 思考商品, 交易等表如何设计, 考虑实现商品打折, 打包促销等特性. 实现数据汇总统计功能, 如: 商品统计, 交易量统计等. 思考索引如何设计.
* 自习室座位预约系统. 模拟实现自习室在空闲时间段内的座位预约功能, 思考教室, 座位, 空闲时间, 预约之间的关系, 以及表结构如何实现. 实现预约, 退约, 空闲座位统计展示等功能. 思考索引如何设计.


[实验报告模板](template.docx)

## 作业1: 数据操作基础（Java语言）

课前准备：自行学习Java语言的基础知识，包括：语法、常用类、IO操作、面向对象概念。

实验目的：

- 掌握Java语言基础语法， 为Hadoop技术的应用作准备。
- 掌握Java语言文件随机访问方法。
- 掌握数据文件的存储方法。
- 掌握唯一性索引的原理和基于Hash算法的索引实现方法。
- 掌握非唯一性索引的原理和基于Hash算法的索引实现方法。


实验内容：

- 下载[工程源代码](stu_db.zip)
- 阅读源代码， 掌握数据文件的基础操作、学生记录基于num唯一索引的创建和维护过程
- 掌握全表扫描的实现方式。基于现有工程源代码，实现基于name（非唯一）的查询。
- 掌握非唯一索引的实现方式。基于现有工程源代码，实现name（非唯一）索引的创建、维护，查询过程。
- 撰写实验报告， 讲解实验过程和主要工作

作业提交：通过作业提交系统上传作业的PDF格式， 文件命名： 学号_姓名_1.pdf , 注意使用英文下划线。

实验总结：（范本）

通过本次项目，深入学习了数据库的索引原理，掌握了基于Hash算法的数据库索引实现方法，
以及自主管理数据文件和索引文件的存储方法；重点掌握了唯一性索引和非唯一性索引的实现方法，
深入理解了两种索引的区别以及应用场景；掌握了基于Java编程语言和Python编程语言的操作系统文件随机读写访问的实现方法。

备注：可选Python语言。下载[工程源代码(python)](stu_db_python.zip), 基于python语言实现本次实验内容和要求。