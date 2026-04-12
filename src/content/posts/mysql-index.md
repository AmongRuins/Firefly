---
title: MySQL索引
published: 2026-04-12
description: '关于MySQL索引的大概解析'
image: './images/covers/mysqlindex.jpg'
tags: [MySQL,索引,软件测试]
category: '数据库'
draft: false 
lang: ''
---

索引是 MySQL 高效查询数据的核心数据结构，本质是数据表的 “目录”，通过预先排序、组织数据的 key 值（`创建二叉树`），`避免全表扫描`，将随机磁盘 IO 转为顺序 IO，从而指数级**提升`查询`性能**。

在我们为表的字段添加 `主键(PRIMARY KEY) / UNIQUE` 时，MySQL就`自动`为该字段添加了一个索引，称为主键索引 / 唯一索引。
| **索引类型** | **核心特点**                         | **创建语法**                                                       |
|----------|----------------------------------|----------------------------------------------------------------|
| **主键索引** | 唯一 + 非空，聚簇索引，一个表仅一个              | `建表时`：PRIMARY KEY (id)新增：ALTER TABLE user ADD PRIMARY KEY (id);  |
| **唯一索引** | 索引列值必须唯一，允许 NULL 值（多个 NULL 不算重复） | `CREATE UNIQUE INDEX idx_user_phone ON user(phone);`             |
| **普通索引** | 无约束，仅用于加速查询，最基础的索引               | `CREATE INDEX idx_user_age ON user(age);`                        |
| **联合索引** | 多个列组合而成的索引，需遵循最左前缀原则             | `CREATE INDEX idx_user_name_age ON user(name,age);`              |
| **前缀索引** | 针对长字符串，仅对字段前 N 个字符建索引，减小索引体积     | `CREATE INDEX idx_url ON article(url(15));`                      |
| **全文索引** | 针对大文本的分词检索，支持模糊匹配                | `CREATE FULLTEXT INDEX idx_article_content ON article(content);` |

MySQL的全文索引效果不是很好，一般高级语言都会有框架解决全文搜索的问题。

- **查看表的所有索引**：`SHOW INDEX FROM 表名;`
- **删除索引**：`DROP INDEX 索引名 ON 表名;` 或 `ALTER TABLE 表名 DROP INDEX 索引名;`

**关于索引需要注意以下几点**：
1. **核心字段优先建索引**：只为 WHERE、JOIN ON、ORDER BY、GROUP BY 涉及的列建索引，不要为 SELECT 中的非查询列单独建索引。
2. **避免过度索引与冗余索引**：索引不是越多越好，`过多的索引会占用磁盘空间`，大幅降低 INSERT/UPDATE/DELETE 的写性能。已有idx(a,b)，无需再建idx(a)，属于冗余索引。
3. **列尽量设置为 NOT NULL**：NULL 值会占用索引空间，导致优化器的索引选择、统计计算更复杂，建议给列设置默认值，避免 NULL。
4. **禁止在索引列上做任何计算 / 函数操作**：把计算放到业务层，不要放到 SQL 中，避免索引失效。