---
title: 数据库表操作
published: 2026-04-08
description: ''
image: ''
tags: []
category: ''
draft: false 
lang: ''
---

## SQL 结构化查询语言

SQL（Structured Query Language，结构化查询语言）是`关系型数据库（RDBMS）的国际标准编程语言`，1986 年成为 ANSI 标准，1987 年纳入 ISO 国际标准，核心用于数据库的数据增删改查（CRUD）、结构定义、权限管控、事务管理等全生命周期操作。

它是声明式非过程化语言，用户只需描述「要什么数据」，无需编写底层执行逻辑，由数据库引擎自动优化执行路径，是`所有主流关系型数据库的通用操作语言，不同数据库仅存在少量方言语法差异`，核心标准 SQL 完全通用。

按功能边界，SQL 可分为 5 大核心模块，覆盖数据库全场景操作：
| 分类  | 全称                                   | 核心作用                      |
|-----|--------------------------------------|---------------------------|
| DDL | 数据定义语言（Data Definition Language）     | 定义 / 修改数据库的结构（库、表、索引、视图等） |
| DQL | 数据查询语言（Data Query Language）          | 数据检索与分析，SQL 最核心、最常用的模块    |
| DML | 数据操纵语言（Data Manipulation Language）   | 对表中数据进行增、删、改操作            |
| DCL | 数据控制语言（Data Control Language）        | 管理数据库用户权限与安全              |
| TCL | 事务控制语言（Transaction Control Language） | 保障数据操作的原子性与一致性            |

## 数据库连接与基础配置

连接数据库
```bash
# 本地连接（默认端口3306）
mysql -u root -p

# 远程连接（指定主机、端口）
mysql -h 192.168.1.100 -P 3306 -u root -p

# 连接时指定数据库
mysql -u root -p test_db
```

基础配置查看
```sql
-- 查看数据库版本
SELECT VERSION();

-- 查看当前数据库
SELECT DATABASE();

-- 查看所有数据库
SHOW DATABASES;

-- 查看当前用户
SELECT USER();
```

MySQL 查看命令
```sql
-- 查看实例级全局默认字符集
SHOW VARIABLES LIKE 'character_set_server';
-- 查看实例级全局默认校验规则
SHOW VARIABLES LIKE 'collation_server';
-- 查看指定数据库的字符集和校验规则
SHOW CREATE DATABASE 你的数据库名;
-- 查看当前数据库支持的所有字符集
SHOW CHARACTER SET;
-- 查看指定字符集支持的所有校验规则
SHOW COLLATION WHERE Charset = 'utf8mb4';
```

## 库操作

```sql
-- 创建数据库（指定字符集）
CREATE DATABASE IF NOT EXISTS test_db DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 使用数据库
USE test_db;

-- 删除数据库
DROP DATABASE IF EXISTS test_db;
```

创建数据库时，`显式指定的字符集 / 校验规则 > 实例级配置的默认值`；若不手动指定，完全继承 `my.cnf/my.ini` 中配置的 `character_set_server` 和 `collation_server`。

MySQL 中的 utf8 是 utf8mb3 的别名，仅支持最多 3 字节的 UTF-8 字符，无法存储 emoji、生僻字，生产环境严禁使用，必须用 `utf8mb4`。

校验规则命名规则：字符集_排序算法_特性，`ci = 大小写不敏感`，`cs = 大小写敏感`，`bin = 二进制比较（严格区分大小写、口音、特殊字符）`。

在`命令行`中运行如下指令可`备份数据库`：
```bash
# 备份整个数据库
mysqldump -u root -p -B test_db > E:\test_db_backup.sql

# 备份指定表
mysqldump -u root -p test_db user orders > test_db_tables_backup.sql

# 备份所有数据库
mysqldump -u root -p --all-databases > all_db_backup.sql
```

在`MySQL命令行`中运行如下指令可`恢复数据库`：
```bash
# 恢复数据库
mysql -u root -p test_db < test_db_backup.sql

# 登录后恢复
SOURCE /path/to/test_db_backup.sql;
```

使用 Navicat 执行数据库恢复却提示语法错误，但在在命令行中进入 MySQL 后输入恢复指令却能正常执行。

## 表操作

创建数据库，并为每列指定`类型`
```sql
-- user表结构
CREATE TABLE IF NOT EXISTS `user` (
  `id` INT PRIMARY KEY AUTO_INCREMENT COMMENT '主键ID',
  `name` VARCHAR(50) NOT NULL COMMENT '用户名',
  `age` TINYINT DEFAULT 0 COMMENT '年龄',
  `gender` CHAR(2) COMMENT '性别',
  `email` VARCHAR(100) UNIQUE COMMENT '邮箱',
  `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户表';
```