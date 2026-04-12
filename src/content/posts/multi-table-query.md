---
title: 多表查询
published: 2026-04-11
description: '关于MySQL多表查询的相关语句'
image: './images/covers/multi-table-query.jpg'
tags: [软件测试]
category: 'MySQL数据库'
draft: false 
lang: ''
---

笛卡尔集（Cartesian Product），又称笛卡尔积，是数学中集合论的概念，在数据库中特指两个表中所有行的无条件组合。
- 假设有两个集合 A 和 B，A 中的每个元素与 B 中的每个元素配对，所有可能的组合结果就是笛卡尔积。
- 在数据库中，若对两个表执行查询时`未指定关联条件`，就会产生笛卡尔积，结果`行数 = 表 1 行数 × 表 2 行数`。

创建 emp表、dept表和 salgrade表
```sql
-- 创建数据库
CREATE DATABASE learn_mysql;

-- 使用数据库
USE learn_mysql;

-- 创建雇员表
CREATE TABLE `emp` (
`empno` mediumint(8) unsigned NOT NULL DEFAULT '0',
`ename` varchar(20) COLLATE utf8_bin NOT NULL DEFAULT '""',
`job` varchar(9) COLLATE utf8_bin NOT NULL DEFAULT '""',
`mgr` mediumint(8) unsigned DEFAULT NULL,
`hiredate` date NOT NULL,
`sal` decimal(7,2) NOT NULL,
`comm` decimal(7,2) DEFAULT NULL,
`deptno` mediumint(8) unsigned NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

-- 为雇员表插入数据
INSERT INTO emp VALUES(7369,'SMITH','CLERK',7902,'1990-12-17',800.00,NULL,20),
(7499,'ALLEN','SALESMAN',7698,'1991-2-20',1600.00,300.00,30),
(7521,'WARD','SALESMAN',7968,'1991-2-22',1250.00,500.00,30),
(7566,'JONES','MANAGER',7839,'1991-4-2',2975.00,NULL,20),
(7654,'MARTIN','SALESMAN',7968,'1991-9-28',1250.00,1400.00,30),
(7698,'BLAKE','MANAGER',7839,'1991-5-1',2850.00,NULL,30),
(7782,'CLARK','MANAGER',7839,'1991-6-9',2450.00,NULL,10),
(7788,'SCOTT','ANALYST',7566,'1991-4-19',3000.00,NULL,20),
(7839,'KING','PRESIDENT',NULL,'1991-11-17',5000.00,NULL,10),
(7844,'TURNER','SALESMAN',7698,'1991-9-8',1500.00,NULL,30),
(7900,'JAMES','CLERK',7698,'1991-12-3',950.00,NULL,30),
(7902,'FORD','ANALYST',7566,'1991-12-3',3000.00,NULL,20),
(7934,'MILLER','CLERK',7782,'1991-1-23',1300.00,NULL,10);

-- 创建工资级别表
CREATE TABLE salgrade(
grade MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
losal DECIMAL(17,2) NOT NULL,
hisal DECIMAL(17,2) NOT NULL
);

-- 为工资级别表插入数据
INSERT INTO salgrade VALUES(1,700,1200),
(2,1201,1400),
(3,1401,2000),
(4,2001,3000),
(5,3001,9999);

-- 创建部门表
CREATE TABLE dept(
deptno MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
dname VARCHAR(20) NOT NULL DEFAULT "",
loc VARCHAR(13) NOT NULL DEFAULT ""
);

-- 为部门表插入数据
INSERT INTO dept VALUES(10, 'ACCOUNTING', 'BEIJING'),
(20, 'RESEARCH', 'SHANGHAI'),
(30, 'SALES', 'NANJING'),
(40, 'OPERATIONS', 'CHENGDU');
```

`查询雇员的工资和所在部门`，该问题涉及两张表（emp 和 dept）
```sql
-- 查询雇员的工资和所在部门
-- 雇员、工资在empty表、所在部门在dept表
-- 如果简单查询两张表，会出现笛卡尔集
SELECT * FROM emp,dept;

-- 通过多表的内连接可以解决笛卡尔集
SELECT ename,sal,dname from emp,dept WHERE emp.deptno = dept.deptno;
```


**自连接：表与自身连接，`需用别名区分`**：
```sql
-- 自连接：查询雇员的上级
SELECT worker.ename AS '雇员',boss.ename AS '所属上级' FROM emp worker,emp boss WHERE worker.mgr = boss.empno;
```

将查询出来的记录当做一张`临时表`：
```sql
-- 查询每个部门大于对应部门的平均工资的员工
-- 1. 查询每个部门的平均工资
select deptno,AVG(sal) AS avg_sal FROM emp GROUP BY deptno;
-- 将上述所查询的记录当做一张临时表
SELECT
  ename,sal,emp.deptno,avg_sal
FROM
  emp,
  (SELECT deptno, AVG(sal) AS avg_sal FROM emp GROUP BY deptno) temp
WHERE
  emp.deptno = temp.deptno AND emp.sal > temp.avg_sal;
```

### 子查询

**行子查询：返回`一列单行或多行`**：
```sql
--  子查询:查询与smith相同部门的员工
SELECT deptno FROM emp WHERE ename = 'SMITH';

-- 通过上面查询到smith所在部门作为子条件来查询该部门员工
SELECT * FROM emp WHERE deptno = (
    SELECT deptno FROM emp WHERE ename = 'SMITH'
  );
```

**列子查询：返回`单行多列`**：
```sql
-- 列子查询:查询与allen部门和岗位相同的员工（不包含allen）
select * FROM emp;

-- 查询allen的部门和岗位
SELECT deptno,job FROM emp where ename = 'ALLEN';

-- 根据上面的查询不包含allen的员工
SELECT * FROM emp WHERE (deptno,job) = (
  SELECT deptno,job FROM emp where ename = 'ALLEN'
) AND ename <> 'ALLEN';
```

### 关键字

在 MySQL 中，ALL 和 ANY 是用于子查询比较的关键字，需配合比较运算符（`>、<、=、>=、<=、<>`）使用，用于将`主查询的值与子查询返回的结果集`进行比对。

示例：假设有成绩表 `score`：
| **student_id** | **score** |
|----------------|-----------|
| **1**          | 75        |
| **2**          | 85        |
| **3**          | 90        |

1. ANY（任意一个）：主查询的值只需满足子查询结果集中的任意一个，条件即成立。
- `> ANY`：大于子查询结果的**最小值**
- `< ANY`：小于子查询结果的**最大值**
- `= ANY`：等价于 IN（等于任意一个）

查询 “成绩大于任意一个 80 分以上学生” 的记录（`即只要大于 80 分即可`）：
```sql
-- 返回 90 分的记录（子查询中返回 85,90两条记录）。
SELECT * FROM score
WHERE score > ANY (SELECT score FROM score WHERE score > 80);
```

在 MySQL 中，**`SOME` 和 `ANY` 完全等价**，可互换使用。

2. ALL（所有）：含义：主查询的值必须满足子查询结果集中的所有值，条件才成立。
- `> ALL`：大于子查询结果的**最大值**
- `< ALL`：小于子查询结果的**最小值**
- `<> ALL`：等价于 NOT IN（不等于所有）

查询 “成绩大于所有 80 分以上学生” 的记录（`即需大于最大值 90`）：
```sql
-- 无记录（因为没有成绩大于 90）。
SELECT * FROM score
WHERE score > ALL (SELECT score FROM score WHERE score > 80);
```

自我复制：`insert into 表1 select * from 表1;`

全字段重复（无主键且所有字段完全一样）
```sql
-- 1. 创建临时表，存入去重后的数据
CREATE TABLE 临时表名 LIKE 原表名;
INSERT INTO 临时表名 SELECT DISTINCT * FROM 原表名;

-- 2. 清空原表
TRUNCATE TABLE 原表名;

-- 3. 将数据插回原表
INSERT INTO 原表名 SELECT * FROM 临时表名;

-- 4. 删除临时表
DROP TABLE 临时表名;
```

### 合并查询

MySQL 中的合并查询通常指使用 `UNION` 或 `UNION ALL` 关键字，将多个 SELECT 语句的结果`纵向`合并为一个结果集（区别于 JOIN 的横向关联）。

| **关键字**       | **作用** | **去重**      | **性能**    |
|---------------|--------|-------------|-----------|
| **UNION**     | 合并结果集  | **自动去除重复行**     | 较慢（需去重排序） |
| **UNION ALL** | 合并结果集  | **保留所有行（包括重复）** | 较快（无需去重）  |

- 多个 SELECT 语句的**列数`必须`相同**。
- 对应列的`数据类型必须兼容`（如 INT 和 VARCHAR 需可隐式转换）。
- **列的顺序`必须`一致**。

### 多表连接

MySQL 多表查询中的外连接（Outer Join） 是一种关联查询方式，它会`返回至少一个表中的所有行，即使另一个表中没有匹配的记录（未匹配部分显示为 NULL）`。
| **类型**   | **关键字**                       | **作用说明**                                             |
|----------|-------------------------------|------------------------------------------------------|
| **左外连接** | LEFT JOIN 或 LEFT OUTER JOIN   | 返回左表的所有行，右表未匹配的列显示为 NULL。                            |
| **右外连接** | RIGHT JOIN 或 RIGHT OUTER JOIN | 返回右表的所有行，左表未匹配的列显示为 NULL。                            |
| **全外连接** | FULL JOIN                     | MySQL 不直接支持，需通过 LEFT JOIN + UNION + RIGHT JOIN 模拟实现。 |

