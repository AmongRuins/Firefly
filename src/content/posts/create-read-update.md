---
title: 数据库的增删改查
published: 2026-04-10
description: '关于MySQL数据库的增删改查语句'
image: './images/covers/readupdate.png'
tags: [软件测试]
category: 'MySQL数据库'
draft: false 
lang: ''
---

## INSERT 语句

INSERT 是 SQL 标准中数据操作语言（DML） 的核心语句，唯一核心作用是`向数据库表中新增一行或多行数据`，是数据库`写入操作`的基础语句，MySQL、PostgreSQL、SQL Server、Oracle 等所有主流关系型数据库均兼容其标准语法。

统一基于以下用户表 users 来展开说明
```sql
-- 用户表 users
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID（自增主键）',
    user_name VARCHAR(50) NOT NULL COMMENT '用户名（非空约束）',
    age INT COMMENT '年龄',
    email VARCHAR(100) UNIQUE COMMENT '邮箱（唯一约束）',
    create_time DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认当前时间）'
);
```

### 基础语法

**单行插入（`指定列`）**，这是`最规范、兼容性最好、可维护性最强`的写法，生产环境优先使用。
```sql
INSERT INTO 表名 (列名1, 列名2, 列名3, ...)
VALUES (值1, 值2, 值3, ...);
```

核心规则：
- `列名顺序可自定义`，无需和表定义的字段顺序一致
- VALUES 中的值，必须和前面的`列名数量一致、数据类型匹配`(会进行隐式转换)
- `非空且无默认值的字段`，必须出现在列名列表中并赋值

```sql
-- 仅插入必填字段，其余字段用默认值/NULL
INSERT INTO users (user_name, age)
VALUES ('张三', 25);

-- 插入指定多个字段，顺序可自定义
INSERT INTO users (email, user_name, age)
VALUES ('zhangsan@example.com', '张三', 25);
```

---

**单行全列插入**，即`省略列名列表`，直接给全表所有字段赋值。
```sql
INSERT INTO 表名
VALUES (值1, 值2, 值3, ...);
```

这种必须`严格按照表定义的字段顺序`，为所有字段完整赋值，包括自增主键、有默认值的字段，一个都不能少。

```sql
-- 必须按 id, user_name, age, email, create_time 的顺序完整传值
INSERT INTO users
VALUES (2, '李四', 28, 'lisi@example.com', '2026-04-09 12:00:00');
```

⚠️ 致命缺陷：`表结构一旦变更`（新增 / 删除字段、调整字段顺序），语句会直接报错，可维护性极差，生产环境严禁使用。

---

**多行批量插入**，标准 SQL 支持单条 INSERT 插入多行数据，相比循环执行单行 INSERT，能大幅减少数据库 IO、网络交互和事务开销，批量写入性能提升可达数十倍。
```sql
INSERT INTO 表名 (列名1, 列名2, ...)
VALUES 
(值1-1, 值1-2, ...),
(值2-1, 值2-2, ...),
(值3-1, 值3-2, ...);
```

⚠️ 性能提示：单条 INSERT 的 VALUES 行数建议控制在 `1000-5000` 行，总数据量不超过 `16MB`（MySQL 默认限制），避免触发数据库包大小限制，反而导致性能下降。

```sql
-- 一次性插入3条用户数据
INSERT INTO users (user_name, age, email)
VALUES 
('王五', 22, 'wangwu@example.com'),
('赵六', 30, 'zhaoliu@example.com'),
('孙七', 27, 'sunqi@example.com');
```

---

无需手动写 VALUES，直接`将另一个表 / 查询的结果集插入到目标表`，常用于数据迁移、数据备份、表数据同步。
```sql
INSERT INTO 目标表名 (列名1, 列名2, ...)
SELECT 列1, 列2, ... 
FROM 源表名
WHERE 筛选条件;
```

需要谨记，SELECT `查询的列数、数据类型`，必须和目标表的列名列表完全匹配。
```sql
-- 新建用户备份表
CREATE TABLE users_bak LIKE users;

-- 将users表中年龄大于25的用户数据，批量插入到备份表
INSERT INTO users_bak (user_name, age, email, create_time)
SELECT user_name, age, email, create_time
FROM users
WHERE age > 25;
```

## UPDATE 语句

UPDATE 是 SQL 标准中DML 数据操作语言的核心语句，核心功能是`修改数据库表中已存在的行数据`，是业务开发中数据更新的唯一标准语句，MySQL、PostgreSQL 等所有主流数据库均兼容。
```sql
-- 生产环境唯一推荐的标准写法
UPDATE 表名
SET 
    字段1 = 新值1,  -- 多个字段用英文逗号分隔，最后一个字段不加逗号
    字段2 = 新值2,
    字段N = 新值N
WHERE 精准筛选条件; -- 【生死线】必须加！无WHERE=全表数据被修改
```

✅ 核心执行逻辑：先通过 WHERE 筛选出要修改的行，再按 SET 给指定字段赋新值，`仅修改符合条件的行`。

执行更新语句前必须遵守如下规则：
1. **UPDATE 必须加 WHERE 子句**，除非你 100% 明确需要修改全表所有行，否则省略 WHERE 会直接修改整张表的所有数据，造成不可逆的数据损坏。
2. **更新前必须先用 SELECT 校验条件**，执行 UPDATE 前，先用完全相同的 WHERE 条件执行 SELECT，确认筛选出来的行就是你要修改的目标行，避免条件写错导致误改。
3. **必须用事务包裹，可回滚兜底**，UPDATE 属于 DML 语句，非自动提交模式下，执行后不会永久生效，用事务包裹，出错可以直接回滚，是误操作后的救命稻草。
4. **禁止用非索引字段做大批量更新的筛选条件**，大批量更新时，WHERE 条件优先用主键、唯一索引字段，避免全表扫描，同时 InnoDB 只会给符合条件的行加行锁，不会锁全表，避免业务系统阻塞。

## DELETE 语句

DELETE 是 SQL 标准中DML 数据操作语言的核心语句，核心作用是`删除数据库表中符合筛选条件的行数据`，MySQL、PostgreSQL、SQL Server、Oracle 等所有主流关系型数据库均完全兼容标准语法。
```sql
DELETE FROM 表名
[WHERE 筛选条件]  -- 【生死线·必加】仅删除符合条件的行，无WHERE=全表删除
[ORDER BY 排序规则]  -- 部分数据库支持，控制删除顺序
[LIMIT 行数限制];  -- 部分数据库支持，限制单次删除的最大行数
```

⚠️ 【最高优先级安全红线】：DELETE 语句`必须搭配 WHERE 子句使用`！除非你明确需要清空整张表的所有数据，否则省略 WHERE 会直接删除表中全部数据，造成不可逆的生产事故，风险远高于 UPDATE！

核心执行逻辑
1. 先通过 WHERE 子句精准筛选出需要删除的行（无 WHERE 则选中全表所有行）
2. 逐行删除符合条件的数据，同时记录事务日志（支持回滚）
3. 不会修改表结构、索引、字段约束，也不会重置自增主键的序列

⚠️ 风险提示：全表 DELETE 会`逐行记录日志，大表删除速度极慢，会锁表导致业务阻塞`，且非事务模式下数据无法恢复，生产环境非极端场景严禁使用。

执行更新语句前必须遵守如下规则：
1. **DELETE 必须加 WHERE 子句**，除非你 100% 明确需要清空全表，否则永远不要省略 WHERE，这是避免 99% 删除事故的核心准则。
2.**删除前必须先用 SELECT 校验条件**，执行 DELETE 前，先用`完全相同的 WHERE 条件执行 SELECT 语句`，确认筛选出来的行就是你要删除的目标行，避免条件写错导致误删。
3. **永远用事务包裹删除操作**，无论删除单行还是多行，必须先开启事务，执行后核对结果，确认无误再提交，发现问题立即回滚，这是误删后的唯一救命稻草。
4. **禁止用非索引字段做删除筛选条件**，大批量删除时，WHERE 条件优先使用主键、唯一索引字段，避免全表扫描，同时 InnoDB 只会给符合条件的行加行锁，不会锁全表，避免业务系统阻塞。
5. **大表删除严禁直接用 DELETE 全表操作**，百万级以上大表全表删除，用 DELETE 会导致锁表、数据库停住，正确方案是分批删除，或用 TRUNCATE（提前备份数据）。

## SELECT 语句

虽然 SELECT 常被单独称为 DQL（数据查询语言），但它是数据库操作中`使用频率最高`的语句，核心作用是`从表中检索所需数据`。

### 基础查询

查询全表所有数据
```sql
-- 查询user表的所有行和所有列
SELECT * FROM `user`;
```

⚠️ 注意：`SELECT *` 虽然方便，但生产环境`不建议`用（会查询不需要的字段，浪费资源），建议`明确指定`要查询的列。

查询指定列（推荐写法）
```sql
-- 只查询用户名、年龄、邮箱这3列
SELECT user_name, age, email FROM `user`;
```

去重查询（DISTINCT）
```sql
-- 查询所有不重复的年龄（去掉重复值）
SELECT DISTINCT age FROM `user`;
```

### 条件查询（WHERE）：精准筛选数据

`最常用`的查询场景，通过 `WHERE` 子句筛选出符合条件的行。

基础比较条件
```sql
-- 查询id=1的用户
SELECT * FROM `user` WHERE id = 1;

-- 查询年龄大于20岁的用户
SELECT * FROM `user` WHERE age > 20;

-- 查询年龄不等于25岁的用户
SELECT * FROM `user` WHERE age != 25;
```

逻辑条件（AND/OR）
```sql
-- 查询年龄在20到30岁之间的用户（AND：同时满足）
SELECT * FROM `user` WHERE age >= 20 AND age <= 30;

-- 查询年龄小于18岁或大于60岁的用户（OR：满足其一即可）
SELECT * FROM `user` WHERE age < 18 OR age > 60;
```

范围查询（IN/BETWEEN）
```sql
-- 查询id是1、3、5的用户（IN：指定多个值）
SELECT * FROM `user` WHERE id IN (1, 3, 5);

-- 查询年龄在20到30岁之间的用户（BETWEEN：闭区间）
SELECT * FROM `user` WHERE age BETWEEN 20 AND 30;
```

模糊查询（LIKE）
```sql
-- 查询用户名以"张"开头的用户（%：匹配任意多个字符）
SELECT * FROM `user` WHERE user_name LIKE '张%';

-- 查询用户名包含"三"的用户
SELECT * FROM `user` WHERE user_name LIKE '%三%';

-- 查询用户名第二个字是"三"的用户（_：匹配单个字符）
SELECT * FROM `user` WHERE user_name LIKE '_三%';
```

空值查询（IS NULL/IS NOT NULL）
```sql
-- 查询邮箱为空的用户
SELECT * FROM `user` WHERE email IS NULL;

-- 查询邮箱不为空的用户
SELECT * FROM `user` WHERE email IS NOT NULL;
```

❌ 错误写法：`email = NULL`（永远不会匹配到任何行，必须用 `IS NULL`）。

### 排序查询（ORDER BY）

```sql
-- 按年龄从小到大排序（ASC：升序，默认值，可省略）
SELECT * FROM `user` ORDER BY age ASC;

-- 按年龄从大到小排序（DESC：降序）
SELECT * FROM `user` ORDER BY age DESC;

-- 多字段排序：先按年龄降序，年龄相同再按id升序
SELECT * FROM `user` ORDER BY age DESC, id ASC;
```

### 分页查询（LIMIT）：MySQL 专属

业务中最常用的功能，比如 “每页显示 10 条，查看第 2 页”。

```sql
-- 基础语法：LIMIT 偏移量, 每页条数
-- 查询前5条数据（第1页）
SELECT * FROM `user` LIMIT 0, 5;

-- 查询第6到10条数据（第2页，偏移量=5）
SELECT * FROM `user` LIMIT 5, 5;

-- 简化写法：LIMIT 条数（等价于 LIMIT 0, 条数）
SELECT * FROM `user` LIMIT 5;
```

### 聚合查询：统计数据（COUNT/SUM/AVG/MAX/MIN）

对数据进行统计计算，比如 “总共有多少用户”、“平均年龄是多少”。

COUNT：统计行数
```sql
-- 统计user表的总用户数（COUNT(*)：统计所有行，包括NULL值）
SELECT COUNT(*) AS 总用户数 FROM `user`;

-- 统计邮箱不为空的用户数（COUNT(字段)：忽略NULL值）
SELECT COUNT(email) AS 有邮箱的用户数 FROM `user`;
```

✅ 技巧：`AS` 用于给查询结果起别名，让结果更易读。

其他聚合函数
```sql
-- 计算所有用户的年龄总和
SELECT SUM(age) AS 年龄总和 FROM `user`;

-- 计算所有用户的平均年龄
SELECT AVG(age) AS 平均年龄 FROM `user`;

-- 查询最大年龄
SELECT MAX(age) AS 最大年龄 FROM `user`;

-- 查询最小年龄
SELECT MIN(age) AS 最小年龄 FROM `user`;
```

### 分组查询（GROUP BY + HAVING）

对数据进行分组统计，比如 “统计每个年龄段的用户数”。

基础分组
```sql
-- 按年龄分组，统计每个年龄的用户数
SELECT age, COUNT(*) AS 用户数
FROM `user`
GROUP BY age;
```

分组后筛选（HAVING）

`HAVING` 用于对`分组后`的结果进行筛选（`WHERE` 是`分组前`筛选）。

```sql
-- 按年龄分组，统计用户数大于2的年龄
SELECT age, COUNT(*) AS 用户数
FROM `user`
GROUP BY age
HAVING COUNT(*) > 2;
```