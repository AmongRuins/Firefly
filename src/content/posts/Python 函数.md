---
title: Python 函数
published: 2026-04-20
updated: 2026-04-24
description: '该篇文章主要内容为Python函数'
image: ''
tags: [软件测试]
category: 'Python'
draft: false 
lang: ''
slug: python-funtion
---

## 函数

函数是使用 `def` 关键字定义、封装了独立功能的代码块，可在需要时重复调用。

```python
# 函数功能：计算商品最终总价
# 3个形参：price(单价)、quantity(购买数量)、discount(折扣率，0~1)
def calc_total(price, quantity, discount):
    return price * quantity * discount
```

核心优势：大幅减少重复代码、降低代码冗余，同时提升代码的可读性与开发效率。

1. 形参与实参

    - 形参：函数定义时括号内声明的参数，无具体数值，仅起到占位作用，用于接收调用时传入的数据。

    - 实参：函数调用时括号内传入的具体数据值，会传递给对应的形参，供函数内部使用。

2. return 语句：用于`终止函数的执行`，同时可将函数的执行结果返回给调用处。

    - 若 return 后未写任何数据，或函数体中完全不写 return，函数执行结束后默认返回 None；

    - 若 return 后携带具体数据，会将该数据作为函数的返回值，返回给函数调用的位置；

    - 函数一旦执行到 return 语句，会立即终止执行，后续代码不会运行。

3. 函数文档注释：在函数体的开头，使用`三引号`包裹注释内容，用于说明函数的功能、参数含义、返回值等信息。

参数定义顺序：`普通参数`，*args，`缺省参数`，**kwargs

## 位置参数

位置参数是最基础的传参方式，`严格按照函数定义时形参的「顺序、个数」一一匹配`，实参的位置，直接决定了它会被绑定到哪个形参上。

```python
# 标准位置传参：按顺序匹配形参
# 第1个实参100 → 第1个形参price
# 第2个实参5 → 第2个形参quantity
# 第3个实参0.8 → 第3个形参discount
total = calc_total(100, 5, 0.8)
print(total)  # 运行结果：400.0
```

位置传参必须严格匹配形参的个数，多传、少传都会直接触发 `TypeError`。

## 关键字参数

关键字传参通过 `形参名=实参值` 的格式传递参数，`无需遵循形参的定义顺序`，因为通过形参名直接锁定了绑定关系。

```python
# 示例1：标准关键字传参，和形参顺序一致
total1 = calc_total(price=100, quantity=5, discount=0.8)

# 示例2：完全打乱传参顺序，依然能精准匹配（关键字传参核心优势）
total2 = calc_total(discount=0.8, price=100, quantity=5)
total3 = calc_total(quantity=5, discount=0.8, price=100)

# 三个结果完全一致
print(total1 == total2 == total3)  # 运行结果：True
```

这里需要注意的是，`所有关键字传参，必须写在位置传参的后面`。

## 缺省参数

缺省参数也叫`默认参数`，指在函数定义时为形参设置默认值；调用函数时，若未给该形参传递实参，会自动使用预设的默认值，大幅简化重复场景的函数调用。

```python
# 错误写法：用可变对象列表[]作为缺省参数
def add_item(item, item_list=[]):
    item_list.append(item)
    return item_list

# 第一次调用：符合预期
print(add_item("苹果"))  # 输出：['苹果']
# 第二次调用：预期输出['香蕉']，实际出现累加
print(add_item("香蕉"))  # 输出：['苹果', '香蕉']
# 第三次调用：问题进一步放大
print(add_item("橙子"))  # 输出：['苹果', '香蕉', '橙子']
```

Python 函数的缺省参数值，`只在函数定义时计算 1 次`，而非每次调用时重新创建。正因此导致上诉 `item_list` 每次调用都会导致数据累加。

解决办法是用`不可变对象None`作为缺省值，在函数内部每次调用时新建可变对象。

```python
# 正确写法：缺省参数用None，函数内部创建可变对象
def add_item(item, item_list=None):
    # 每次调用时，若未传item_list，就新建一个空列表
    if item_list is None:
        item_list = []
    item_list.append(item)
    return item_list

# 多次调用，完全符合预期
print(add_item("苹果"))  # 输出：['苹果']
print(add_item("香蕉"))  # 输出：['香蕉']
print(add_item("橙子"))  # 输出：['橙子']

# 也支持自定义传入列表
my_list = ["西瓜"]
print(add_item("葡萄", my_list))  # 输出：['西瓜', '葡萄']
```

## 多值参数

多值参数也叫`可变参数`，专门用于处理`函数调用时实参个数不确定`的场景，是 Python 函数参数体系的核心进阶内容，核心分为两类：

- *args：可变位置参数，接收任意多个位置传参，自动打包成`元组` (tuple)

- **kwargs：可变关键字参数，接收任意多个关键字传参，自动打包成`字典` (dict)

### 可变位置参数

*args 会把所有传入的位置实参打包成一个元组，函数内部通过遍历元组使用参数，适配参数个数不固定的位置传参场景。

```python
# 定义函数：*args接收任意多个数字参数
def sum_numbers(*args):
    # args 是一个元组，包含所有传入的位置实参
    print(f"传入的参数打包结果：{args}，类型：{type(args)}")
    total = 0
    for num in args:
        total += num
    return total

# 调用：支持传0个、1个、任意多个参数
print("求和结果：", sum_numbers())  # 传0个参数
print("求和结果：", sum_numbers(10))  # 传1个参数
print("求和结果：", sum_numbers(1, 2, 3, 4, 5))  # 传多个参数
```

运行结果如下：
```plaintext
传入的参数打包结果：()，类型：<class 'tuple'>
求和结果： 0
传入的参数打包结果：(10,)，类型：<class 'tuple'>
求和结果： 10
传入的参数打包结果：(1, 2, 3, 4, 5)，类型：<class 'tuple'>
求和结果： 15
```

### 可变关键字参数

**kwargs 会把所有传入的`关键字实参`打包成一个字典（`key 为形参名，value 为实参值`），适配参数个数不固定的关键字传参场景。

```python
# 定义函数：**kwargs接收任意多个关键字参数
def print_user(**kwargs):
    # kwargs 是一个字典，包含所有传入的关键字实参
    print(f"传入的参数打包结果：{kwargs}，类型：{type(kwargs)}")
    for key, value in kwargs.items():
        print(f"{key}：{value}")

# 调用：支持传0个、任意多个关键字参数
print("--- 调用1 ---")
print_user(name="张三", age=20, city="北京", gender="男")

print("--- 调用2 ---")
print_user(goods_name="Python教程", price=99, stock=100, discount=0.8)
```

运行结果如下：
```plaintext
--- 调用1 ---
传入的参数打包结果：{'name': '张三', 'age': 20, 'city': '北京', 'gender': '男'}，类型：<class 'dict'>
name：张三
age：20
city：北京
gender：男
--- 调用2 ---
传入的参数打包结果：{'goods_name': 'Python教程', 'price': 99, 'stock': 100, 'discount': 0.8}，类型：<class 'dict'>
goods_name：Python教程
price：99
stock：100
discount：0.8
```

### 参数解包

实际开发中，我们常已有现成的`列表 / 元组 / 字典`，需要把里面的元素批量传给多值参数，此时就需要用解包操作：

- 对列表 / 元组：用 * 解包，拆成单个位置参数传给 *args
    ```python
    # 列表/元组解包（*）
    def sum_nums(*args):
        return sum(args)

    # 已有现成的列表/元组
    num_list = [1, 2, 3, 4, 5]
    num_tuple = (10, 20, 30)

    # 解包传参
    print(sum_nums(*num_list))  # 等价于 sum_nums(1,2,3,4,5) → 输出15
    print(sum_nums(*num_tuple))  # 等价于 sum_nums(10,20,30) → 输出60
    print(sum_nums(100, *num_list, 200))  # 支持混合传参 → 输出315
    ```

- 对字典：用 ** 解包，拆成关键字参数传给 **kwargs
    ```python
    # 字典解包（**）
    def user_info(name, age, city):
        print(f"姓名：{name}，年龄：{age}，城市：{city}")

    # 已有现成的字典（key必须和函数形参名完全一致）
    user_dict = {
        "name": "赵六",
        "age": 28,
        "city": "广州"
    }

    # 解包传参
    user_info(**user_dict)  # 等价于 user_info(name="赵六", age=28, city="广州")
    ```

## 匿名函数

匿名函数是 Python 中用 `lambda` 关键字定义的无函数名、仅包含单个表达式的轻量级函数，适合处理简单、一次性使用的逻辑（无需单独定义完整函数），核心语法为：

```python
lambda 参数1, 参数2, ... : 表达式
```

lambda 只能有一个表达式，表达式的结果就是返回值，不能包含复杂语句（如循环、多分支 if-else）。

1. 单个参数的 lambda
    ```python
    # 定义：接收一个参数x，返回x的平方
    square = lambda x: x ** 2

    # 调用：和普通函数一样
    print(square(5))  # 输出：25
    print(square(10)) # 输出：100
    ```

2. 多个参数的 lambda
    ```python
    # 定义：接收a、b两个参数，返回a+b
    add = lambda a, b: a + b

    # 调用
    print(add(3, 5))    # 输出：8
    print(add(10, 20))  # 输出：30
    ```

3. 直接调用（一次性使用）
    ```python
    # 定义后立即调用，无需赋值给变量
    result = (lambda x, y: x * y)(4, 6)
    print(result)  # 输出：24
    ```

可通过 `ord（字符）：获取字符对应 ASCII编码`，`chr（ASCII编码）：获取该ASCII编码对应的字符`。