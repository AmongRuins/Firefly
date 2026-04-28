---
title: Python 文件操作和异常处理
published: 2026-04-26
description: '该篇文章主要内容为Python文件操作和异常处理'
image: ''
tags: [软件测试]
category: 'Python'
draft: false 
lang: ''
slug: python-file-exception-handle
---

## 文件操作

Python 文件操作主要围绕**打开、读写、关闭**文件展开，核心函数是 `open()`，推荐使用 `with` 语句自动管理文件生命周期。

必须关闭文件：使用 `with` 语句会自动调用 `f.close()`，避免手动关闭遗漏。

### 基本操作

1. 打开文件：`open(file, mode='r', encoding=None)`
    - `file`：文件路径（相对或绝对路径）
        - 相对路径：'`test.txt`'（当前目录）、'`./data/test.txt`'（子目录）
        - 绝对路径：'`C:/Users/xxx/test.txt`'（Windows）或 '`/home/xxx/test.txt`'（Linux/macOS）
    - `mode`：打开模式

    | **模式**        | **描述** | **注意**                |
    |---------------|--------|-----------------------|
    | **r**       | 只读（默认） | 文件不存在则报错              |
    | **w**       | 只写（覆盖） | 文件不存在则创建，存在则清空重写      |
    | **a**       | 追加写    | 文件不存在则创建，存在则在末尾追加     |
    | **r+**      | 读写     | 文件不存在则报错，写操作从当前指针位置开始 |
    | **rb / wb** | 二进制读写  | 用于图片、视频等非文本文件         |

    - `encoding`：编码格式（如 '`utf-8`'，处理中文时建议指定）

2. 读文件

    ```python
    # 方法1：读取全部内容
    with open('test.txt', 'r', encoding='utf-8') as f:
        content = f.read()  # 返回字符串
        print(content)

    # 方法2：按行读取（推荐大文件使用）
    with open('test.txt', 'r', encoding='utf-8') as f:
        for line in f:  # 逐行迭代，内存占用小
            print(line.strip())  # strip()去除换行符

    # 方法3：读取所有行到列表
    with open('test.txt', 'r', encoding='utf-8') as f:
        lines = f.readlines()  # 返回列表，每个元素是一行
    ```

3. 写文件

    ```python
    # 覆盖写（'w'模式），
    with open('test.txt', 'w', encoding='utf-8') as f:
        f.write('第一行内容\n')  # \n手动换行
        f.writelines(['第二行\n', '第三行\n'])  # 写入列表

    # 追加写（'a'模式）
    with open('test.txt', 'a', encoding='utf-8') as f:
        f.write('追加的内容\n')
    ```

### json文件

Python 操作 JSON 文件主要使用内置的 `json 模块`，核心是 JSON 数据与 Python 对象的相互转换。

| **JSON**       | **Python** |
|----------------|------------|
| **对象 {}**      | 字典 dict    |
| **数组 []**      | 列表 list    |
| **字符串 ""**     | 字符串 str    |
| **数字**         | int/float  |
| **true/false** | True/False |
| **null**       | None       |

1. 读取 JSON 文件（`json.load()`）：将文件中的 JSON 数据转换为 Python 对象

    ```python
    import json

    with open('data.json', 'r', encoding='utf-8') as f:
        data = json.load(f)  # 返回 dict/list
        print(data['name'])  # 直接按 Python 语法操作
    ```

2. 写入 JSON 文件（`json.dump()`）：将 Python 对象转换为 JSON 格式写入文件

    ```python
    import json

    data = {
        "name": "张三",
        "age": 25,
        "hobbies": ["读书", "游泳"],
        "is_student": False
    }

    # 推荐参数：indent格式化、ensure_ascii处理中文、sort_keys排序
    with open('output.json', 'w', encoding='utf-8') as f:
        json.dump(
            data, 
            f, 
            indent=4,          # 缩进4空格（美化输出）
            ensure_ascii=False,# 保留中文（否则会转成\uXXXX）
            sort_keys=True     # 按键名排序
        )
    ```

3. 字符串与 JSON 互转（`loads() / dumps()`）

    ```python
    import json

    # JSON字符串 → Python对象
    json_str = '{"name": "李四", "age": 30}'
    data = json.loads(json_str)
    print(data['age'])  # 30

    # Python对象 → JSON字符串
    data = {"city": "北京", "code": 100000}
    json_str = json.dumps(data, ensure_ascii=False, indent=2)
    print(json_str)
    ```

## 异常处理

Python 的异常处理机制，核心作用就是捕获、处理`运行时异常`，让程序不会直接崩溃，同时完成善后工作、记录错误、给出友好提示。主要有两种错误：

- 语法错误（SyntaxError）：代码不符合 Python 语法规范，解释器在解析阶段就会报错，代码完全不会执行。比如括号不闭合、缩进错误、关键字拼写错误。
    ```python
    # 语法错误：缺少右括号，解释器直接拒绝运行
    print("hello world"
    ```

- 运行时异常（Exception）：代码语法完全正确，但在程序运行过程中，因非法操作、不符合预期的输入、资源缺失等触发的错误。异常会中断程序正常执行流，若不处理，程序会直接崩溃退出。
    ```python
    # 语法正确，运行时触发 ZeroDivisionError 异常
    a = 10 / 0
    print("这句话不会执行，因为上面的异常中断了程序")
    ```

Python 异常处理的完整语法由 `try、except、else、finally` 四个关键字组成，每个关键字都有明确的分工。 

```python
f = None
try:
    f = open("test.txt", "r", encoding="utf-8")
    content = f.read()
    10 / 0  # 这里触发异常
except FileNotFoundError as e:
    print(f"文件不存在：{e}")
else:
    print(content)
finally:
    # 无论是否出错，都会执行这里，确保文件被关闭
    print("执行finally块，释放资源")
    if f is not None:
        f.close()
```

执行 `try` 块中的代码，若触发异常，立即终止 `try` 块后续代码，跳转到匹配的 `except` 块执行处理逻辑，程序不会崩溃。

当且仅当 `try` 块中没有触发任何异常时，才会执行 `else` 快代码。

无论 `try` 块是否触发异常，`finally` 块都会强制执行。