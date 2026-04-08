---
title: 数据库表操作
published: 2026-04-08
description: '针对数据库与表的操作，即数据定义语言'
image: ''
tags: [MySQL]
category: '软件测试'
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

操作表前必须`先指定数据库`，执行 `USE 数据库名;`，或通过 `库名.表名` 方式指定归属库。

### 表的创建（CREATE TABLE）

核心作用：定义表结构、字段类型、约束规则、存储属性，是表操作的基础。
```sql
CREATE TABLE [IF NOT EXISTS] 表名 (
    字段名1 数据类型 [字段约束] [COMMENT '字段注释'],
    字段名2 数据类型 [字段约束] [COMMENT '字段注释'],
    ...
    [表级约束]
) [ENGINE=存储引擎] [DEFAULT CHARSET=字符集] [COLLATE=校验规则] [COMMENT='表注释'];
```

### 修改表

核心作用：修改表的字段、约束、属性，大表修改需在业务低峰期执行，避免锁表影响业务。

| **操作场景**        | **SQL 语句**                                                                                                                            | **注意事项**                            |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------|
| **新增字段**        | ALTER TABLE user ADD COLUMN address VARCHAR(200) NOT NULL DEFAULT '' COMMENT '地址' AFTER email;                                        | AFTER 指定字段位置，FIRST 放在首位；不指定默认追加到表末尾 |
| **批量新增字段**      | ALTER TABLE user ADD COLUMN province VARCHAR(50) COMMENT '省份' AFTER address, ADD COLUMN city VARCHAR(50) COMMENT '城市' AFTER province; | 一次修改完成，避免多次 ALTER 大表                |
| **修改字段类型 / 约束** | ALTER TABLE user MODIFY COLUMN address VARCHAR(300) DEFAULT '' COMMENT '详细地址';                                                        | 仅修改属性，不能重命名字段；修改类型需兼容原有数据，避免数据截断    |
| **重命名字段**       | ALTER TABLE user RENAME COLUMN address TO full_address;                                                                               | MySQL 8.0+ 支持；5.7 版本需用 CHANGE 语法    |
| **5.7 兼容重命名字段** | ALTER TABLE user CHANGE COLUMN address full_address VARCHAR(300) DEFAULT '' COMMENT '详细地址';                                           | CHANGE 必须重写完整的字段类型和属性               |
| **删除字段**        | ALTER TABLE user DROP COLUMN full_address;                                                                                            | 高危操作，删除后数据不可恢复，执行前必须备份  

### 表属性全局修改

```sql
-- 1. 重命名表（两种写法）
-- 写法1：推荐，原子操作
ALTER TABLE user RENAME TO user_info;
-- 写法2：兼容旧版本
RENAME TABLE user TO user_info;

-- 2. 修改表的存储引擎（大表慎用，会锁表全表重建）
ALTER TABLE user ENGINE=InnoDB;

-- 3. 修改表的字符集与校验规则（同步修改字段字符集）
ALTER TABLE user CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;

-- 4. 修改表注释
ALTER TABLE user COMMENT='用户基础信息表';

-- 5. 修改自增主键的起始值（只能往大调，不能小于当前最大id）
ALTER TABLE user AUTO_INCREMENT=1000;
```

```sql
-- 1. 重命名表（两种写法）
-- 写法1：推荐，原子操作
ALTER TABLE user RENAME TO user_info;
-- 写法2：兼容旧版本
RENAME TABLE user TO user_info;

-- 2. 修改表的存储引擎（大表慎用，会锁表全表重建）
ALTER TABLE user ENGINE=InnoDB;

-- 3. 修改表的字符集与校验规则（同步修改字段字符集）
ALTER TABLE user CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;

-- 4. 修改表注释
ALTER TABLE user COMMENT='用户基础信息表';

-- 5. 修改自增主键的起始值（只能往大调，不能小于当前最大id）
ALTER TABLE user AUTO_INCREMENT=1000;
```

### 表的删除与清空

| **特性**   | **DROP TABLE**      | **TRUNCATE TABLE** | **DELETE FROM 表名**       |
|----------|---------------------|--------------------|--------------------------|
| **操作类型** | DDL（数据定义语言）         | DDL                | DML（数据操纵语言）              |
| **执行效果** | 删除整个表（结构 + 数据 + 索引） | 清空全表数据，保留结构        | 按条件删除数据，无 WHERE 则清空全表    |
| **事务回滚** | 不可回滚                | 不可回滚               | 可回滚（需开启事务）               |
| **自增主键** | 彻底删除                | 重置为初始值             | 不重置，继续递增                 |
| **执行速度** | 极快                  | 极快（大数据量远优于 DELETE） | 逐行删除，大数据量极慢              |
| **锁机制**  | 元数据锁                | 元数据锁               | 行锁（InnoDB），无 WHERE 会触发表锁 |