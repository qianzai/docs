# Mysql

## 1. 什么是数据库

### 1.1. 数据库简介

1、顾名思义，数据库(DB，database)是按照**数据结构**来**组织**、**存储**和**管理**数据的“仓库”。

2、**数据库**指的是以一定方式储存在一起、能为**多个用户共享**、具有尽可能小的冗余度的特点的、与应用程序彼此独立的数据集合。

3、数据库可以通过统一的一些指令对数据进行**增、删、改、查**（Create，Retrive，Updata，Delete）等操作。

### 1.2. DBMS 与 DBS

1、数据库管理系统（`DBMS`，`Database Management System`），在用户与操作系统之间，由相互关联的数据集合以及一组用于访问这些数据的程序组成。

2、数据库系统（DBS，Database System），是实现有组织的、动态的存储大量关联数据、方便多用户访问的计算机硬件、软件（包括操作系统、应用程序、DBMS）、数据资源、人员（数据库管理员、使用者等）组成的系统。

3、主流数据库管理系统：`Oracle`（甲骨文公司推出）、`MySQL`（开源）、`Microsoft SQL Server`、`SQLite`（轻型、嵌入式、Android 与 iOS 开发都使用该数据库）等。

### 1.3. 数据库分类

1、数据库分为关系型数据库（`sql`数据库）与非关系型数据库（`no-sql`数据库）。两者的区别在于是否使用 SQL 语句作为操作的方式和方法。我们常见的数据库多属于关系型数据库。

2、关系型数据库：数据存放在表（table）中；表的每一行被称为记录（record）：表中所有记录都有相同的字段(列)，每一列代表一个属性。

3、非关系型数据库：NOSQL 数据库一般都是 Key-Value 形式的，它的查询不需要 SQL 语句支持，而是通过查询 key 获得 value 值。相比关系型数据库，查询效率更高、扩展性更好。

4、结构化查询语言（Structured Query Language）是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理数据库系统，也就是一些命令，来增加、删除、修改和查询这个文件里存储的数据；同时 SQL 也是数据库脚本文件的扩展名。

---

## 2. Mysql 简介

- MySQL 是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，目前属于 Oracle 公司。
- MySQL 是最流行的[关系型数据库管理系统](https://baike.baidu.com/item/关系型数据库管理系统/696511)之一，在 WEB 应用方面，MySQL 是最好的 [RDBMS](https://baike.baidu.com/item/RDBMS/1048260) (Relational Database Management System，关系数据库管理系统) 应用软件之一。
- MySQL 是一种关系型数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

![v2-22f78ac55ee443ca0cbcaf15d3e12cfc_1200x500](../../../../../../../../Documents/NetSarang/Xftp/Temporary/media/Mysql.assets/v2-22f78ac55ee443ca0cbcaf15d3e12cfc_1200x500.jpg)

### 2.1. Mysql 特点

1、可以处理拥有上千万条记录的大型数据；
2、支持常见的 SQL 语句规范；
3、可移植行高，安装简单小巧；
4、良好的运行效率，有丰富信息的网络支持；
5、调试、管理，优化简单（相对其他大型数据库）。

### 2.2. Mysql 安装

#### 2.2.1. Linux 安装

> www.baidu.com

#### 2.2.2. docker 安装

```shell
 拉取镜像
$ docker pull mysql:5.7

 运行mysql
$ sudo docker run -d -p 3306:3306 --restart always --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

---

## 3. 数据类型

- MySQL 中定义数据字段的类型对你数据库的优化是非常重要的。

- MySQL 支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。

### 3.1. 数值类型

| 类型            | 大小                                          | 用途               |
| :-------------- | :-------------------------------------------- | :----------------- |
| TINYINT         | 1 byte                                        | 小整数值           |
| SMALLINT        | 2 bytes                                       | 大整数值           |
| MEDIUMINT       | 3 bytes                                       | 大整数值           |
| `INT`或 INTEGER | 4 bytes                                       | 大整数值，int 常用 |
| BIGINT          | 8 bytes                                       | 极大整数值         |
| FLOAT           | 4 bytes                                       | 单精度 浮点数值    |
| DOUBLE          | 8 bytes                                       | 双精度 浮点数值    |
| DECIMAL         | 对 DECIMAL(M,D) ，如果 M>D，为 M+2 否则为 D+2 | 小数值，金融计算   |

### 3.2. 字符串类型

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| `VARCHAR`  | 0-65535 bytes         | **变长字符串（常用）**          |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| `TEXT`     | 0-65 535 bytes        | **长文本数据（常用）**          |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

### 3.3. 日期和时间类型

| 类型      | 大小 ( bytes) | 范围                                                            | 格式                | 用途                     |
| :-------- | :------------ | :-------------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                           | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                        | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                       | YYYY                | 年份值                   |
| DATETIME  | 8             | 1000-01-01 00:00:00/9999-12-31 23:59:59                         | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4             | 时间戳，1970-01-01 00:00:00/2038 结束时间是第 **2147483647** 秒 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

## 4. 数据库的字段属性

### 4.1. Unsigned 无符号

- 无符号的整数
- 声明了该列不能声明为负数

### 4.2. Zerofill 填充零

在数字长度不够的数据前面填充 0，以达到设定的长度。

### 4.3. AUTO_INCREMENT 自增

MySQL 的`AUTO_INCREMENT`属性可以用于在插入新的记录的时候，进行主键自增。

### 4.4. 非空 not Null

假设设置为 not null ，如果不给它赋值，就会报错! .

### 4.5. 默认

给字段设置默认值

## 5. 数据库引擎

`MySQL`数据库有多种存储引擎：比如：`MyISAM`、`InnoDB`、`MERGE`、`MEMORY(HEAP)`、`BDB(BerkeleyDB)`、EXAMPLE、FEDERATED、ARCHIVE、CSV、BLACKHOLE 等等，最常见的也就是`MyISAM`和`InnoDB`了

|              | `MyISAM` | `InnoDB`          |
| ------------ | -------- | ----------------- |
| 事务支持     | ✖        | ✔                 |
| 数据行锁定   | ✖        | ✔                 |
| 外键约束     | ✖        | ✔                 |
| 全文索引     | ✔        | ✖                 |
| 表空间的大小 | 较小     | 较大，MyISAM 两倍 |

- `MyISAM`引擎是一种非事务性的引擎，提供高速存储和检索，以及全文搜索能力，适合数据仓库等查询频繁的应用。
- `InnoDB`则是一种支持事务的引擎。给 MySQL 提供了具有提交，回滚和崩溃恢复能力的事务安全（ACID 兼容）存储引擎。

## 6. Mysql 命令行

### 6.1. 连接数据库

```shell
 mysql -h主机地址 -u用户名 -p用户密码
$ mysql -uroot -p123456
```

```sql
flush privileges;	--刷新权限

exit; --退出连接
```

### 6.2. 操作数据库

> 操作数据库>操作数据库中的表>操作数据库中表的数据

#### 6.2.1. 操作数据库

```sql
--创建数据库
CREATE DATABASE [IF NOT EXISTS] westos;

--删除数据库
DROP DATABASE [IF EXISTS] westos;

--使用数据库
use h1；

-- 查看所有的数据库
show databases;
```

#### 6.2.2. 操作表

```sql
--创建表
CREATE TABLE `t_user` (
  `id` int(4) NOT NULL,
  `name` varchar(30) DEFAULT '匿名',
  `age` int(4) DEFAULT '18',
  `pwd` varchar(30) DEFAULT '123456',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

--查看创建表sql
SHOW CREATE TABLE t_user

describe user; --查看表结构


--修改表
ALTER TABLE `t_user` RENAME `user1`

--增加表字段
ALTER TABLE `user1` ADD COLUMN `age`  int(11)

--修改表字段 约束
ALTER TABLE `user1` MODIFY COLUMN `age`  VARCHAR(11)

-- 修改表字段 ，重命名和约束
ALTER TABLE `user1` CHANGE COLUMN `age`  `age1` INT(3)

-- 删除表的字段
ALTER TABLE `user1` DROP COLUMN `age1`

-- 删除表
DROP TABLE if EXISTS user1
```

## 7. Mysq 数据管理

### 7.1. 外键（了解）

**数据库中为什么不推荐使用外键约束**

> 【强制】不得使用外键与级联，一切切外键概念必须在应用层解决。——阿里 Java 规范

[数据库中为什么不推荐使用外键约束](https://www.cnblogs.com/rjzheng/p/9907304.html)

### 7.2. SQL 语言共分为四大类

**1. 数据定义语言 DDL**

```markdown
- 数据定义语言 DDL 用来创建数据库中的各种对象-----表、视图、索引、同义词、聚簇等如：
- CREATE TABLE/VIEW/INDEX/SYN/CLUSTER
- DDL 操作是隐性提交的！不能 rollback
```

**2 .数据操纵语言 DML**

数据操纵语言 DML 主要有三种形式：

```markdown
- 插入：INSERT

- 更新：UPDATE

- 删除：DELETE
```

**3. 数据查询语言 DQL**

数据查询语言 DQL 基本结构是由 SELECT 子句，FROM 子句，WHERE 子句组成的查询块：

```markdown
- SELECT <字段名表>

- FROM <表或视图名>

- WHERE <查询条件>
```

**4.数据控制语言 DCL**
数据控制语言 DCL 用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。

```markdown
- GRANT：授权。
- ROLLBACK [WORK] TO [SAVEPOINT]：回退到某一点。

回滚---ROLLBACK
回滚命令使数据库状态回到上次最后提交的状态。其格式为：
SQL>ROLLBACK
```

---

### 7.3. 增加 INSERT

MySQL 表中使用 **INSERT INTO** SQL 语句来插入数据。

```sql
--语法
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );

INSERT INTO `t_user` (`id`, `name`, `age`) VALUES ('1', 'bzm', '20')
```

### 7.4. 删除 DELETE

删除 MySQL 数据表中的记录。

```sql
--语法
DELETE FROM table_name [WHERE Clause]

DELETE FROM `t_user` WHERE (`id`='4')
```

> `TRUNCATE table_name`
>
> **总结：**
> `delete`和`truncate table`的最大区别是`delete`可以通过`WHERE`语句选择要删除的记录。但执行得速度不快。而且还可以返回被删除的记录数。而`truncate table`无法删除指定的记录，而且不能返回被删除的记录。但它执行得非常快。

### 7.5. 修改 UPDATE

修改或更新 MySQL 中的数据，使用 SQL UPDATE 命令来操作。

```sql
--语法
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]

--不带WHERE，默认所有的数据
UPDATE `t_user` SET `age`='3' WHERE (`id`='1')
```

### 7.6. 查询 SELECT

SELECT 语句来查询数据。

```sql
--语法
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
    [left | right | inner join table_name2]  -- 联合查询
    [WHERE ...]  -- 指定结果需满足的条件
    [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
    [HAVING]  -- 过滤分组的记录必须满足的次要条件
    [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
    [LIMIT {[offset,]row_count | row_countOFFSET offset}];
    --  指定查询的记录从哪条至哪条
```

**AS 子句**

```sql
-- 这里是为列取别名(当然as关键词可以省略)
SELECT studentno AS 学号,studentname AS 姓名 FROM student;

-- 使用as也可以为表取别名
SELECT studentno AS 学号,studentname AS 姓名 FROM student AS s;

-- 使用as,为查询结果取一个新名字
-- CONCAT()函数拼接字符串
SELECT CONCAT('姓名:',studentname) AS 新姓名 FROM student;
```

**DISTINCT 关键字**

```sql
-- # 查看哪些同学参加了考试(学号)  去除重复项
SELECT * FROM result; -- 查看考试成绩
SELECT studentno FROM result; --  查看哪些同学参加了考试
SELECT DISTINCT studentno FROM result; -- 了解:DISTINCT 去除重复项 , (默认是ALL)
```

**使用表达式的列**

```sql
-- selcet查询中可以使用表达式
SELECT @@auto_increment_increment; -- 查询自增步长
SELECT VERSION(); -- 查询版本号
SELECT 100*3-1 AS 计算结果; -- 表达式

-- 学员考试成绩集体提分一分查看
SELECT studentno,StudentResult+1 AS '提分后' FROM result;
```

### 7.7. WHERE 子句

> 作用：用于检索数据表中 符合条件 的记录

```sql
[WHERE condition1 [AND [OR]] condition2.....
```

| 操作符  | 描述                                           | 实例                  |
| :------ | :--------------------------------------------- | :-------------------- |
| =       | 等号，检测两个值是否相等                       | (A = B) 返回 false。  |
| <>, !=  | 不等于，检测两个值是否相等                     | (A != B) 返回 true。  |
| >       | 大于号，检测左边的值是否大于右边的值           | (A > B) 返回 false。  |
| <       | 小于号，检测左边的值是否小于右边的值           | (A < B) 返回 true。   |
| >=      | 大于等于号，检测左边的值是否大于或等于右边的值 | (A >= B) 返回 false。 |
| <=      | 小于等于号，检测左边的值是否小于或等于右边的值 | (A <= B) 返回 true。  |
| BETWEEN | 选取介于两个值之间的数据范围                   | BETWEEN 1 AND 3。     |
| AND     | &&                                             | 两个都满足返回 true。 |
| OR      | \|\|                                           | 一个满足返回 true。   |

```sql
-- 满足条件的查询(where)
SELECT Studentno,StudentResult FROM result;

-- 查询考试成绩在95-100之间的
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 AND StudentResult<=100;

-- AND也可以写成 &&
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult>=95 && StudentResult<=100;

-- 模糊查询(对应的词:精确查询)
SELECT Studentno,StudentResult
FROM result
WHERE StudentResult BETWEEN 95 AND 100;

-- 除了1000号同学,要其他同学的成绩
SELECT studentno,studentresult
FROM result
WHERE studentno!=1000;

-- 使用NOT
SELECT studentno,studentresult
FROM result
WHERE NOT studentno=1000;
```

#### 7.7.1. 模糊查询

1.  like 是模糊查询关键字
2.  %表示任意多个任意字符
3.  \_表示一个任意字符

```sql
--查询姓黄的学生:
select * from students where name like '黄%';

--查询姓黄并且“名”是一个字的学生:
select * from students where name like '黄_';

--查询姓黄或叫靖的学生:
select * from students where name like '黄%' or name like '%靖';
```

#### 7.7.2. 范围查询

1.  between … and … 表示在一个连续的范围内查询
2.  in 表示在一个非连续的范围内查询

```sql
--查询编号为3至8的学生:
select * from students where id between 3 and 8;

--查询编号不是3至8的男生:
select * from students where (not id between 3 and 8) and gender='男';
```

#### 7.7.3. 空判断查询

```sql
--查询没有填写身高的学生:
select * from students where height is null;
```

### 7.8. 联表查询

[联表查询](https://my.oschina.net/u/4270970/blog/4120280/print)

```sql
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `DeptTB`
-- ----------------------------
DROP TABLE IF EXISTS `DeptTB`;
CREATE TABLE `DeptTB` (
  `dept_id` int(10) NOT NULL AUTO_INCREMENT,
  `dept_name` varchar(30) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`dept_id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ----------------------------
-- Records of DeptTB
-- ----------------------------
INSERT INTO `DeptTB` VALUES ('1', '技术部');
INSERT INTO `DeptTB` VALUES ('2', '市场部');
INSERT INTO `DeptTB` VALUES ('3', '工程部');

-- ----------------------------
-- Table structure for `EmployeeTB`
-- ----------------------------
DROP TABLE IF EXISTS `EmployeeTB`;
CREATE TABLE `EmployeeTB` (
  `employee_id` int(10) NOT NULL AUTO_INCREMENT,
  `employee_name` varchar(30) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `dept_id` int(10) DEFAULT NULL,
  PRIMARY KEY (`employee_id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ----------------------------
-- Records of EmployeeTB
-- ----------------------------
INSERT INTO `EmployeeTB` VALUES ('1', '张三', '1');
INSERT INTO `EmployeeTB` VALUES ('2', '李四', '1');
INSERT INTO `EmployeeTB` VALUES ('3', '王五', '2');
INSERT INTO `EmployeeTB` VALUES ('4', '赵六', '2');
INSERT INTO `EmployeeTB` VALUES ('5', '郑七', null);

```

#### 7.8.1. 内联结查询

```sql
SELECT e.employee_id, e.employee_name, d.dept_name
FROM EmployeeTB AS e, DeptTB AS d
WHERE e.dept_id=d.dept_id;

SELECT e.employee_id, e.employee_name, d.dept_name
FROM EmployeeTB AS e
INNER JOIN DeptTB AS d ON e.dept_id=d.dept_id;
```

_上面两句查询的效果是一样的_

![image-20200609094624365](../../../../../../../../Documents/NetSarang/Xftp/Temporary/media/Mysql.assets/image-20200609094624365.png)

#### 7.8.2. 外联结查询：

##### 7.8.2.1. 左外联结：

```sql
SELECT e.employee_id, e.employee_name, d.dept_name
FROM EmployeeTB AS e
LEFT OUTER JOIN DeptTB AS d ON d.dept_id=e.dept_id;
```

##### 7.8.2.2. 右外联结：

```sql
SELECT d.employee_id, d.employee_name, e.dept_name
FROM DeptTB AS e
LEFT OUTER JOIN EmployeeTB AS d ON d.dept_id=e.dept_id;

SELECT e.employee_id, e.employee_name, d.dept_name
FROM EmployeeTB AS e
RIGHT OUTER JOIN DeptTB AS d ON d.dept_id=e.dept_id;
```

##### 7.8.2.3. 完全外联结：

```sql
SELECT e.employee_id, e.employee_name, d.dept_name FROM EmployeeTB AS e FULL OUTER JOIN DeptTB AS d ON d.dept_id=e.dept_id;

SELECT e.employee_id, e.employee_name, d.dept_name FROM EmployeeTB AS e LEFT OUTER JOIN DeptTB AS d ON d.dept_id=e.dept_id UNION SELECT e.employee_id, e.employee_name, d.dept_name FROM EmployeeTB AS e RIGHT OUTER JOIN DeptTB AS d ON d.dept_id=e.dept_id;
```

#### 7.8.3. 自连接

### 7.9. 分页和排序

#### 7.9.1. 排序

```sql
/*============== 排序 ================
语法 : ORDER BY
    ORDER BY 语句用于根据指定的列对结果集进行排序。
    ORDER BY 语句默认按照ASC升序对记录进行排序。
    如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
    
*/

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
-- 按成绩降序排序
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC
```

#### 7.9.2. 分页

```sql
/*============== 分页 ================
语法 : SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
好处 : (用户体验,网络传输,查询压力)
推导:
    第一页 : limit 0,5
    第二页 : limit 5,5
    第三页 : limit 10,5
    ......
    第N页 : limit (pageNo-1)*pageSzie,pageSzie
    [pageNo:页码,pageSize:单页面显示条数]
    
*/

-- 每页显示5条数据

```

## 8. MySQL 函数

### 8.1. 常用函数

#### 8.1.1. 数据函数

```sql
 SELECT ABS(-8);  /*绝对值*/
 SELECT CEILING(9.4); /*向上取整*/
 SELECT FLOOR(9.4);   /*向下取整*/
 SELECT RAND();  /*随机数,返回一个0-1之间的随机数*/
 SELECT SIGN(0); /*符号函数: 负数返回-1,正数返回1,0返回0*/
```

#### 8.1.2. 字符串函数

```sql
 SELECT CHAR_LENGTH('Qianzai坚持就能成功'); /*返回字符串包含的字符数*/
 SELECT CONCAT('我','爱','程序');  /*合并字符串,参数可以有多个*/
 SELECT INSERT('我爱编程helloworld',1,2,'超级热爱');  /*替换字符串,从某个位置开始替换某个长度*/
 SELECT LOWER('Qianzai'); /*小写*/
 SELECT UPPER('Qianzai'); /*大写*/
 SELECT LEFT('hello,world',5);   /*从左边截取*/
 SELECT RIGHT('hello,world',5);  /*从右边截取*/
 SELECT REPLACE('Qianzai坚持就能成功','坚持','努力');  /*替换字符串*/
 SELECT SUBSTR('Qianzai坚持就能成功',4,6); /*截取字符串,开始和长度*/
 SELECT REVERSE('Qianzai坚持就能成功'); /*反转
 
 -- 查询姓周的同学,改成邹
 SELECT REPLACE(studentname,'周','邹') AS 新名字
 FROM student WHERE studentname LIKE '周%';
```

#### 8.1.3. 日期和时间函数

```sql
 SELECT CURRENT_DATE();   /*获取当前日期*/
 SELECT CURDATE();   /*获取当前日期*/
 SELECT NOW();   /*获取当前日期和时间*/
 SELECT LOCALTIME();   /*获取当前日期和时间*/
 SELECT SYSDATE();   /*获取当前日期和时间*/
 
 -- 获取年月日,时分秒
 SELECT YEAR(NOW());
 SELECT MONTH(NOW());
 SELECT DAY(NOW());
 SELECT HOUR(NOW());
 SELECT MINUTE(NOW());
 SELECT SECOND(NOW());
```

#### 8.1.4. 系统信息函数

```sql
 SELECT VERSION();  /*版本*/
 SELECT USER();     /*用户*/
```

### 8.2. 聚合函数

| 函数名称 | 描述                                                                          |
| -------- | ----------------------------------------------------------------------------- |
| COUNT()  | 返回满足 Select 条件的记录总和数，如 select count(_) 【不建议使用 _，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。                                |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值                                |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。                      |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。                      |

```sql
 -- 聚合函数
 /*COUNT:非空的*/
 SELECT COUNT(studentname) FROM student;
 SELECT COUNT(*) FROM student;
 SELECT COUNT(1) FROM student;  /*推荐*/
 
 -- 从含义上讲，count(1) 与 count(*) 都表示对全部数据行的查询。
 -- count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即不统计字段为null 的记录。
 -- count(*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为null 的记录；
 -- count(1) 用1代表代码行，在统计结果的时候，包含字段为null 的记录 。
 /*
 很多人认为count(1)执行的效率会比count(*)高，原因是count(*)会存在全表扫描，而count(1)可以针对一个字段进行查询。其实不然，count(1)和count(*)都会对全表进行扫描，统计所有记录的条数，包括那些为null的记录，因此，它们的效率可以说是相差无几。而count(字段)则与前两者不同，它会统计该字段不为null的记录条数。
 
 下面它们之间的一些对比：
 
 1）在表没有主键时，count(1)比count(*)快
 2）有主键时，主键作为计算条件，count(主键)效率最高；
 3）若表格只有一个字段，则count(*)效率较高。
 */
 
 SELECT SUM(StudentResult) AS 总和 FROM result;
 SELECT AVG(StudentResult) AS 平均分 FROM result;
 SELECT MAX(StudentResult) AS 最高分 FROM result;
 SELECT MIN(StudentResult) AS 最低分 FROM result;
```

### 8.3. MD5 加密

`md5`的全称是`md5信息摘要算法`（英文：MD5 Message-Digest Algorithm ），一种被广泛使用的**密码散列函数**，可以产生一个 128 位（16 字节，1 字节 8 位）的散列值（常见的是用 32 位的 16 进制表示，比如：0caa3b23b8da53f9e4e041d95dc8fa2c），用于确保信息传输的完整一致。

**新建一个表 testmd5**

```sql
 CREATE TABLE `testmd5` (
   `id` INT(4) NOT NULL,
   `name` VARCHAR(20) NOT NULL,
   `pwd` VARCHAR(50) NOT NULL,
   PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8
```

```sql
--插入一些数据
INSERT INTO testmd5 VALUES(1,'Qianzai','123456'),(2,'qinjiang','456789')

--如果我们要对pwd这一列数据进行加密，语法是：
 update testmd5 set pwd = md5(pwd);
 
 --如果单独对某个用户(如Qianzai)的密码加密：
 INSERT INTO testmd5 VALUES(3,'Qianzai2','123456')
 update testmd5 set pwd = md5(pwd) where name = 'Qianzai2';
 
 --插入新的数据自动加密
INSERT INTO testmd5 VALUES(4,'Qianzai3',md5('123456'));

--查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）
SELECT * FROM testmd5 WHERE `name`='Qianzai' AND pwd=MD5('123456');
```

## 9. 事务

### 9.1. 事务四大特性（ACID）

#### 9.1.1. 原子性（atomicity）：

一个事务必须被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性。

#### 9.1.2. 一致性（consistency）：

数据库总是从一个一致性的状态转换到另一个一致性的状态。在前面的例子中，一致性确保了，即使在执行第三、四条语句之间时系统崩溃，前面执行的语句也不会生效。因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中。

#### 9.1.3. 隔离性（isolation）：

通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。当执行完第三条语句、第四条语句还未开始时，此时有另外一个程序开始运行，则看不到第三条语句做出的改变。

#### 9.1.4. 持久性（durability）：

一旦事务提交，则其所做的修改就会永久保存到数据库中。此时即使系统崩溃，修改的数据也不会丢失。持久性是个有点模糊的概念，因为实际上持久性也分很多不同的级别。有些持久性策略能够提供非常强的安全保障，而有些则未必。而且不可能有能做到 100%的持久性保证策略。

[事务 ACID 理解](https://blog.csdn.net/dengjili/article/details/82468576)

### 9.2. 事务语法

```sql
-- 使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/

-- 注意:
---  1.MySQL中默认是自动提交
---  2.使用事务时应先关闭自动提交

-- 开始一个事务,标记事务的起始点
START TRANSACTION  

-- 提交一个事务给数据库
COMMIT

-- 将事务回滚,数据回到本次事务的初始状态
ROLLBACK

-- 还原MySQL数据库的自动提交
SET autocommit =1;

-- 保存点
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点
```

## 10. 索引

[MySQL 索引背后的数据结构及算法原理](https://blog.codinglabs.org/articles/theory-of-mysql-index.html)

> `MySQL`官方对索引的定义为：**索引（Index）是帮助 MySQL 高效获取数据的数据结构。**提取句子主干，就可以得到索引的本质：**索引是数据结构。**

### 10.1. 索引的分类

[mysql 索引类型以及创建](https://www.jianshu.com/p/7a0c215edb1d)

1.普通索引 2.唯一索引 3.主键索引 4.组合索引 5.全文索引

```sql
/*
方法一：创建表时
    　　CREATE TABLE 表名 (
                字段名1  数据类型 [完整性约束条件…],
                字段名2  数据类型 [完整性约束条件…],
                [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
                [索引名]  (字段名[(长度)]  [ASC |DESC])
                );
方法二：CREATE在已存在的表上创建索引
        CREATE  [UNIQUE | FULLTEXT | SPATIAL ]  INDEX  索引名
                     ON 表名 (字段名[(长度)]  [ASC |DESC]) ;
方法三：ALTER TABLE在已存在的表上创建索引
        ALTER TABLE 表名 ADD  [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                             索引名 (字段名[(长度)]  [ASC |DESC]) ;
                            
                            
删除索引：DROP INDEX 索引名 ON 表名字;
删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;
显示索引信息: SHOW INDEX FROM student;
*/

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况
MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

### 10.2. 索引测试

_建表插入 100w 条数据_

```sql
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '',
`eamil` varchar(50) NOT NULL,
`phone` varchar(20) DEFAULT '',
`gender` tinyint(4) unsigned DEFAULT '0',
`password` varchar(100) NOT NULL DEFAULT '',
`age` tinyint(4) DEFAULT NULL,
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8


-- 插入100万数据.
DELIMITER $$
-- 写函数之前必须要写，标志
CREATE FUNCTION mock_data ()
RETURNS INT
BEGIN
DECLARE num INT DEFAULT 1000000;
DECLARE i INT DEFAULT 0;
WHILE i<num DO
INSERT INTO `app_user`(`name`,`eamil`,`phone`,`gender`)VALUES(CONCAT('用户',i),'19224305@qq.com','123456789',FLOOR(RAND()*2));
SET i=i+1;
END WHILE;
RETURN i;
END;

SELECT mock_data() -- 执行此函数 生成一百万条数据
```

### 10.3. 索引原则

[mysql 建索引的几大原则](https://blog.csdn.net/u013412790/article/details/51612304)

## 11. 权限管理

关于`mysql`的权限简单的理解就是`mysql`允许你做你全力以内的事情，不可以越界。比如只允许你执行`select`操作，那么你就不能执行`update`操作。只允许你从某台机器上连接`mysql`，那么你就不能从除那台机器以外的其他机器连接`mysql`。

### 11.1. 权限说明

| **权限**                | **权限级别**           | **权限说明**                                                                                                            |
| ----------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| CREATE                  | 数据库、表或索引       | 创建数据库、表或索引权限                                                                                                |
| DROP                    | 数据库或表             | 删除数据库或表权限                                                                                                      |
| GRANT OPTION            | 数据库、表或保存的程序 | 赋予权限选项                                                                                                            |
| REFERENCES              | 数据库或表             |                                                                                                                         |
| ALTER                   | 表                     | 更改表，比如添加字段、索引等                                                                                            |
| DELETE                  | 表                     | 删除数据权限                                                                                                            |
| INDEX                   | 表                     | 索引权限                                                                                                                |
| INSERT                  | 表                     | 插入权限                                                                                                                |
| SELECT                  | 表                     | 查询权限                                                                                                                |
| UPDATE                  | 表                     | 更新权限                                                                                                                |
| CREATE VIEW             | 视图                   | 创建视图权限                                                                                                            |
| SHOW VIEW               | 视图                   | 查看视图权限                                                                                                            |
| ALTER ROUTINE           | 存储过程               | 更改存储过程权限                                                                                                        |
| CREATE ROUTINE          | 存储过程               | 创建存储过程权限                                                                                                        |
| EXECUTE                 | 存储过程               | 执行存储过程权限                                                                                                        |
| FILE                    | 服务器主机上的文件访问 | 文件访问权限                                                                                                            |
| CREATE TEMPORARY TABLES | 服务器管理             | 创建临时表权限                                                                                                          |
| LOCK TABLES             | 服务器管理             | 锁表权限                                                                                                                |
| CREATE USER             | 服务器管理             | 创建用户权限                                                                                                            |
| PROCESS                 | 服务器管理             | 查看进程权限                                                                                                            |
| RELOAD                  | 服务器管理             | 执行 flush-hosts, flush-logs, flush-privileges, flush-status, flush-tables, flush-threads, refresh, reload 等命令的权限 |
| REPLICATION CLIENT      | 服务器管理             | 复制权限                                                                                                                |
| REPLICATION SLAVE       | 服务器管理             | 复制权限                                                                                                                |
| SHOW DATABASES          | 服务器管理             | 查看数据库权限                                                                                                          |
| SHUTDOWN                | 服务器管理             | 关闭数据库权限                                                                                                          |
| SUPER                   | 服务器管理             | 执行 kill 线程权限                                                                                                      |

---

| 权限分布 | 可能的设置的权限                                                                                  |
| -------- | ------------------------------------------------------------------------------------------------- |
| 表权限   | 'Select', 'Insert', 'Update', 'Delete', 'Create', 'Drop', 'Grant', 'References', 'Index', 'Alter' |
| 列权限   | 'Select', 'Insert', 'Update', 'References'                                                        |
| 过程权限 | 'Execute', 'Alter Routine', 'Grant'                                                               |

```sql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户  CREATE USER qianzai IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
    - 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
    - 只能创建用户，不能赋予权限。
    - 用户名，注意引号：如 'user_name'@'192.168.1.1'
    - 密码也需引号，纯数字密码也要加引号
    - 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户  RENAME USER qianzai TO qianzai2
RENAME USER old_user TO new_user

-- 设置密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码

-- 删除用户  DROP USER qianzai2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
    - all privileges 表示所有权限
    - *.* 表示所有库的所有表
    - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
    -- 查看当前用户权限
    SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限
```

## 12. Mysql 备份

### 12.1. 备份恢复策略

- 确定要备份的表的存储引擎是事务型还是非事务型，两种不同的存储引擎备份方式在处理数据一致性方面是不太一样的。
- 确定使用全备份还是增量备份。全备份的优点是备份保持最新备份，恢复的时候可以花费更少的时间；缺点是如果数据量大，将会花费很多的时间，并对系统造成较长时间的压力。增量备份相反，只需要备份每天的增量日志，备份时间少，对负载压力也小；缺点就是恢复的时候需要全备份加上次备份到故障前的所有日志，恢复时间长一些。
- 可以考虑采用复制的方法来做异地备份，但不能代替备份，它对数据库的误操作也无能为力。
- 要定期做备份，备份的周期要充分考虑系统可以承受的恢复时间。备份要在系统负载较小的时候进行
- 确保 MySQL 打开 log-bin 选项，有了 binlog，MySQL 才可以在必要的时候做完整恢复，或基于时间点的恢复，或基于位置的恢复。
- 经常做备份恢复测试，确保备份时有效的，是可以恢复的。

> 在 `MySQL `中，逻辑备份的最大优点是对于各种存储引擎都可以用同样的方法来备份；而物理备份则不同，不同的存储引擎有着不同的备份方法，因此，对于不同存储引擎混合的数据库，逻辑备份会简单一点。

### 12.2. 命令备份

```sql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source  备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

## 13. 规范数据库设计

- 节省数据的存储空间
- 能够保证数据的完整性
- 方便进行数据库应用系统的开发

### 13.1. 三大范式（表设计）

[三大范式的通俗理解](https://www.cnblogs.com/wsg25/p/9615100.html)

#### 13.1.1. 第一范式（1NF）：

要求数据库表的每一列都是不可分割的原子数据项。

#### 13.1.2. 第二范式（2NF）：

在 1NF 的基础上，非码属性必须完全依赖于候选码（在 1NF 基础上消除非主属性对主码的部分函数依赖）

第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。

#### 13.1.3. 第三范式（3NF）：

在 2NF 基础上，任何非属性不依赖于其它非主属性（在 2NF 基础上消除传递依赖）

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

#### 13.1.4. 反范式

```markdown
- 优点：减少了连接，可以更好的利用索引进行筛选和排序，对查询操作可以提高性能。
- 缺点：要在数据一致性与查询之间找到平衡点，符合业务场景的设计才是好的设计
```

### 13.2. 存储引擎的选择

#### 13.2.1. InnoDB:

```markdown
1.灾难恢复性好 2.支持 4 中级别的事务，默认事务的隔离级别是 Repeatable Read，事务支持是通过 MVCC 多版本并发控制来提供的。 3.使用行级锁，并发性能高。 4.使用此存储引擎的表，数据的物理组织形式是簇表，数据按主键来组织，即主键索引和数据是在一起的，B+树就是这样的 5.实现缓冲管理，能缓存索引也能缓存数据。 6.支持外键 7.支持热备份
```

#### 13.2.2. MyISAM:

```markdown
1.配合锁，实现操作系统下的复制备份，迁移 2.使用表记锁并发性差 3.支持全文索引 4.主机宕机后，表容易损坏，灾难恢复性不佳 5.无事务支持 6.只缓存索引，数据缓存利用操作系统缓冲区实现的，引发过多系统调用，性能不佳。 7.数据紧凑存储，可以获得更快的索引和更快的全表扫描性能。
```
