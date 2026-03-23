---
title: JavaScript和CSS相关描述
published: 2026-03-23
description: 'HTML用于页面的显示，那么JavaScript则在于页面动作的处理与校验，CSS则用于美化页面'
image: 'https://list.yppp.net/d/image/2026-03-22-0c979247acb2954e309bd77773f14e83.jpg'
tags: [JavaScript,CSS]
category: '软件测试'
draft: false 
lang: ''
---

## CSS的简单概述

CSS（Cascading Style Sheets，层叠样式表）是用来**控制网页外观和布局**的核心技术，和 HTML（负责网页结构）、JavaScript（负责网页交互）并称前端三大基石。

CSS 有三种常见的使用方式：
1. 行内样式（直接写在 HTML 标签里）
```html
<!-- 直接在标签的style属性中写样式，仅作用于当前标签 -->
<div style="width: 200px; height: 100px; background: #f0f0f0;">
  这是行内样式的盒子
</div>
```

缺点：**样式和结构耦合**，不利于维护，仅临时调试时使用。

2. 内部样式表（写在 HTML 的 style 标签里）
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>内部样式表</title>
  <!-- 内部样式表：样式写在head的style标签中，作用于当前页面 -->
  <style>
    /* 选择器：选中页面中所有class为box的元素 */
    .box {
      width: 200px;
      height: 100px;
      background: #42b983; /* 绿色背景 */
      color: white; /* 文字白色 */
      text-align: center; /* 文字居中 */
      line-height: 100px; /* 行高等于高度，实现文字垂直居中 */
    }
  </style>
</head>
<body>
  <div class="box">内部样式表的盒子</div>
</body>
</html>
```

**优点**：样式集中管理，作用于当前页面；**缺点**：无法复用到其他页面。

3. 外部样式表（单独的 `.css` 文件，**推荐**）
    - 创建 `style.css` 文件：
    ```css
    /* style.css - 外部样式表文件 */
    /* 选择器：选中class为box的元素 */
    .box {
    width: 200px;
    height: 100px;
    background: #3498db; /* 蓝色背景 */
    color: white;
    border-radius: 8px; /* 圆角 */
    padding: 10px; /* 内边距 */
    }
    ```

    - 在 HTML 中引入该文件：
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>外部样式表</title>
    <!-- 引入外部CSS文件，rel="stylesheet"表示这是样式表，href是文件路径 -->
    <link rel="stylesheet" href="style.css">
    </head>
    <body>
    <div class="box">外部样式表的盒子</div>
    </body>
    </html>
    ```

    优点：样式和结构完全分离，可复用到多个页面，维护成本低。

### CSS 核心概念
1. 常见选择器（选中要样式化的元素）

| 选择器类型  | 语法   | 示例      | 说明                      |
|--------|------|---------|-------------------------|
| 类选择器   | `.类名`  | `.box`    | 选中所有 `class=""box""` 的元素  |
| ID 选择器 | `#ID名` | `#header` | 选中唯一的 `id=""header""` 的元素 |
| 标签选择器  | `标签名`  | `div`     | 选中所有 `div` 元素             |
| 通配符    | `*`   | `*`       | 选中所有元素（慎用）              |

2. 常用样式属性（控制元素外观）
    - **尺寸**：`width`（宽度）、`height`（高度）
    - **背景**：`background`（背景色 / 图片）
    - **文字**：`color`（文字颜色）、`font-size`（字号）、`text-align`（文字对齐）
    - **边距**：`margin`（外边距，元素和其他元素的间距）、`padding`（内边距，元素内容和边框的间距）
    - **边框**：`border`（边框，如 `border: 1px solid #ccc;`）

3. 当多个样式作用于同一个元素时，优先级规则（从高到低）：**行内样式 > ID 选择器 > 类选择器 > 标签选择器 > 通配符**

示例，一个居中的浅灰色盒子，里面有红色居中的标题，和两行 16px 的文字：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>CSS入门示例</title>
  <style>
    /* 标签选择器：所有p标签 */
    p {
      font-size: 16px;
      line-height: 1.5;
    }
    /* 类选择器：class="title"的元素 */
    .title {
      color: #e74c3c; /* 红色 */
      font-size: 24px;
      text-align: center;
    }
    /* ID选择器：id="container"的元素 */
    #container {
      width: 800px;
      margin: 0 auto; /* 水平居中 */
      padding: 20px;
      background: #f9f9f9;
      border: 1px solid #eee;
    }
  </style>
</head>
<body>
  <div id="container">
    <h1 class="title">CSS入门学习</h1>
    <p>CSS用来控制网页的样式和布局。</p>
    <p>核心是「选择器 + 样式属性」。</p>
  </div>
</body>
</html>
```

### 总结
1. CSS 的核心作用是**分离网页结构和样式**，推荐使用**外部样式表**管理样式；
2. CSS 语法核心是`「选择器 {样式属性：值；}」`，新手先掌握类选择器、标签选择器；
3. 优先级规则：`行内样式 > ID 选择器 > 类选择器 > 标签选择器`，层叠特性让样式可以叠加生效。

## JavaScript 简单概述

JavaScript（简称 JS）是一门**跨平台、面向对象**的脚本语言，核心作用是为网页添加**交互功能**（比如点击按钮、表单验证、数据动态展示等），和 HTML（结构）、CSS（样式）共同构成前端三大核心技术。

### 核心使用方式

1. 内部脚本（写在 HTML 的 script 标签里）
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JS内部脚本</title>
</head>
<body>
  <button onclick="alert('你点击了按钮！')">点我</button>

  <!-- JS代码写在script标签中，建议放在body末尾（避免DOM未加载完成） -->
  <script>
    // 单行注释：这是JS的入门代码
    /* 多行注释：
       控制台打印（调试常用）
       console.log() 是JS最基础的输出方式
    */
    console.log("Hello JavaScript!");

    // 弹出提示框
    alert("欢迎学习JS！");
  </script>
</body>
</html>
```

2. 外部脚本（单独的 `.js` 文件，**推荐**）
    - 创建 main.js 文件：
    ```JavaScript
    // main.js - 外部JS文件
    // 向控制台输出内容
    console.log("这是外部JS文件的代码");

    // 获取页面中的按钮元素，并添加点击事件
    const btn = document.getElementById("myBtn");
    btn.addEventListener("click", function() {
    alert("你点击了外部脚本的按钮！");
    });
    ```

    - 在 HTML 中引入：
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>JS外部脚本</title>
    </head>
    <body>
    <button id="myBtn">点我（外部脚本）</button>

    <!-- 引入外部JS文件，src指定文件路径 -->
    <script src="main.js"></script>
    </body>
    </html>
    ```

    核心原则：`script` 标签要么写内部代码（无 `src`），要么引入外部文件（有 `src`），不能同时做两件事。

### JS 核心基础

1. 变量与数据类型

变量是存储数据的容器，JS 是弱类型语言，用 `let/const` 声明（替代老旧的var）：
```JavaScript
// 变量声明：
let name = "张三"; // 字符串类型（用单/双引号包裹）
let age = 20;      // 数字类型（整数/小数）
let isStudent = true; // 布尔类型（true/false）
const PI = 3.14159;   // 常量（值不可修改）

// 控制台打印变量
console.log("姓名：", name);
console.log("年龄：", age);
```

核心区别：**let**：变量可重新赋值（如 `age = 21;`）；**const**：常量不可修改（声明时必须赋值）。

2. 运算符与流程控制
    - 常用运算符
    ```JavaScript
    // 算术运算符：+ - * / %（加减乘除取余）
    let sum = 10 + 5; // 15
    let remainder = 10 % 3; // 1

    // 比较运算符：==（值相等） ===（值+类型都相等） > < >= <=
    console.log(10 == "10"); // true（仅值相等）
    console.log(10 === "10"); // false（类型不同）

    // 逻辑运算符：&&（且） ||（或） !（非）
    let a = true, b = false;
    console.log(a && b); // false
    console.log(a || b); // true
    ```

    - 流程控制（条件 / 循环）
    ```JavaScript
    // 1. if-else 条件判断
    let score = 85;
    if (score >= 90) {
    console.log("优秀");
    } else if (score >= 70) {
    console.log("良好");
    } else {
    console.log("及格");
    }

    // 2. for 循环（重复执行代码）
    for (let i = 0; i < 5; i++) {
    console.log("循环次数：", i); // 输出 0-4
    }

    // 3. 数组遍历（常用）
    let fruits = ["苹果", "香蕉", "橙子"];
    fruits.forEach(function(fruit, index) {
    console.log(`第${index+1}个水果：${fruit}`);
    });
    ```

3. 函数（封装可复用的代码）

函数是 JS 的核心，用于封装重复逻辑：
```JavaScript
// 定义函数：计算两个数的和
function add(num1, num2) {
  return num1 + num2; // 返回结果
}

// 调用函数
let result = add(5, 3);
console.log("5+3=", result); // 输出 8

// 箭头函数（简化写法，ES6+）
const multiply = (a, b) => a * b;
console.log("2*4=", multiply(2, 4)); // 输出 8
```

4. DOM 操作（核心：操作网页元素）

DOM（文档对象模型）是 JS 操作 HTML 元素的接口：
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>DOM操作示例</title>
</head>
<body>
  <h1 id="title">初始标题</h1>
  <p class="content">初始内容</p>
  <button id="changeBtn">修改内容</button>

  <script>
    // 1. 获取元素（三种常用方式）
    const title = document.getElementById("title"); // 通过ID获取
    const content = document.getElementsByClassName("content")[0]; // 通过类名获取（返回数组）
    const btn = document.querySelector("#changeBtn"); // 通过选择器获取（CSS语法）

    // 2. 修改元素内容/样式
    btn.addEventListener("click", function() {
      title.innerText = "修改后的标题"; // 修改文本内容
      content.style.color = "red"; // 修改样式（CSS属性用驼峰命名，如backgroundColor）
      content.innerHTML = "<b>加粗的内容</b>"; // 可解析HTML标签
    });
  </script>
</body>
</html>
```

核心方法：
- **获取元素**：`getElementById()`、`querySelector()`（最灵活）；
- **修改内容**：`innerText`（纯文本）、`innerHTML`（可解析 HTML）；
- **修改样式**：元素.style.样式属性（如 `style.fontSize = "20px"`）；
- **绑定事件**：`addEventListener("事件名", 回调函数)`（如 click 点击事件）。

示例：输入两个数字，点击按钮后显示求和结果；若输入非数字，提示错误
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>JS完整示例</title>
  <style>
    #result {
      margin-top: 20px;
      font-size: 18px;
      color: #3498db;
    }
  </style>
</head>
<body>
  <h2>简易计算器</h2>
  <input type="number" id="num1" placeholder="输入第一个数">
  <input type="number" id="num2" placeholder="输入第二个数">
  <button id="calcBtn">计算求和</button>
  <div id="result"></div>

  <script>
    // 获取元素
    const num1Input = document.getElementById("num1");
    const num2Input = document.getElementById("num2");
    const calcBtn = document.getElementById("calcBtn");
    const resultDiv = document.getElementById("result");

    // 绑定点击事件
    calcBtn.addEventListener("click", function() {
      // 获取输入值并转换为数字
      const num1 = Number(num1Input.value);
      const num2 = Number(num2Input.value);

      // 验证输入
      if (isNaN(num1) || isNaN(num2)) {
        resultDiv.innerText = "请输入有效的数字！";
        resultDiv.style.color = "red";
        return;
      }

      // 计算并展示结果
      const sum = num1 + num2;
      resultDiv.innerText = `求和结果：${num1} + ${num2} = ${sum}`;
      resultDiv.style.color = "#3498db";
    });
  </script>
</body>
</html>
```

### 总结
1. JS 核心作用是**实现网页交互**，推荐将代码写在外部 `.js` 文件中，且 `script标签` 放在 `body` 末尾；
2. 变量优先用 `let/const` 声明，流程控制（`if/for`）实现逻辑分支，函数封装复用代码；
3. DOM 操作是 JS 操作网页的核心，核心步骤：获取元素 → 修改内容 / 样式 → 绑定事件。