---
title: Python 面向对象
published: 2026-04-26
description: '该篇文章主要内容为Python面向对象相关知识'
image: ''
tags: [软件测试]
category: 'Python'
draft: false 
lang: ''
slug: python-object-oriented
---

## 面向对象思想

面向对象（`Object-Oriented Programming, OOP`）：以 “对象” 为中心，将数据（属性）和操作数据的方法封装成 “`类`”，通过类的实例（对象）交互解决问题。

面向过程（`Procedure-Oriented Programming, POP`）：以 “过程” 为中心，将问题拆解为`一系列步骤（函数 / 方法）`，按顺序执行这些步骤解决问题。

## 类和对象

类（Class）：是创建对象的`模板`，定义了对象的`属性（数据）`和`方法（操作）`。比如 “人类” 是一个类，包含 “姓名、年龄” 等属性，“说话、走路” 等方法。

对象（Object）：是类的`实例`（具体存在的个体）。比如 “张三” 是 “人类” 的一个对象，拥有具体的姓名（“张三”）和年龄（25 岁）。

通过关键字 `class` 定义一个类，其中用 `__init__` 方法（构造函数）初始化属性，`self` 代表当前对象本身。

```python
class Person:
    # 构造函数：初始化对象属性
    def __init__(self, name, age):
        self.name = name  # 实例属性：姓名
        self.age = age    # 实例属性：年龄

    # 方法：对象的行为
    def say_hello(self):
        print(f"你好，我是{self.name}，今年{self.age}岁。")
```

通过 `类名()` 创建一个对象，此时 `__init__` 方法 被调用，初始化属性

```python
# 创建两个 Person 类的对象
person1 = Person("张三", 25)
person2 = Person("李四", 30)

# 访问属性
print(person1.name)  # 输出：张三
print(person2.age)   # 输出：30

# 调用方法
person1.say_hello()  # 输出：你好，我是张三，今年25岁。
person2.say_hello()  # 输出：你好，我是李四，今年30岁。
```

### 属性

通过 `self` 关联的属性是`实例属性`，每个对象独有的数据（如 name、age）。

不写 `self` 的属性是`类属性`，所有对象共享的数据，直接在类中定义。

```python
class Person:
    species = "人类"  # 类属性：所有 Person 对象共享

    def __init__(self, name):
        self.name = name  # 实例属性

# 访问类属性
print(Person.species)  # 输出：人类
print(person1.species) # 输出：人类（对象也可访问类属性）
```

在 Python 类中，属性根据访问权限分为：

- 公有属性：类内部、外部都能直接访问的属性。

    ```python
    class Person:
        # 公有类属性
        species = "人类"

        def __init__(self, name):
            # 公有实例属性
            self.name = name  

    # 外部访问公有属性
    p = Person("张三")
    print(p.name)          # 输出：张三（访问实例属性）
    print(Person.species)  # 输出：人类（访问类属性）

    # 外部修改公有属性
    p.name = "李四"
    print(p.name)          # 输出：李四
    ```

- 私有属性：仅类内部能直接访问，外部无法直接访问的属性（Python 通过`名字改写`实现 “`伪私有`”）。

    ```python
    class Person:
        def __init__(self, name, age):
            self.name = name      # 公有属性
            self.__age = age      # 私有实例属性，以双下划线 __ 开头，

        def get_age(self):
            # 类内部可直接访问私有属性
            return self.__age

    p = Person("张三", 25)

    # 1. 外部直接访问私有属性 → 报错
    # print(p.__age)  # AttributeError: 'Person' object has no attribute '__age'

    # 2. 通过类方法间接访问（推荐）
    print(p.get_age())  # 输出：25

    # 3. 通过“名字改写”强行访问（不推荐）
    print(p._Person__age)  # 输出：25
    ```

### 方法

1. 实例方法（`最常用`、类的默认方法类型）：无需任何装饰器，类中定义的普通方法默认就是实例方法

    ```python
    class Student:
        # __init__ 是最典型的实例方法，用于初始化实例属性
        def __init__(self, name, score):
            # self.xxx 定义实例属性，每个实例独有
            self.name = name
            self.score = score

        # 自定义实例方法：读取实例与类属性
        def get_student_info(self):
            return f"【{self.school_name}】学生：{self.name}，分数：{self.score}"

    # 1. 必须先创建实例，才能调用实例方法
    stu1 = Student("张三", 85)
    # 2. 实例调用：Python自动把stu1绑定到self参数
    print(stu1.get_student_info())
    ```

2. 类方法（绑定到类，而非实例）：必须用`@classmethod`装饰器标记，第一个参数必须是`cls`（Python 约定俗成的命名），`cls`代表当前的类本身，而非实例

    ```python
    class Student:
        school_name = "XX中学"

        def __init__(self, name):
            self.name = name
            # 每创建一个实例，类属性计数+1
            Student.student_count += 1

        # 类方法：修改类属性
        @classmethod
        def update_school_name(cls, new_name):
            # cls 等价于 Student类本身，可直接操作类属性
            cls.school_name = new_name
            print(f"学校名称已更新为：{cls.school_name}")

    # 类直接调用类方法
    Student.update_school_name("YY第一中学")
    ```

3. 静态方法（类中的普通工具函数）：必须用`@staticmethod`装饰器标记，**没有任何强制的默认参数**
    ```python
    class Student:
        def __init__(self, name, score):
            self.name = name
            self.score = score

        # 静态方法：分数合法性校验工具，和类相关但不操作类/实例
        @staticmethod
        def is_score_valid(score):
            # 既不用self，也不用cls，仅处理传入的参数
            return 0 <= score <= 100

    # 1. 类直接调用静态方法（推荐）
    print(Student.is_score_valid(85))  # True

    # 2. 实例也能调用静态方法
    stu = Student("李四", 90)
    print(stu.is_score_valid(95))  # True
    ```

## 面向对象三大特性

### 封装特性

封装（Encapsulation），`隐藏对象的内部属性和实现细节`，仅对外暴露必要的接口，保证数据安全和逻辑可控。

将属性设为私有，通过公共方法（如 `get_xxx/set_xxx`）访问 / 修改属性。

```python
class Person:
    def __init__(self, name, age):
        self.name = name          # 公有属性
        self.__age = age          # 私有属性（隐藏内部数据）

    # 公共方法：间接访问私有属性
    def get_age(self):
        return self.__age

    # 公共方法：间接修改私有属性（可加逻辑控制）
    def set_age(self, new_age):
        if 0 < new_age < 120:
            self.__age = new_age
        else:
            print("年龄不合法！")

p = Person("张三", 25)
print(p.get_age())  # 输出：25（通过方法访问）
p.set_age(30)       # 通过方法修改（带合法性检查）
print(p.get_age())  # 输出：30
```

### 继承特性

继承（Inheritance），子类（派生类）继承父类（基类）的属性和方法，实现`代码复用`，同时可扩展或重写父类功能。

通过 `子类名(父类名)` 来定义继承关系，再用 `super()` 调用父类方法。

```python
# 父类（基类）
class Animal:
    def __init__(self, name):
        self.name = name  # 父类属性

    def eat(self):
        print(f"{self.name} 正在吃东西")  # 父类方法

# 子类（派生类）：继承 Animal
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # 调用父类构造函数
        self.breed = breed      # 子类新增属性

    def bark(self):
        print(f"{self.name}（{self.breed}）正在汪汪叫")  # 子类新增方法

# 子类：继承 Animal，并重写父类方法
class Cat(Animal):
    def eat(self):
        print(f"{self.name} 正在吃鱼")  # 重写父类方法

# 使用继承
dog = Dog("旺财", "柴犬")
dog.eat()   # 输出：旺财 正在吃东西（继承父类方法）
dog.bark()  # 输出：旺财（柴犬）正在汪汪叫（子类新增方法）

cat = Cat("咪咪")
cat.eat()   # 输出：咪咪 正在吃鱼（重写后的方法）
```

### 多态特性

多态（Polymorphism），同一个接口（方法名），不同的实现方式，提高代码灵活性。

Python 的多态依赖 **“鸭子类型”**：只要`对象有相同的方法名`，就可被统一调用，无需严格继承。

```python
# 定义统一接口：接收任意对象，调用 eat() 方法
def feed_animal(animal):
    animal.eat()

# 不同类，都有 eat() 方法
class Dog:
    def eat(self):
        print("狗吃骨头")

class Cat:
    def eat(self):
        print("猫吃鱼")

class Robot:
    def eat(self):
        print("机器人充电")

# 多态调用：同一个函数，传入不同对象，行为不同
feed_animal(Dog())   # 输出：狗吃骨头
feed_animal(Cat())   # 输出：猫吃鱼
feed_animal(Robot()) # 输出：机器人充电（鸭子类型，无需继承）
```