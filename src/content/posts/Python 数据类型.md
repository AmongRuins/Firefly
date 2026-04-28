---
title: Python 数据类型
published: 2026-04-20
updated: 2026-04-24
description: '该篇文章主要内容为Python的数据类型'
image: ''
tags: [软件测试]
category: 'Python'
draft: false 
lang: ''
slug: python-basic-data
---

Python 内置了丰富的数据类型，核心可分为 **基础标量类型（单值、不可变）和复合容器类型（多值、分可变 / 不可变）** 两大类，所有数据类型本质都是 Python 的类，变量都是对应类的实例。

## 基础标量类型（不可变类型）

这类类型存储单个值，`一旦创建就无法原地修改`，修改操作会生成新的对象。

### 数值类型（Number）

数值类型（Number）：用于`存储数字`，细分为 4 个子类型，是所有数值计算的基础。

| **子类型**         | **核心说明**         | **关键特性**                                                |
|-----------------|------------------|---------------------------------------------------------|
| **int 整型**      | 整数，无大小 / 精度限制    | 无溢出问题，支持二进制 / 八进制 / 十六进制写法                              |
| **float 浮点型**   | 带小数点的数字，双精度 64 位 | 存在二进制精度误差（如`0.1 + 0.2 ≠ 0.3`），不适合高精度金融计算                      |
| **bool 布尔型**    | 逻辑值，仅 2 个合法取值    | 是 int 的子类，True等价于 1，False等价于 0；空值 / 0 / 空容器均会被判定为 False |
| **complex 复数型** | 复数，含实部 + 虚部      | 虚部必须带后缀 j（`a = 3 + 4j`），多用于科学计算场景                                      |

```python
print(True + 1)  # 输出 2，bool是int的子类
print(0.1 + 0.2)  # 输出 0.3000000000000004，浮点精度特性
```

### 字符串类型（str）

用于表示文本，是`不可变的字符序列`，用单引号 / 双引号 / 三引号包裹。

```python
s1 = '单引号字符串'
s2 = "双引号字符串"
s3 = '''三引号支持
多行字符串'''
```

核心特性：

1. 支持正向（从左到右，以 `0、1、2...` 依次编号）、负向索引（从右到左，以 `-1、-2、-3...` 依次编号）访问单个字符。
    ```python
    s1 = '单引号字符串'
    print(s1[1]) # 输出 引
    print(s1[-2]) # 输出 符
    ```

2. 切片：通过`索引范围 + 步长`快速截取子串，返回新的子串对象，不会修改原字符串，其语法为 `字符串[start:end:step]`
    - 步长省略默认取 1，从左到右截取完整字符串：
    ```python
    s = "时崎狂三Kurumi"
    print(s[:])  # 输出：时崎狂三Kurumi（返回原字符串的完整副本）
    ```

    - 语法：`s[start:]`，从指定位置截取到末尾
    ```python
    print(s[3:])   # 输出：三Kurumi（从正索引3开始，截取到末尾）
    print(s[-6:])  # 输出：Kurumi（从负索引-6开始，截取到末尾，取最后6个字符）
    ```

    - 语法：`s[:end]`，从开头截取到指定位置前
    ```python
    print(s[:4])   # 输出：时崎狂三（左闭右开，截取0-3索引，不包含4）
    print(s[:-6])  # 输出：时崎狂三（截取到倒数第6位前，去掉最后6个字符）
    ```

    - 语法：`s[start:end]`，截取指定区间的子串
    ```python
    print(s[1:3])   # 输出：崎狂（取索引1、2，不包含3）
    print(s[-8:-4]) # 输出：狂三（取负索引-8到-4前，即-8、-7、-6、-5）
    ```

    - 正数步长：步长 > 1 时，会按步长间隔跳过字符截取
    ```python
    s = "1234567890"
    print(s[::2])  # 输出：13579（从开头到结尾，每隔1个字符取1个）
    print(s[1:8:2])# 输出：2468（从索引1到7，步长2）
    ```

    - 负数步长：步长 < 0 时，会从右往左反向截取，最经典的用法是字符串反转
    ```python
    s = "时崎狂三Kurumi"
    # 1. 完整反转字符串（高频必记）
    print(s[::-1])  # 输出：imuruK三狂崎时

    # 2. 反向截取指定区间
    # 注意：反向截取时，start索引必须在end索引的右侧（从右往左看），否则返回空串
    print(s[7:2:-1])  # 输出：uruK三狂（从索引7往左，截取到索引2前，不包含2）
    print(s[-2:-7:-1])# 输出：muruk（从倒数第2位往左，截取到倒数第7位前）
    ```

3. 常用方法
    - `字符串.find(sub, start=0, end=len(字符串))`：从左到右查找子串首次出现的正索引，若未找到则返回 -1
    ```python
    s = "时崎狂三 Kurumi Kurumi"
    # 从索引 6 开始查（跳过第一个 "Kurumi"）
    print(s.find("Kurumi", 6))  # 输出：12
    # 仅在索引 0 到 10 之间查（不包含 10）
    print(s.find("Kurumi", 0, 10))  # 输出：5
    # 负索引示例：从倒数第 10 位查到倒数第 5 位
    print(s.find("Kurumi", -10, -5))  # 输出：12
    ```

    - `字符串.replace(old, new, count=-1)`：用于将字符串中的指定子串替换为新子串，返回新字符串
    ```python
    s = "时崎狂三 狂三 狂三"
    # 仅替换前2次
    new_s = s.replace("狂三", "Kurumi", 2)
    print(new_s)  # 输出：时崎Kurumi Kurumi 狂三
    ```

    - `字符串.split(sep=None, maxsplit=-1)`：用于将字符串中的指定子串替换为新子串，返回新字符串
    ```python
    s = "时崎 狂三  \n  Kurumi  \t  Tokisaki"
    # 按任意空白拆分，自动忽略首尾和连续空白
    result = s.split()
    print(result)  # 输出：['时崎', '狂三', 'Kurumi', 'Tokisaki']

    # 按" - "拆分
    s = "时崎 - 狂三 - Kurumi"
    print(s.split(" - "))  # 输出：['时崎', '狂三', 'Kurumi']

    s = "时崎 狂三 Kurumi Tokisaki"
    # 仅拆前1次，列表长度为2
    print(s.split(maxsplit=1))  # 输出：['时崎', '狂三 Kurumi Tokisaki']
    # 仅拆前2次
    print(s.split(maxsplit=2))  # 输出：['时崎', '狂三', 'Kurumi Tokisaki']
    ```

    - `连接符字符串.join(可迭代对象)`：用当前字符串作为连接符，将一个 **可迭代对象（如列表、元组、字符串）** 中的所有元素连接成一个新字符串，原字符串和可迭代对象均不会被修改
    ```python
    # 用空格连接列表
    names = ["时崎", "狂三", "Kurumi"]
    result = " ".join(names)
    print(result)  # 输出：时崎 狂三 Kurumi

    # 用" - "连接元组
    chars = ("时崎", "狂三", "Tokisaki")
    result = " - ".join(chars)
    print(result)  # 输出：时崎 - 狂三 - Tokisaki
    ```

### 空类型（NoneType）

该类型`仅有一个合法取值None`，专门用于表示 “`空值、不存在、无返回`”，常用于函数默认返回值、变量初始化占位。

需要注意的是，空类型是一个完全独立的类型，不能表示 `0、空字符串、False`。

## 复合容器类型

用于存储多个数据元素，按是否可原地修改，分为`不可变容器`和`可变容器`。

- **不可变容器**：创建后无法原地增删改元素，修改操作会生成新对象。

- **可变容器**：创建后可原地增删改元素，内存地址不变，是 Python 最常用的容器类型。

### 元组

元组（tuple）属于不可变容器，`有序、不可变`的序列，用小括号`()`包裹，元素用逗号分隔。

```python
t1 = (1, 2, 3, 'hello')  # 多元素元组
t2 = (5,)  # 单元素元组必须加逗号，否则会被识别为普通数值
t3 = ()  # 空元组
```

由于不可变的特性，`只支持查询操作`，即索引、切片这些，用法和字符串一致。

元素可存放任意数据类型，支持嵌套，其不可变特性也可作为字典的 key 来使用。

内存占用更小，遍历效率高于列表。

### 列表

列表（list）属于可变容器，`有序、可变`的序列，用中括号`[]`包裹，是 Python 最通用的容器，可存储`任意类型`的数据。

```python
list1 = [1, 2.5, 'python', True, [4,5]]  # 支持任意类型元素，可嵌套
list1[0] = 100 # 可变，可原地修改元素
list2 = []  # 空列表
```

列表推导式是 Python 中高效创建列表的语法糖，核心结构为：`变量名 = [表达式 for 变量 in 可迭代对象]`。执行逻辑：每循环一次，将「表达式」的计算结果追加到新列表中。

```python
my_list = ["hello" for i in range(5)] # 遍历 5次，输出有5个 "hello" 的列表
```

#### 常用方法

1. `list.index(element, start=0, end=len(list))`：`查找`列表中指定元素`第一次`出现的索引位置。

    ```python
    fruits = ["apple", "banana", "cherry", "banana"]

    # 基本查找
    print(fruits.index("banana"))  # 输出: 1（第一次出现的位置）

    # 指定起始范围查找
    print(fruits.index("banana", 2))  # 输出: 3（从索引 2 开始找）

    # 查找不存在的元素（会报错）
    # print(fruits.index("orange"))  # 抛出 ValueError

    # 若不确定元素是否存在，可先用 in 检查，防止出现异常
    if "orange" in fruits:
        print(fruits.index("orange"))
    else:
        print("元素不存在")
    ```

2. `list.count(element)`：统计列表中指定元素出现的总次数。

    ```python
    fruits = ["apple", "banana", "cherry", "banana", "banana"]
    numbers = [1, 2, 3, 2, 1.0, 1, 4]

    # 统计存在的元素
    print(fruits.count("banana"))  # 输出: 3
    print(numbers.count(1))        # 输出: 3，元素相等类型不同，也会被视为同一元素（1 == 1.0 为 True）

    # 统计不存在的元素
    print(fruits.count("orange"))  # 输出: 0
    ```

3. `list.append(element)`：在列表的`末尾原地追加`单个元素，是列表最常用的新增元素方法，`直接修改原列表，不会创建新列表`。

    ```python
    # 追加单个基础类型元素
    nums = [1, 2, 3]
    nums.append(4)
    print(nums)  # 输出: [1, 2, 3, 4]

    # 追加不同类型的元素
    fruits = ["apple", "banana"]
    fruits.append("cherry")  # 字符串
    fruits.append([3,4])         # 数字
    fruits.append(True)      # 布尔值
    print(fruits)  # 输出: ['apple', 'banana', 'cherry', [3,4], True]
    ```

4. `list.insert(index, element)`：在列表`指定的索引位置`原地插入单个元素，直接修改原列表，不会创建新列表。

    ```python
    nums = [1, 2, 3]
    # 在索引1的位置插入元素4，原索引1及后方元素后移
    nums.insert(1, 4)
    print(nums)  # 输出: [1, 4, 2, 3]

    # 在倒数第1个元素的前方插入
    nums.insert(-1, "orange")
    print(nums)  # 输出: [1, 4, 2, "orange" , 3]

    # # 索引超过列表最大长度，自动插入到列表末尾（等价append）
    nums.insert(10, 4)
    print(nums)  # 输出: [1, 4, 2, "orange" , 3 , 4]

    # 索引小于列表最小负数范围，自动插入到列表开头
    nums.insert(-10, 0)
    print(nums)  # 输出: [0 , 1, 4, 2, "orange" , 3 , 4]
    ```

5. `列表.extend(可迭代对象)`：批量添加元素到原列表末尾，不会生成嵌套结构。

    ```python
    a = [1, 2]
    b = [3, 4]

    # extend：把元素逐个添加（展开）
    a.extend(b)
    print(a)  # [1, 2, 3, 4]
    ```

6. `list.pop(index=-1)`：移除并返回列表中指定索引位置的元素，默认移除列表最后一个元素。

    ```python
    nums = [1, 2, 3, 4]
    # 不填索引，默认移除最后一个
    removed = nums.pop()
    print("被移除的元素:", removed)  # 输出: 4
    print("修改后的列表:", nums)    # 输出: [1, 2, 3]

    # # 移除索引1的元素（2）
    removed = nums.pop(1)
    print("被移除的元素:", removed)  # 输出: 2
    print("修改后的列表:", nums)    # 输出: [1, 3]

    # 移除倒数第2个元素（1）
    removed = nums.pop(-2)
    print("被移除的元素:", removed)  # 输出: 1
    print("修改后的列表:", nums)    # 输出: [3]
    ```

7. `list.remove(element)`：按值删除列表中第一个匹配的元素，直接原地修改原列表。

    ```python
    fruits = ["apple", "banana", "cherry" , "banana"]
    fruits.remove("banana") # 只删除第一个匹配项
    print(fruits)  # 输出: ['apple', 'cherry' , "banana"]
    ```

8. `list.clear()`：原地清空列表中的所有元素，使列表变为空列表。

    ```python
    # 清空非空列表
    nums = [1, 2, 3, 4, 5]
    nums.clear()
    print(nums)  # 输出: []

    nums.clear()
    print(nums)  # 输出: 对空列表调用，无报错无影响
    ```

9. `list.reverse()`：原地反转列表中所有元素的排列顺序。

    ```python
    # 字符串列表反转
    fruits = ["apple", "banana", "cherry"]
    fruits.reverse()
    print(fruits)  # 输出: ['cherry', 'banana', 'apple']

    # 空列表调用无报错、无影响
    empty_list = []
    empty_list.reverse()
    print(empty_list)  # 输出: []

    nested_list = [1, 2, [3, 4, 5], 6]
    nested_list.reverse()
    print(nested_list)  # 输出: [6, [3, 4, 5], 2, 1]
    # 子列表 [3,4,5] 内部顺序没有变化，仅顶层元素被反转
    ```

10. `list.sort(key=None, reverse=False)`：原地对列表元素进行排序。默认按升序排列。

    ```python
    # 字符串列表升序（按Unicode码点）
    fruits = ["banana", "apple", "cherry", "date"]
    fruits.sort()
    print(fruits)  # 输出: ['apple', 'banana', 'cherry', 'date']

    # 数字列表降序
    nums = [3, 1, 4, 1, 5]
    nums.sort(reverse=True)
    print(nums)  # 输出: [5, 4, 3, 1, 1]

    # 自定义规则排序
    students = [("Alice", 25), ("Bob", 20), ("Charlie", 23)]
    students.sort(key=lambda x: x[1])  # 按元组的第2个元素（年龄）升序
    print(students)  # 输出: [('Bob', 20), ('Charlie', 23), ('Alice', 25)]
    ```

### 字典

字典是由`键值对（key-value）`格式的`可变`映射容器，Python3.7 + 保留插入顺序，用大括号`{}`包裹。

```python
dict1 = {"name": "张三", "age": 20, "hobby": ["python", "篮球"]}
dict1 = {"name" : "李四"}  # key 存在，修改对应的值
dict1 = {"birthday" : "05-20"} # key 不存在，在末尾添加元素
```

需要注意的是：key 必须是`不可变类型`（ int/str/tuple 等），且`必须唯一`；value 则可以是任意类型。

#### 常用方法

1. `del 字典[键]`：永久删除字典中指定键值对（**列表也存在该方法**）

    ```python
    student = {"name": "Alice", "age": 20}
    del student["age"]
    print(student)  # 输出: {'name': 'Alice'}
    ```

2. `字典.get(key, default=None)`：安全获取字典中指定键对应值，相比 `字典[键]` 的优势是不会抛出 `KeyError` 异常

    ```python
    # 定义测试字典
    fruit_price = {"apple": 10, "banana": 5, "grape": None}

    print(fruit_price.get("apple"))  # 输出：10

    print(fruit_price.get("orange"))  # 输出：None

    print(fruit_price.get("orange", "该水果暂无定价"))  # 输出：该水果暂无定价
    ```

3. 遍历字典

    - 直接遍历字典（默认遍历键）

    ```python
    # 定义测试字典
    user_info = {"name": "张三", "age": 25, "city": "东莞", "job": "工程师"}

    for key in user_info:
        print(key)
    # 输出：name age city job

    # 也可以显示调用 keys() 方法
    for key in user_info.keys():
        print(key)
    ```

    - 遍历字典的值（value）

    ```python
    # 定义测试字典
    user_info = {"name": "张三", "age": 25, "city": "东莞", "job": "工程师"}

    for value in user_info.values():
        print(value)
    # 输出：张三 25 东莞 工程师
    ```

    - 同时遍历键和值（key-value，常用）

    ```python
    # 定义测试字典
    user_info = {"name": "张三", "age": 25, "city": "东莞", "job": "工程师"}

    # 直接解包键和值
    for key, value in user_info.items():
        print(f"键：{key}，对应值：{value}")
    ```

## 补充：类型转换和判断

1. 类型判断

    - `type(x)`：返回 x 的精确类型，不考虑继承关系

    - `isinstance(x, 类型)`：判断 x 是否是该类型 / 该类型的子类，**日常开发推荐使用**

    ```python
    print(type(True) is int)  # False
    print(isinstance(True, int))  # True，bool是int的子类
    ```

2. 常用类型转换：当数据符合转换规则时实现类型互转

    ```python
    int("123")  # 字符串转整型 123，非数字字符串不能转换
    float("3.14")  # 字符串转浮点型 3.14
    str(100)  # 数字转字符串 "100"
    list((1,2,3))  # 元组转列表 [1,2,3]
    tuple([1,2,3])  # 列表转元组 (1,2,3)
    set([1,2,2,3])  # 列表转集合 {1,2,3}
    dict([("a",1), ("b",2)])  # 键值对列表转字典 {'a':1, 'b':2}
    ```