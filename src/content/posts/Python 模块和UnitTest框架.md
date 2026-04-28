---
title: Python 模块和UnitTest框架
published: 2026-04-27
description: '该篇文章主要介绍UnitTest框架'
image: ''
tags: [软件测试]
category: 'Python'
draft: false 
lang: ''
slug: python-module-unittest
---

## 模块和包

核心概念：

- **模块（Module）**：一个 `.py` 文件，包含函数、类、变量或可执行代码，用于组织复用代码。

- **包（Package）**：一个包含 `__init__.py` 文件的文件夹，用于`组织多个相关模块`，避免命名冲突。

两者虽然组织方式有所区别，但基本使用一致。

```python
import math  # 导入整个模块
import json  # 导入整个包

from math import pi  # 导入模块中的特定内容
from json import load  # 导入包中特定内容
```

需要注意的是，解释器会`优先查找项目`中是否有该 模块 / 包，如果没有才会去系统目录中查找，所以项目中最好不要自定义已有的包名。

被导入的模块或包会在主程序运行时自动执行， Python 解释器自动为每个模块（.py 文件）内置的全局字符串变量 `__name__`，核心作用是区分模块的运行方式：判断当前文件是作为主程序直接执行，还是被其他文件导入执行。

- 模块被直接执行（主程序运行）时，`__name__` 会被自动赋值为固定字符串：`__main__`。

- 模块被其他文件 `import` 导入时，它的 `__name__` 会被自动赋值为模块自身的名称，即 `.py` 文件的文件名，不带后缀）；如果是包内的模块，取值为 `包名.模块名`。

```python
# 模块核心功能代码（被导入时会正常执行）
def add(a, b):
    return a + b

# 打印查看__name__的实时值
print(f"当前模块的__name__值为：{__name__}")

# 仅当模块直接运行时，才会执行的代码块
if __name__ == '__main__':
    # 这里放模块的自测、调试、演示代码
    print("=== 正在执行模块自测代码 ===")
    print(f"1+2的计算结果：{add(1, 2)}")

```

## UnitTest框架

UnitTest 框架是 Python 内置的标准单元测试框架，基于 JUnit 设计，用于编写和运行可重复的测试代码。对于测试人员来说，主要是用于 `自动化脚本（用例代码）`执行框架，即用该框架管理运行多个测试用例。

核心组件：

- TestCase（测试用例）：测试用例测试代码。

- TestSuite（测试套件）：管理并组装多个 TestCase。

- TestRunner（测试运行器）：执行测试套件并输出结果。

- TestLoader（测试加载器）：对测试套件的功能补充。

- TestFixture（测试夹具）：测试前的准备（如创建数据）和测试后的清理（如关闭文件）工作。

### TestCase（测试用例）

UnitTest 单元测试框架的核心基类，所有自定义测试类必须继承 `unittest.TestCase`，方法名必须以 `test_` 开头才会被自动执行。

```python
"""导入框架包"""
import unittest

# 继承测试用例基类
class TestDemo(unittest.TestCase):
    def test_one(self):
        print('测试成功')

    def test_two(self):
        print('测试成功')

```

如果将鼠标光标放在`类名`后边，则会执行所有测试方法；如果将鼠标光标放在`对应方法名`后边，则会执行该方法。

### TestSuite（测试套件）

UnitTest 单元测试框架的测试套件，核心作用是组织、组装、管理多个测试用例（TestCase）。

```python
""""导入包"""
import unittest
from unittest框架 import TestDemo

# 实例化testsuite对象
suite = unittest.TestSuite()

# 添加测试用例的单个方法
suite.addTest(TestDemo('test_one'))

# 批量执行多个测试用例的方法
# suite.addTest(unittest.TestLoader().loadTestsFromTestCase(TestDemo))

```

控制台不会有什么输出，需要配合 TestRunner 才能执行测试方法。

### TestRunner（测试运行器）

UnitTest 单元测试框架的测试执行器，核心作用是`运行测试套件`（TestSuite）、收集测试结果、并以指定格式输出测试报告，是整个测试流程的最后一环。

```python
import unittest

runner = unittest.TextTestRunner(
    stream=sys.stdout,          # 输出流，默认是控制台
    verbosity=1,                # 输出详细程度：0(极简)、1(默认)、2(详细)
    descriptions=True,          # 是否显示测试用例描述
    failfast=False,             # 是否遇到第一个失败/错误就停止执行
    buffer=False                # 是否缓存用例的 print 输出，仅在失败时显示
)
```

执行测试套件：

```python
unittest.TextTestRunner(verbosity=2).run(suite)
```

### TestLoader（测试加载器）

UnitTest 框架内置的测试用例自动发现与加载核心引擎，是实现测试用例自动化批量管理的关键组件。

它的核心职责是：按照预设规则，自动扫描代码中的测试用例（TestCase），将符合规则的用例`批量加载`并组装成 TestSuite 测试套件，彻底解决手动逐个添加用例的繁琐问题，是大型自动化测试项目的必备工具。

```python
import unittest

# 加载测试类的测试方法
# suite = unittest.TestLoader().discover('./','unittest*.py')

# 与上面代码功能相同，都是加载测试类的测试方法
suite = unittest.defaultTestLoader.discover('./','unittest*.py')

# 执行测试
unittest.TextTestRunner(verbosity=2).run(suite)
```

### TestFixture（测试夹具）

TestFixture（测试夹具）不是一个具体的类或方法，而是 UnitTest 单元测试框架中的一个核心测试术语，指的是`测试用例执行前后`的准备工作和清理工作，目的是为测试用例提供一个稳定、可控的运行环境，保证测试的独立性和可重复性。

| **层级**  | **前置方法**      | **后置方法**         | **装饰器要求**        | **执行时机**                    |
|---------|---------------|------------------|------------------|-----------------------------|
| **模块级** | setUpModule() | tearDownModule() | 无，写在测试类外         | 整个.py 文件（模块）执行前 / 后，仅执行 1 次 |
| **类级**  | setUpClass()  | tearDownClass()  | 必须加 @classmethod | 整个测试类执行前 / 后，仅执行 1 次        |
| **方法级** | setUp()       | tearDown()       | 无                | 每个 test_ 测试方法执行前 / 后，都会执行 1 次 |

```python
"""导入框架包"""
import unittest

# 继承测试用例基类
class TestDemo(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        print('打开浏览器')

    @classmethod
    def tearDownClass(cls):
        print('关闭浏览器')

    def setUp(self):
        print('测试前...')

    def tearDown(self):
        print('测试结束...')

    def test_one(self):
        print('测试成功')

    def test_two(self):
        print('测试成功')

```

### 断言

TestCase 类内置的核心方法，是替代手工判定测试用例「通过 / 失败」的唯一标准。

| **断言方法**                                 | **核心作用**                      |
|------------------------------------------|-------------------------------|
| **assertEqual(a, b, msg=None)**    | 断言预取结果是否和实际结果一致 |
| **assertIn(a, b)** | 断言预期结果是否包含在实际结果中        |


```python
def add(a, b):
    if( a == b ):
        return '测试通过'
    else:
        return '测试失败'


class TestDivide(unittest.TestCase):
    def test_add(self):
        # 断言判断预期结果和实际结果是否一致
        self.assertEqual('测试失败',add(2,3))
    
    def test_sub(self):
        # 断言判断预期结果是否包含在实际结果中
        self.assertIn('通过',add(3,3))
```

### 跳过

UnitTest 提供了`测试用例跳过（Skip）` 机制，用于在特定条件下（如：环境不满足、依赖缺失、功能未完成），不执行某些测试用例，直接标记为「跳过」，避免因无关因素导致的测试误报，保证测试结果的有效性。

| **装饰器**                                 | **核心作用**                       | **必选参数**                   |
|-----------------------------------------|--------------------------------|----------------------------|
| **@unittest.skip(reason)**              | 无条件跳过：直接跳过被装饰的用例 / 类           | reason：跳过原因（必填，用于测试报告说明）   |
| **@unittest.skipIf(condition, reason)** | 条件为真时跳过：当 condition 为 True 时跳过 | condition：判断条件；reason：跳过原因 |

```python
def add(a, b):
    if( a == b ):
        return '测试通过'
    else:
        return '测试失败'

# 版本号
version = 20

class TestDivide(unittest.TestCase):
    # 直接跳过
    @unittest.skip('直接跳过')
    def test_add(self):
        # 断言判断预期结果和实际结果是否一致
        self.assertEqual('测试失败',add(2,3))

    # 根据条件看是否跳过
    @unittest.skipIf(version <= 30,'版本号不符合')
    def test_sub(self):
        # 断言判断预期结果是否包含在实际结果中
        self.assertIn('通过',add(3,3))
```

### 参数化

UnitTest 原生不支持参数化测试，但可以通过第三方库实现。参数化的核心作用是：用一套测试逻辑，`运行多组测试数据`，避免重复编写测试方法，极大提升测试效率和代码可维护性。

最常用的两个库是 `ddt` 和 `parameterized`，二选一即可，无需同时安装。

| **库名**            | **安装命令**                  | **核心特点**               |
|-------------------|---------------------------|------------------------|
| **ddt**           | `pip install ddt`           | 需装饰测试类，语法清晰，社区资料多      |
| **parameterized** | `pip install parameterized` | 无需装饰测试类，语法更简洁，支持更多数据格式 |

#### ddt 库

| **装饰器**          | **作用**            | **用法**                  |
|------------------|-------------------|-------------------------|
| @ddt         | 装饰测试类，启用 ddt 参数化  | 加在测试类定义上方               |
| @data(*测试数据) | 传入多组测试数据          | 加在测试方法上方，数据用 * 解包       |
| @unpack      | 解包复杂数据（如元组、列表、字典） | 当测试数据是嵌套结构时，配合 @data 使用 |

1. 简单数据（单参数）：

    ```python
    import unittest
    from ddt import ddt, data, unpack

    # 待测试函数
    def add(a, b):
        return a + b

    # 1. 测试类必须加 @ddt 装饰器
    @ddt
    class TestAddWithDDT(unittest.TestCase):
        # 2. 用 @data 传入多组测试数据（单参数场景）
        @data(1, 2, 3, 4, 5)
        def test_add_positive(self, num):
            """测试正数加法：num + 1"""
            self.assertEqual(add(num, 1), num + 1)

    ```

2. 复杂数据（多参数，元组 / 列表）

    ```python
    import unittest
    from ddt import ddt, data, unpack

    def add(a, b):
        return a + b

    @ddt
    class TestAddWithDDT(unittest.TestCase):
        # 测试数据：[(入参1, 入参2, 预期结果), ...]
        test_cases = [
            (1, 2, 3),
            (10, 20, 30),
            (-1, -1, -2),
            (0, 0, 0)
        ]

        # 1. @data 传入测试数据列表
        # 2. @unpack 解包元组，将3个值分别传给 a, b, expect
        @data(*test_cases)
        @unpack
        def test_add_multi_params(self, a, b, expect):
            """测试多组加法数据"""
            self.assertEqual(add(a, b), expect)

    ```

3. 字典数据（更清晰的参数命名）

    ```python
    import unittest
    from ddt import ddt, data, unpack

    def add(a, b):
        return a + b

    @ddt
    class TestAddWithDDT(unittest.TestCase):
        # 测试数据：字典格式，key对应测试方法的参数名
        test_cases = [
            {"a": 1, "b": 2, "expect": 3},
            {"a": 10, "b": 20, "expect": 30},
            {"a": -1, "b": -1, "expect": -2}
        ]

        @data(*test_cases)
        @unpack  # 解包字典，key必须和测试方法的参数名完全一致
        def test_add_with_dict(self, a, b, expect):
            """用字典数据测试加法"""
            self.assertEqual(add(a, b), expect)

    ```

#### parameterized 库

通过装饰器 `@parameterized.expand(测试数据)` 直接装饰测试方法，传入测试数据。

```python
import unittest
from parameterized import parameterized

def add(a, b):
    return a + b

class TestAddWithParameterized(unittest.TestCase):
    # 测试数据：[(用例描述, 入参1, 入参2, 预期结果), ...]
    # 第一个元素是用例描述，会显示在测试报告中，方便定位
    test_cases = [
        ("测试正数加法", 1, 2, 3),
        ("测试大数加法", 10, 20, 30),
        ("测试负数加法", -1, -1, -2),
        ("测试零加法", 0, 0, 0)
    ]

    # 直接用 @parameterized.expand 装饰测试方法，无需装饰类
    @parameterized.expand(test_cases)
    def test_add(self, case_name, a, b, expect):
        """参数化测试加法"""
        self.assertEqual(add(a, b), expect)

```

`parameterized.expand` 的第一个元素可以是用例描述，会直接显示在测试报告中，比 ddt 更易定位问题。