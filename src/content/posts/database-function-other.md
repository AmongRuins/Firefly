---
title: 数据库函数
published: 2026-04-10
description: '数据库常用函数'
image: './images/covers/database-function-other.jpg'
tags: [MySQL,函数]
category: '数据库'
draft: false 
lang: ''
---

## 函数

### 字符串函数

字符串拼接（最常用）：
| **函数**             | **标准语法**                                                    | **核心功能**                 | **示例 SQL**                                                                 | **执行结果**     | **关键提示**                                      |
|--------------------|-------------------------------------------------------------|--------------------------|----------------------------------------------------------------------------|--------------|-----------------------------------------------|
| **CONCAT()**       | CONCAT(s1,s2,...sn)                                         | 拼接多个字符串 / 字段             | SELECT CONCAT('MySQL',' ','8.0');                                          | MySQL 8.0    | 任意一个参数为 NULL，整体返回 NULL，拼接字段需用IFNULL(字段,'')预处理 |
| **CONCAT_WS()**    | CONCAT_WS(分隔符,s1,s2,...sn)                                  | 带分隔符安全拼接（With Separator） | SELECT CONCAT_WS('-','2026','04','09');                                    | 2026-04-09   | 自动跳过 NULL 值，分隔符必填，是多字段拼接的首选                   |
| **GROUP_CONCAT()** | GROUP_CONCAT([DISTINCT] 字段 [ORDER BY 排序字段] [SEPARATOR 分隔符]) | 分组行转列，将多行值拼接为单个字符串       | SELECT dept,GROUP_CONCAT(user_name SEPARATOR '、') FROM user GROUP BY dept; | 研发部、张三、李四、王五 | MySQL 特有高频函数，默认最大拼接长度 1024 字节，超长会静默截断         |

长度计算：
| **函数**            | **标准语法**       | **核心功能**   | **示例 SQL**                     | **执行结果** | **关键提示**                                 |
|-------------------|----------------|------------|--------------------------------|----------|------------------------------------------|
| **CHAR_LENGTH()** | CHAR_LENGTH(s) | 返回字符串的字符个数 | SELECT CHAR_LENGTH('你好MySQL'); | 7        | 中文、英文、数字、emoji 均计为 1 个字符，统计文本字数首选        |
| **LENGTH()**      | LENGTH(s)      | 返回字符串的字节长度 | SELECT LENGTH('你好MySQL');      | 11       | UTF-8 下中文占 3 字节、emoji 占 4 字节，常用于校验字段存储上限 |

子串截取
| **函数**          | **标准语法**                      | **核心功能**                   | **示例 SQL**                          | **执行结果** | **关键提示**                                                          |
|-----------------|-------------------------------|----------------------------|-------------------------------------|----------|-------------------------------------------------------------------|
| **SUBSTRING()** | SUBSTRING(s, start[, length]) | 从指定位置截取子串，别名SUBSTR()/MID() | SELECT SUBSTRING('HelloMySQL',6,5); | MySQL    | 1. 索引从 1 开始，start=0 返回空；2. start 为负数时从字符串末尾倒数；3. 省略 length 则截取到末尾 |
| **LEFT()**      | LEFT(s, length)               | 从左侧截取指定长度的子串               | SELECT LEFT('HelloMySQL',5);        | Hello    | 简化版 SUBSTRING，length 为 0 / 负数返回空                                  |
| **RIGHT()**     | RIGHT(s, length)              | 从右侧截取指定长度的子串               | SELECT RIGHT('HelloMySQL',5);       | MySQL    | 常用于取固定后缀，如手机号后 4 位                                                |

子串查找定位
| **函数**       | **标准语法**                    | **核心功能**        | **示例 SQL**                          | **执行结果** | **关键提示**                              |
|--------------|-----------------------------|-----------------|-------------------------------------|----------|---------------------------------------|
| **INSTR()**  | INSTR(母串s, 子串sub)           | 返回子串在母串中首次出现的位置 | SELECT INSTR('HelloMySQL','MySQL'); | 6        | 找不到子串返回 0，大小写敏感受字段排序规则影响              |
| **LOCATE()** | LOCATE(sub, s[, start_pos]) | 从指定位置开始查找子串位置   | SELECT LOCATE('l','HelloMySQL',4);  | 4        | 比 INSTR 多了自定义起始查找位置的能力，参数顺序与 INSTR 相反 |

字符串替换
| **函数**        | **标准语法**                          | **核心功能**          | **示例 SQL**                                 | **执行结果** | **关键提示**                                    |
|---------------|-----------------------------------|-------------------|--------------------------------------------|----------|---------------------------------------------|
| **REPLACE()** | REPLACE(s, old_sub, new_sub)      | 全局替换字符串中所有匹配的子串   | SELECT REPLACE('HelloMySQL','Hello','Hi'); | HiMySQL  | 大小写敏感受排序规则影响，old_sub 为空时返回原串                |
| **INSERT()**  | INSERT(s, start, length, new_sub) | 从指定位置开始，替换指定长度的字符 | SELECT INSERT('HelloMySQL',1,5,'Hi');      | HiMySQL  | start 为 0 / 超出字符串长度返回原串；length 为负数时替换到字符串末尾 |


空格 / 指定字符修剪
| **函数**      | **标准语法**                                         | **核心功能**                     | **示例 SQL**                     | **执行结果**   | **关键提示**                                                                    |
|-------------|--------------------------------------------------|------------------------------|--------------------------------|------------|-----------------------------------------------------------------------------|
| **TRIM()**  | TRIM([[BOTH/LEADING/TRAILING] [rem_str] FROM] s) | 去除字符串首尾 / 左侧 / 右侧的指定字符（默认空格） | SELECT TRIM('  HelloMySQL  '); | HelloMySQL | BOTH = 首尾（默认）、LEADING = 左、TRAILING = 右；支持去除自定义字符，如TRIM('x' FROM 'xxTestxx') |
| **LTRIM()** | LTRIM(s)                                         | 仅去除字符串左侧的空格                  | SELECT LTRIM('  HelloMySQL');  | HelloMySQL | 仅能去空格，去自定义字符用TRIM(LEADING)                                                  |
| **RTRIM()** | RTRIM(s)                                         | 仅去除字符串右侧的空格                  | SELECT RTRIM('HelloMySQL  ');  | HelloMySQL | 同上                                                                          |


大小写转换 & 固定长度填充
| **函数**              | **标准语法**                     | **核心功能**       | **示例 SQL**                  | **执行结果**   | **关键提示**                         |
|---------------------|------------------------------|----------------|-----------------------------|------------|----------------------------------|
| **UPPER()/UCASE()** | UPPER(s)                     | 字符串全部转大写       | SELECT UPPER('HelloMySQL'); | HELLOMYSQL | 对中文无效果                           |
| **LOWER()/LCASE()** | LOWER(s)                     | 字符串全部转小写       | SELECT LOWER('HelloMySQL'); | hellomysql | 同上                               |
| **LPAD()**          | LPAD(s, target_len, pad_str) | 左侧用指定字符填充至目标长度 | SELECT LPAD('123',6,'0');   | 000123     | 若原串长度 > target_len，会从右侧截断原串，而非填充 |
| **RPAD()**          | RPAD(s, target_len, pad_str) | 右侧用指定字符填充至目标长度 | SELECT RPAD('123',6,'0');   | 123000     | 同上                               |

进阶函数
| **函数**                        | **核心功能**                             | **示例 SQL**                                       | **执行结果**     |
|-------------------------------|--------------------------------------|--------------------------------------------------|--------------|
| **REVERSE(s)**                | 反转字符串                                | SELECT REVERSE('MySQL');                         | LQSyM        |
| **STRCMP(s1,s2)**             | 字符串比较：s1<s2 返回 - 1，相等返回 0，s1>s2 返回 1 | SELECT STRCMP('a','b');                          | -1           |
| **ELT(n, s1,s2,...sn)**       | 返回第 n 个字符串，n 从 1 开始                  | SELECT ELT(2,'Java','MySQL','Python');           | MySQL        |
| **FIELD(sub, s1,s2,...sn)**   | 返回 sub 在列表中首次出现的位置，找不到返回 0           | SELECT FIELD('MySQL','Java','MySQL','Python');   | 2            |
| **FIND_IN_SET(sub, str_set)** | 返回 sub 在逗号分隔的字符串集合中的位置               | SELECT FIND_IN_SET('MySQL','Java,MySQL,Python'); | 2            |
| **FORMAT(num, d)**            | 格式化数字为千分位格式，保留 d 位小数                 | SELECT FORMAT(1234567.89,2);                     | 1,234,567.89 |
| **SPACE(n)**                  | 生成 n 个空格的字符串                         | SELECT CONCAT('Hello',SPACE(2),'MySQL');         | Hello  MySQL |
| **ASCII(s)**                  | 返回字符串第一个字符的 ASCII 码                  | SELECT ASCII('A');                               | 65           |
| **CHAR(n)**                   | 将 ASCII 码转换为对应的字符                    | SELECT CHAR(65);                                 | A            |
| **REPEAT(s, n)**              | 将字符串 s 重复 n 次                        | SELECT REPEAT('*',3);                            | ***          |

正则专用字符串函数
| **函数**                             | **核心功能**           | **示例 SQL**                                                                | **执行结果**    |
|------------------------------------|--------------------|---------------------------------------------------------------------------|-------------|
| **REGEXP_LIKE(s, 正则表达式)**          | 判断字符串是否匹配正则，返回 1/0 | SELECT REGEXP_LIKE('13812345678','^1[3-9]\\d{9}$');                       | 1（验证手机号合规）  |
| **REGEXP_SUBSTR(s, 正则表达式)**        | 提取字符串中匹配正则的子串      | SELECT REGEXP_SUBSTR('订单号123456','\\d+');                                 | 123456      |
| **REGEXP_REPLACE(s, 正则, new_str)** | 正则替换匹配的内容          | SELECT REGEXP_REPLACE('13812345678','(\\d{3})\\d{4}(\\d{4})','$1****$2'); | 138****5678 |
| **REGEXP_INSTR(s, 正则)**            | 返回正则匹配内容的首次出现位置    | SELECT REGEXP_INSTR('MySQL8.0','\\d');                                    | 6           |

关键避坑指南（新手必看）
1. 索引起始规则：MySQL 所有字符串函数的位置索引`从 1 开始`，不是编程常用的 0，start=0 会直接返回空字符串，是最常见的新手错误。
2. NULL 值陷阱：CONCAT()只要有一个参数是 NULL，整体返回 NULL，拼接字段时必须用IFNULL(字段, '')预处理；CONCAT_WS()会自动跳过 NULL 值，是多字段拼接的首选。
3. 长度函数区分：统计文本字数用CHAR_LENGTH()，校验存储字节用LENGTH()，中文场景混用会导致结果完全错误。
4. 填充截断问题：LPAD()/RPAD()的核心是「最终长度固定为 target_len」，如果原串长度超过目标长度，会直接截断，而非保留原串。
5. GROUP_CONCAT 长度限制：默认最大拼接长度为 1024 字节，超长会静默截断，生产环境需提前修改配置：SET GLOBAL group_concat_max_len = 1024000;
6. 大小写敏感规则：默认使用_ci结尾的排序规则（不区分大小写），如需区分大小写，可使用_bin二进制排序规则，或加BINARY关键字，如SELECT REPLACE(BINARY 'Hello','h','H');


关键注意点：
- **MySQL**：`LENGTH()` 返回字节数（UTF-8 下中文占 3 字节），`CHAR_LENGTH()` 返回字符数。
- **SQL Server**：`SUBSTRING(start, len)` 中 start 从 `1` 开始，无 `LPAD/RPAD`，需用 `REPLICATE(pad, len) + s` 实现。
- **Oracle**：`SUBSTR(start, len)` 支持负数 `start（从尾部倒数）`，`LENGTHB()` 返回字节数。

### 数学函数

MySQL 数学函数覆盖基础运算、取整、随机数、幂指对、三角函数等场景，以下是核心函数的结构化整理：

基础运算函数：
| **函数名**                   | **功能说明**                | **示例 SQL**                | **示例结果** |
|---------------------------|-------------------------|---------------------------|----------|
| **ABS(n)**                | 返回绝对值                   | SELECT ABS(-123);         | 123      |
| **SIGN(n)**               | 返回符号：-1 (负)/0 (零)/1 (正) | SELECT SIGN(-45);         | -1       |
| **MOD(n, m)**             | 取模（求余），同 n % m          | SELECT MOD(10, 3);        | 1        |
| **GREATEST(v1, v2, ...)** | 返回多个值中的最大值              | SELECT GREATEST(5, 9, 3); | 9        |
| **LEAST(v1, v2, ...)**    | 返回多个值中的最小值              | SELECT LEAST(5, 9, 3);    | 3        |

取整与截断函数：
| **函数名**                  | **功能说明**              | **示例 SQL**                   | **示例结果** |
|--------------------------|-----------------------|------------------------------|----------|
| **CEIL(n) / CEILING(n)** | 向上取整（不小于 n 的最小整数）     | SELECT CEIL(4.1);            | 5        |
| **FLOOR(n)**             | 向下取整（不大于 n 的最大整数）     | SELECT FLOOR(4.9);           | 4        |
| **ROUND(n, d)**          | 四舍五入，d 为保留小数位数（d 可为负） | SELECT ROUND(123.456, 2);    | 123.46   |
| **TRUNCATE(n, d)**       | 截断小数，直接舍去指定位数（不四舍五入）  | SELECT TRUNCATE(123.456, 2); | 123.45   |

幂指对与三角函数：
| **函数名**                         | **功能说明**        | **示例 SQL**            | **示例结果**   |
|---------------------------------|-----------------|-----------------------|------------|
| **POWER(n, exp)**               | 幂运算（n 的 exp 次方） | SELECT POWER(2, 3);   | 8          |
| **SQRT(n)**                     | 平方根             | SELECT SQRT(16);      | 4          |
| **EXP(n)**                      | e 的 n 次方（自然指数）  | SELECT EXP(1);        | 2.718...   |
| **LOG(n) / LN(n)**              | 自然对数（以 e 为底）    | SELECT LOG(2.718);    | ~1         |
| **LOG10(n)**                    | 常用对数（以 10 为底）   | SELECT LOG10(100);    | 2          |
| **LOG2(n)**                     | 二进制对数（以 2 为底）   | SELECT LOG2(8);       | 3          |
| **SIN(n) / COS(n) / TAN(n)**    | 三角函数（n 为弧度）     | SELECT SIN(PI()/2);   | 1          |
| **ASIN(n) / ACOS(n) / ATAN(n)** | 反三角函数（返回弧度）     | SELECT ASIN(1);       | PI()/2     |
| **RADIANS(deg)**                | 角度转弧度           | SELECT RADIANS(180);  | PI()       |
| **DEGREES(rad)**                | 弧度转角度           | SELECT DEGREES(PI()); | 180        |
| **PI()**                        | 返回圆周率 π         | SELECT PI();          | 3.14159... |

随机数函数：
| **函数名**                           | **功能说明**               | **示例 SQL**                 | **示例结果** |
|-----------------------------------|------------------------|----------------------------|----------|
| **RAND()**                        | 生成 0 到 1 之间的随机浮点数      | SELECT RAND();             | ~0.567   |
| **RAND(seed)**                    | 生成固定序列的随机数（seed 为整数种子） | SELECT RAND(123);          | 固定值      |
| **FLOOR(RAND()*(max-min+1))+min** | 生成 [min, max] 之间的随机整数  | SELECT FLOOR(RAND()*10+1); | 1-10 随机数 |


关键注意点
- **ROUND 的负数精度**：`ROUND(1234, -2)` 会将十位及以下四舍五入，结果为 1200。
- **TRUNCATE 与 ROUND 的区别**：`TRUNCATE(4.9, 0)` 结果为 4（直接截），`ROUND(4.9, 0)` 结果为 5（四舍五入）。
- **RAND 的种子用法**：若需重复测试，可给 `RAND()` 加固定种子（如 `RAND(123)`），每次执行会生成相同的随机序列。
- **NULL 值处理**：`GREATEST()/LEAST()` 若参数中包含NULL，结果直接返回NULL，需注意判空。
- **三角函数的单位**：MySQL 三角函数默认使用弧度，若输入角度需先用 RADIANS() 转换。

## 时间日期函数

### 获取当前日期和时间

- **NOW() / CURRENT_TIMESTAMP()**：返回当前日期和时间（格式：`YYYY-MM-DD HH:MM:SS`）。
- **CURDATE() / CURRENT_DATE()**：返回当前日期（格式：`YYYY-MM-DD`）。
- **CURTIME() / CURRENT_TIME()**：返回当前时间（格式：`HH:MM:SS`）。

NOW() 记录过后就不会在改变，而 CURRENT_TIMESTAMP() 会发生改变。

### 日期时间格式化

- **DATE_FORMAT(date, format)**：将日期按指定格式显示，常用格式符：
    - `%Y`：4 位年份（如 2026），`%y`：2 位年份（如 26）
    - `%m`：月份（01 - 12），`%c`：月份（1 - 12）
    - `%d`：日期（01 - 31），`%e`：日期（1 - 31）
    - `%H`：小时（00 - 23），`%h`：小时（01 - 12）
    - `%i`：分钟（00 - 59），`%s`：秒（00 - 59）

```sql
--  2026年4月10日 15：49：31
SELECT DATE_FORMAT(CURRENT_TIMESTAMP(),'%Y年%c月%e日 %H：%i：%s');
```

### 日期时间计算

- **DATE_ADD(date, INTERVAL expr unit)**：日期加上指定时间间隔，`unit 可选：DAY、MONTH、YEAR、HOUR`等。
- **DATE_SUB(date, INTERVAL expr unit)**：日期减去指定时间间隔。
    ```sql
    -- 需要 INTERVAL
    SELECT DATE_ADD(NOW(), INTERVAL 7 DAY) FROM DUAL;
    ```
- **DATEDIFF(date1, date2)**：计算两个日期相差的`天数`（date1 - date2）。
- **TIMESTAMPDIFF(unit, datetime1, datetime2)**：计算两个日期`时间的差值`datetime2 - datetime1），`unit 可选：SECOND、MINUTE、HOUR、DAY、MONTH、YEAR`。

### 日期时间提取

- **YEAR(date) / MONTH(date) / DAY(date)**：提取年、月、日。
- **HOUR(time) / MINUTE(time) / SECOND(time)**：提取时、分、秒
- **DAYOFWEEK(date)**：返回星期几（1 = 周日，2 = 周一，…，7 = 周六）。
- **DAYOFMONTH(date)**：返回当月第几天（1-31）。
- **DAYOFYEAR(date)**：返回当年第几天（1-366）。

### 其他函数

- **STR_TO_DATE(str, format)**：将字符串按指定格式转换为日期。
    - 示例：`STR_TO_DATE('2026-04-10', '%Y-%m-%d')` → 日期类型
- **LAST_DAY(date)**：返回当月最后一天的日期。
- **UNIX_TIMESTAMP()**：返回当前时间的 Unix 时间戳（`1970-1-1 到如今的秒数`）。
- **FROM_UNIXTIME(timestamp)**：将 Unix 时间戳转换为日期时间格式。

## 加密和系统函数

- **user()**：返回当前用户及所在IP。
- **DATABASE()**：返回当前使用的数据库。
- **MD5(str)**：返回一个 MD5 32位的字符串，常用于（用户密码）加密
- **PASSWORD(str)**：MySQL默认加密函数

## 流程控制函数

这类函数可直接用于 `SELECT、WHERE、UPDATE、INSERT、ORDER BY 等普通 SQL 语句`，无需存储过程 / 函数，用于单行数据的条件判断、空值处理，是日常开发`最常用`的能力。

1. IF () 函数：二选一基础判断
- 语法：`IF(condition, value_if_true, value_if_false)`
- 核心逻辑：条件 `condition` 为 TRUE（非 0、非 NULL），返回第二个参数；否则返回第三个参数。

```sql
-- 基础二值判断
SELECT IF(amount > 1000, '大额订单', '普通订单') AS order_type FROM orders;

-- 嵌套IF实现多分支（超过3层建议用CASE WHEN，可读性更好）
SELECT IF(score >= 90, '优秀', IF(score >= 60, '及格', '不及格')) AS grade FROM student;
```

- 条件为 NULL 时，会被视为 FALSE，例如 IF(NULL, 1, 0) 返回 0
- 两个返回值的`数据类型需一致`，否则会触发`隐式转换`，可能导致索引失效

---

2. IFNULL () 函数：空值兜底专用
- 语法：`IFNULL(expr1, expr2)`
- 核心逻辑：如果 expr1 不为 NULL，返回原值；否则返回兜底值 expr2，是 MySQL 处理 NULL 值最高频的函数。

```sql
-- 字段空值兜底
SELECT IFNULL(phone, '未预留手机号') AS contact FROM user;

-- 计算场景兜底，避免 NULL 导致结果为 NULL
SELECT IFNULL(price * num, 0) AS total_amount FROM order_item;
```

- 仅判断 NULL，`空字符串''、数字 0、布尔值 false` 都会被视为「非 NULL」，不会触发兜底。
- 是 MySQL 专属函数，跨库兼容性不如 `COALESCE()`

---

3. NULLIF () 函数：等值判空，规避异常
- 语法：`NULLIF(expr1, expr2)`
- 核心逻辑：如果 `expr1 = expr2` 成立，返回 NULL；否则返回 expr1。最核心的用途是`规避除零错误`、避免无意义的数据更新

```sql
-- 经典除零防护，分母为0时返回NULL，不会报Division by 0错误
SELECT total / NULLIF(quantity, 0) AS unit_price FROM sales;

-- 规避重复更新：新旧值一致时设为NULL，避免无意义的行锁和日志写入
UPDATE user SET username = NULLIF(new_username, old_username) WHERE id = 1;
```

- 本质等价于 `CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END`
- 两个参数数据类型不同时，会触发隐式类型转换

---

4. COALESCE () 函数：多参数空值兜底
- 语法：`COALESCE(expr1, expr2, expr3, ..., exprN)`
- 核心逻辑：返回参数列表中第一个非 NULL 的值，支持无限个参数，是IFNULL()的超集，也是 SQL 标准函数，跨数据库兼容性最好。

```sql
-- 多字段优先级兜底，按手机号>邮箱>微信的优先级取联系方式
SELECT COALESCE(mobile, email, wechat_id, '暂无联系方式') AS contact FROM user;

-- 金额计算兜底，多字段都为NULL时最终返回0
SELECT COALESCE(settle_amount, pay_amount, order_amount, 0) AS final_amount FROM orders;
```

- 所有参数都为 NULL 时，最终返回 NULL
- 比嵌套 `IFNULL()` 更简洁，可读性更强，优先推荐使用

---

5. CASE 表达式：最强大的多分支控制
- 语法：
```sql
CASE 匹配表达式
  WHEN 匹配值1 / 条件1 THEN 结果1
  WHEN 匹配值2 / 条件2 THEN 结果2
  ...
  ELSE 默认结果
END
```

- ELSE 子句可选，不写时所有 WHEN 都不匹配会返回 NULL
- 所有 THEN 和 ELSE 的返回值类型需一致，否则会触发隐式转换
- 搜索 CASE 支持任意复杂条件（AND/OR、函数、子查询），优先推荐使用