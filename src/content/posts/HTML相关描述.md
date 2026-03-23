---
title: HTML相关描述
published: 2026-03-21T22:04:40
description: '页面的描述，web测试中需要理解前端的一些语言'
image: 'https://list.yppp.net/d/image/352672f885bab2a5d285e326b940ef49.jpg'
tags: [HTML]
category: 软件测试
draft: false 
lang: ''
---

在移动端开发中，使用原生的编程语言(**安卓和 iOS 都是分开的**)，外加 HTML5 ，则被称为`混合开发`，未加则是`原生开发`。

## 一、核心定义

HTML（超文本标记语言）是**用来构建网页结构**的`标记语言`，不是编程语言，通过各种标签描述网页的文字、图片、链接、表单等元素的布局和展示。

基础结构：
```html
<!DOCTYPE html>  <!-- 声明文档类型，HTML5简化版 -->
<html lang="zh-CN">  <!-- 根标签，lang指定语言 -->
<head>  <!-- 头部：页面元信息，不显示在页面上 -->
    <meta charset="UTF-8">  <!-- 编码格式，避免乱码（测试必查） -->
    <title>网页标题</title>  <!-- 浏览器标签页标题 -->
</head>
<body>  <!-- 主体：页面可见内容都在这里 -->
    这是页面内容
</body>
</html>
```

### 标题标签

在 HTML 中，**标题标签（Heading Tags）** 是用来定义网页中不同层级标题的标签，它们不仅能让文本以不同大小显示，更重要的是赋予文本语义化的含义，帮助搜索引擎和浏览器理解页面结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>标题标签示例</title>
    <!-- 可选：自定义标题样式，覆盖默认样式 -->
    <style>
        h1 { color: #2c3e50; font-size: 28px; }
        h2 { color: #3498db; font-size: 24px; }
        h3 { color: #2ecc71; font-size: 20px; }
        h4, h5, h6 { color: #95a5a6; font-size: 18px; }
    </style>
</head>
<body>
    <h1>一级标题（页面主标题，建议一个页面仅1个）</h1>
    <h2>二级标题（主要章节标题）</h2>
    <h3>三级标题（章节下的子标题）</h3>
    <h4>四级标题（更细分的子标题）</h4>
    <h5>五级标题</h5>
    <h6>六级标题（层级最低）</h6>
</body>
</html>
```

核心使用规则：
1. **语义优先，而非样式**：不要为了调整字体大小滥用标题标签（比如用 `<h3>` 代替 `<p>` 加样式），标题标签的核心是表达内容层级，样式可以通过 CSS 自定义。
2. **层级有序**：标题应按 `<h1> → <h2> → <h3>` 的顺序使用，不要跳过层级（比如直接从 `<h1>` 跳到` <h3>`）。
3. **`<h1>` 的使用规范**：一个页面建议只使用 1 个 `<h1>`，作为页面的核心主题（比如网站名称、文章标题），这对 SEO（搜索引擎优化）至关重要。
4. **不可嵌套**：标题标签不能互相嵌套（比如 `<h1><h2>`错误`</h2></h1>` 是无效的）。

### 段落标签

在 HTML 中，**段落标签**是用来定义文本段落的核心语义标签，它会自动为文本块添加上下外边距，将内容分隔成独立的段落，让页面结构更清晰、易读。

`<p>` 标签是双标签（有开始和结束），内容写在 `<p>` 和 `</p>` 之间，浏览器会自动在段落前后添加空白（默认的外边距），无需手动加换行符。
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>段落标签示例</title>
    <!-- 可选：自定义段落样式 -->
    <style>
        p {
            font-size: 16px;    /* 字体大小 */
            line-height: 1.8;   /* 行高，提升可读性 */
            color: #333;        /* 字体颜色 */
            margin: 10px 0;     /* 上下外边距10px，左右0 */
            text-indent: 2em;   /* 首行缩进2个字符（中文常用） */
        }
    </style>
</head>
<body>
    <h1>春天的田野</h1>
    <h2>田野里的生机</h2>
    <p>春风拂过田野，麦苗探出嫩绿的脑袋，在微风中轻轻摇晃。田埂边的野花也悄悄绽放，紫的、黄的、白的，星星点点铺满了地头。</p>
    <p>不远处的小河解冻了，流水叮咚作响，像是在唱着欢快的歌。几只燕子掠过水面，剪碎了倒映在水里的云影，给寂静的田野添了几分灵动。</p>
    
    <!-- 空段落（无实际意义，不建议使用） -->
    <!-- <p></p> -->
</body>
</html>
```

1. **语义优先**：`<p>` 标签的核心是标记 “段落文本”，不要用它单纯来添加空白（空白可通过 CSS 的 margin/padding 控制）。
2. **避免空段落**：不要写 `<p></p>` 这种空段落来换行，既不符合语义，也不利于 SEO，换行建议用 `<br>`（换行标签）或 CSS。
3. **嵌套规则**：<p> 标签内不能嵌套块级元素（比如 `<h1>、<div>、<p>` 本身），只能嵌套行内元素（比如 `<span>、<a>、<em>` 等）。

    错误示例：`<p><h3>`错误`</h3></p>`（浏览器会自动拆分，导致结构混乱）。

4. **换行 vs 分段**：

    同一内容内的换行用 `<br>`：`<p>`第一行`<br>`第二行`</p>`（仍属于同一个段落）。

    不同内容块用 `<p>` 分段：`<p>`第一段<`/p><p>`第二段`</p>`（两个独立段落，有默认间距）。

### 超链接标签

在 HTML 中，超链接标签（`<a>` 标签，全称 Anchor） 是实现页面跳转、资源链接的核心标签，也是构成网页 “互联” 特性的基础。`<a>` 标签通过 href 属性指定链接目标，还能通过其他属性控制链接的行为和样式。

`<a>` 是双标签，核心属性是 href（指定链接地址），常用辅助属性包括 target（指定打开方式）、title（鼠标悬浮提示）等。
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>超链接标签示例</title>
    <style>
        /* 自定义链接样式（覆盖默认蓝色下划线） */
        a {
            color: #2c3e50;       /* 链接默认颜色 */
            text-decoration: none;/* 去掉下划线 */
            margin-right: 10px;
        }
        a:hover {
            color: #3498db;       /* 鼠标悬浮时颜色 */
            text-decoration: underline; /* 悬浮显示下划线 */
        }
        a:visited {
            color: #9b59b6;       /* 已访问链接颜色 */
        }
    </style>
</head>
<body>
    <h2>超链接的常见类型</h2>

    <!-- 1. 跳转到外部网站（绝对路径） -->
    <p>访问百度：<a href="https://www.baidu.com" target="_blank" title="打开百度首页">百度首页</a></p>

    <!-- 2. 跳转到本站内的页面（相对路径） -->
    <p>返回首页：<a href="index.html">首页</a></p>

    <!-- 3. 跳转到页面内的锚点（锚链接） -->
    <p><a href="#section1">跳转到“段落1”</a></p>
    <!-- 定义锚点（id 与 href 对应） -->
    <h3 id="section1">段落1</h3>
    <p>这是锚点对应的内容，滚动页面后点击链接会直接定位到这里。</p>
    <!-- 回到顶部的锚链接（# 代表页面顶部） -->
    <p><a href="#">回到顶部</a></p>

    <!-- 4. 链接到邮箱/电话 -->
    <p>联系我们：<a href="mailto:contact@example.com">发送邮件</a></p>
    <p>拨打电话：<a href="tel:10086">10086</a></p>

    <!-- 5. 下载文件（href 指向文件地址） -->
    <p>下载文档：<a href="docs/说明文档.pdf" download="网站使用说明.pdf">说明文档</a></p>
</body>
</html>
```

| 属性 | 作用 |
| --- | --- |
| href | 必选属性，指定链接目标（网址、页面路径、锚点、邮箱 / 电话、文件地址等） |
| target | 可选，指定链接打开方式：- _blank：新标签页打开（最常用）- _self：当前标签页打开（默认） |
| title | 可选，鼠标悬浮在链接上时显示的提示文本，提升用户体验 |
| download | 可选，点击链接时触发文件下载（值为下载后的文件名） |

1. **忘记写 href**：`<a>链接</a>` 只是普通文本，必须加 href 才是有效链接；若暂时不想跳转，可写 **href="javascript:;"** 占位。
2. **绝对路径 vs 相对路径混淆**：
    - 跳转到外部网站用绝对路径（带 `http:///https://`）；
    - 跳转到本站内页面用相对路径（如 `index.html、../about.html`）。
3. **锚链接使用错误**：锚点需定义 `id`（如 `<h3 id="section1">`），链接的 `href` 要加 `#`（如 `href="#section1"`）。
4. **样式覆盖问题**：浏览器默认给链接加蓝色下划线，可通过 CSS 的 `text-decoration: none` 去掉，`hover` 伪类可自定义悬浮效果。

### 图片标签

在 HTML 中，**图片标签**是用来在网页中嵌入图片的核心标签，它是单标签（没有结束标签），通过核心属性指定图片地址、替代文本等关键信息，是网页可视化呈现的基础。

`<img>` 标签的核心属性是 `src`（指定图片地址）和 `alt`（图片加载失败时的替代文本），还可通过 `width / height` 控制尺寸，`title` 增加悬浮提示。
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>图片标签示例</title>
    <style>
        /* 自定义图片样式，避免变形 */
        img {
            /* 只设置宽度/高度其中一个，另一个自动等比缩放，防止图片变形 */
            max-width: 100%;
            height: auto;
            /* 加边框和间距，提升美观度 */
            border: 1px solid #eee;
            padding: 5px;
            margin: 10px 0;
            border-radius: 4px; /* 圆角 */
        }
        /* 图片容器，限制最大宽度 */
        .img-container {
            max-width: 800px;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <div class="img-container">
        <h2>图片标签的常见用法</h2>

        <!-- 1. 基础用法（本地图片，相对路径） -->
        <p>本地图片：</p>
        <img 
            src="images/flower.jpg"  <!-- 图片路径（相对路径） -->
            alt="一朵粉色的花"       <!-- 替代文本（必写，提升可访问性） -->
            width="500"             <!-- 宽度（单位px，可省略） -->
            title="春日盛开的花"     <!-- 鼠标悬浮提示文本 -->
        >

        <!-- 2. 网络图片（绝对路径） -->
        <p>网络图片：</p>
        <img 
            src="https://picsum.photos/500/300"  <!-- 免费测试图片地址 -->
            alt="风景图"
            height="300"                         <!-- 高度（仅设高度，宽度自动等比） -->
        >

        <!-- 3. 图片作为超链接（嵌套在<a>标签内） -->
        <p>可点击的图片：</p>
        <a href="https://www.example.com" target="_blank" title="点击跳转到示例网站">
            <img src="https://picsum.photos/200/100" alt="示例网站封面" width="200">
        </a>

        <!-- 4. 图片加载失败的兜底（alt文本生效） -->
        <p>加载失败的图片（alt文本展示）：</p>
        <img src="images/不存在的图片.jpg" alt="图片加载失败，请检查路径">
    </div>
</body>
</html>
```

| 属性 | 作用 |
| --- | --- |
| src | 必选属性，指定图片地址：`一般本地图片用相对路径，网络图片用绝对路径` |
| alt | 必选属性（语义化 / SEO 必备），图片加载失败时显示的文本，也供屏幕阅读器读取，提升可访问性 |
| width/height | 可选，设置图片尺寸（单位默认 px）；⚠️ 建议只设置其中一个，另一个自动等比缩放，避免图片变形 |
| title | 可选，鼠标悬浮在图片上时显示的提示文本 |
| loading | 可选，现代浏览器支持，loading="lazy" 表示懒加载（滚动到图片位置才加载），提升页面加载速度 |

1. **忘记写 alt 属性**：alt 是必选的语义化属性，不仅能在图片加载失败时提示用户，还能帮助搜索引擎理解图片内容，提升 SEO 效果
2. **同时设置宽高导致变形**：如果手动同时设置 `width 和 height`，且比例与原图不一致，图片会拉伸 / 压缩变形；建议只设一个，或用 `CSS max-width:100%; height:auto;` 自适应
3. **路径错误导致图片不显示**：其中 `./` 表示同级目录，`../` 表示上一级目录
4. **忽略图片格式**：网页常用图片格式：jpg（色彩丰富，无透明）、png（支持透明）、webp（体积更小，兼容性好）、svg（矢量图，放大不失真）

### 表格标签

HTML 表格标签用于在网页中**结构化展示二维数据**（行 + 列），比如成绩表、商品清单、财务数据等，核心是让数据排列整齐、层次清晰。

基础骨架标签：
| 标签      | 全称 / 含义            | 核心说明                    |
|---------|--------------------|-------------------------|
| `<table>` | Table（表格）          | 表格的外层容器，所有表格内容都包裹在里面    |
| `<tr>`    | Table Row（表格行）     | 定义表格的一行，只能包含`<td>/<th>`   |
| `<td>`    | Table Data（表格数据）   | 普通单元格，存放具体数据（默认左对齐）     |
| `<th>`    | Table Header（表格表头） | 表头单元格，默认居中、加粗，描述列 / 行含义 |

纯用基础标签能做表格，但语义化标签能让表格结构更清晰（对搜索引擎、屏幕阅读器更友好）：
| 标签        | 作用   | 位置 / 使用说明                      |
|-----------|------|--------------------------------|
| `<thead>`   | 表格头部 | 包裹表头行（`<tr>+<th>`），放在`<table>`内最上方 |
| `<tbody>`   | 表格主体 | 包裹数据行（`<tr>+<td>`），可多个（比如分页数据）   |
| `<tfoot>`   | 表格底部 | 包裹合计 / 备注行，会优先渲染（即使写在`<tbody>`前） |
| `<caption>` | 表格标题 | 放在`<table>`内第一个位置，默认居中显示表格名称     |

不是标签，是`<td>/<th>`的核心属性，用于合并单元格（高频实用）：
| 属性      | 作用         | 示例                    |
|---------|------------|-----------------------|
| `colspan` | 跨列合并（左右合并） | colspan=""3"" → 跨 3 列 |
| `rowspan` | 跨行合并（上下合并） | rowspan=""2"" → 跨 2 行 |

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>学生成绩表</title>
  <style>
    /* 简单美化，让表格更易读 */
    table {
      width: 500px;
      border-collapse: collapse; /* 合并边框 */
      text-align: center;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 8px;
    }
    thead {
      background-color: #f5f5f5;
    }
    tfoot {
      font-weight: bold;
    }
  </style>
</head>
<body>
  <table>
    <!-- 表格标题 -->
    <caption>2024级学生期末成绩表</caption>
    
    <!-- 表头区域 -->
    <thead>
      <tr>
        <th rowspan="2">姓名</th> <!-- 跨行合并2行 -->
        <th colspan="2">文化课</th> <!-- 跨列合并2列 -->
        <th rowspan="2">体育</th>
      </tr>
      <tr>
        <th>语文</th>
        <th>数学</th>
      </tr>
    </thead>
    
    <!-- 数据主体 -->
    <tbody>
      <tr>
        <td>小明</td>
        <td>95</td>
        <td>98</td>
        <td>85</td>
      </tr>
      <tr>
        <td>小红</td>
        <td>92</td>
        <td>90</td>
        <td>90</td>
      </tr>
    </tbody>
    
    <!-- 表格底部（合计） -->
    <tfoot>
      <tr>
        <td>平均分</td>
        <td>93.5</td>
        <td>94</td>
        <td>87.5</td>
      </tr>
    </tfoot>
  </table>
</body>
</html>
```

新手常见注意事项：
- 不要用表格做页面布局！现在主流用 Flex/Grid 布局，表格仅用于展示数据。
- `border-collapse: collapse`; 是表格美化的常用 CSS，能让边框不重叠。
- 合并单元格后，要删除被合并的多余单元格（比如跨 2 列后，同行要少写 1 个`<td>`）。

总结：
- 表格核心骨架是 `<table> + <tr> + <td>/<th>`，`<th>` 专做表头、默认加粗居中。
- `<thead>/<tbody>/<tfoot>` 是语义化标签，能让表格结构更清晰，优先使用。
- 单元格合并靠 `colspan（跨列）`和 `rowspan（跨行）`，是表格的核心实用技巧。

### 列表标签

HTML 列表标签用于**有序 / 无序地展示一组相关内容**，比如导航菜单、步骤说明、商品卖点等，核心是让内容排列更有规律、可读性更强，是网页开发中高频使用的标签。

HTML 列表主要分 3 类：`无序列表`、`有序列表`、`定义列表`，各自有明确的使用场景和语法。
1. 无序列表（最常用）

| 标签   | 全称 / 含义              | 核心说明                 |
|------|----------------------|----------------------|
| `<ul>` | Unordered List（无序列表） | 列表外层容器，内容是一组无固定顺序的项  |
| `<li>` | List Item（列表项）       | 定义列表中的每一项，必须嵌套在`<ul>`内 |

特点：默认显示「`圆点 (●)`」作为项目符号，可通过 CSS 修改为方块、空心圆或自定义样式。s

2. 有序列表

| 标签   | 全称 / 含义            | 核心说明                 |
|------|--------------------|----------------------|
| `<ol>` | Ordered List（有序列表） | 列表外层容器，内容是一组有固定顺序的项  |
| `<li>` | List Item（列表项）     | 定义列表中的每一项，必须嵌套在`<ol>`内 |

特点：默认显示「数字 (1、2、3)」作为序号，可通过`type`属性修改序号类型（字母、罗马数字等），也可通过`start`属性指定起始序号。

3. 定义列表（语义化，用于 “名词 + 解释”）

| 标签   | 全称 / 含义                      | 核心说明                 |
|------|------------------------------|----------------------|
| `<dl>` | Definition List（定义列表）        | 外层容器，包裹 “名词 + 解释” 组合 |
| `<dt>` | Definition Term（定义术语）        | 要解释的`名词 / 标题`          |
| `<dd>` | Definition Description（定义描述） | 对`<dt>`的`解释 / 说明`，默认缩进显示 |

下面的示例涵盖所有列表类型，附带简单 CSS 美化，方便理解和复用：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>HTML列表标签示例</title>
  <style>
    /* 简单美化，区分不同列表 */
    .list-box {
      margin: 20px;
      padding: 10px;
      border: 1px solid #eee;
      width: 400px;
    }
    ul {
      list-style-type: square; /* 无序列表改为方块符号 */
    }
    ol[type="A"] {
      list-style-position: inside; /* 序号和内容同排 */
    }
    dd {
      color: #666;
      margin-left: 20px;
    }
  </style>
</head>
<body>
  <!-- 1. 无序列表（导航/卖点示例） -->
  <div class="list-box">
    <h3>无序列表（商品卖点）</h3>
    <ul>
      <li>支持快充（66W）</li>
      <li>5000mAh大容量电池</li>
      <li>防水防尘（IP68）</li>
    </ul>
  </div>

  <!-- 2. 有序列表（步骤/教程示例） -->
  <div class="list-box">
    <h3>有序列表（泡茶步骤）</h3>
    <ol type="A" start="2"> <!-- 序号类型：大写字母，起始为B -->
      <li>温杯：用热水冲洗茶杯</li>
      <li>投茶：放入3-5g茶叶</li>
      <li>注水：倒入80℃热水</li>
      <li>出汤：等待30秒后倒出茶汤</li>
    </ol>
  </div>

  <!-- 3. 定义列表（名词解释示例） -->
  <div class="list-box">
    <h3>定义列表（技术术语）</h3>
    <dl>
      <dt>HTML</dt>
      <dd>超文本标记语言，用于构建网页结构的标记语言。</dd>
      <dt>CSS</dt>
      <dd>层叠样式表，用于美化网页样式的语言。</dd>
      <dt>JavaScript</dt>
      <dd>脚本语言，用于实现网页交互功能。</dd>
    </dl>
  </div>
</body>
</html>
```

新手常见注意事项：
1. **嵌套规则**：`<li>`必须嵌套在`<ul>/<ol>`内，不能单独使用；列表可以嵌套（比如`<ul>`里的`<li>`中再放`<ul>`，实现多级菜单）。
2. **样式修改**：尽量用 CSS（list-style-type）修改列表符号，而非 HTML 属性（比如`<ul type="square">`），符合 “结构与样式分离” 的最佳实践。
3. **使用场景**：
    - 无序列表：导航栏、商品卖点、标签云等无顺序的内容；
    - 有序列表：教程步骤、排行榜、考试答案等有顺序的内容；
    - 定义列表：词典解释、产品参数说明、FAQ 问答等 “名词 + 解释” 的内容。

总结：
- HTML 列表分 3 类：`<ul>`（无序列表，无固定顺序）、`<ol>`（有序列表，有固定顺序）、`<dl>`（定义列表，名词 + 解释），核心列表项都是`<li>`（`<dl>用<dt>/<dd>`）。
- 无序列表默认圆点符号，有序列表默认数字序号，可通过 CSS 自定义样式。
- 列表可嵌套使用（比如多级导航），且需遵循 “结构与样式分离”，用 CSS 控制外观而非 HTML 属性。

## div标签

`<div>` 是 HTML 中的通用块级容器标签（div 是 `division` 的缩写，意为 “分割、分区”），本身**没有任何默认样式和语义**，核心作用是：
- 把网页内容**分组、划分区域**（比如头部、主体、侧边栏）；
- 作为 CSS 样式和 JavaScript 操作的**载体**（给 div 加 class/id，就能精准控制样式 / 交互）。

简单来说：`<div>` 就是一个 **“空盒子”**，你可以往里面装任何内容（文本、图片、表格、列表、甚至其他 div），并通过样式把它变成你想要的样子。

| 特点    | 说明                                                          |
|-------|-------------------------------------------------------------|
| 块级元素  | 默认独占一行，宽度默认撑满父容器，可设置宽高、内外边距                                 |
| 无默认样式 | 默认没有边框、背景、间距，纯 “透明盒子”                                       |
| 无语义   | 仅用于布局分组，搜索引擎 / 屏幕阅读器无法识别其内容用途（对比 `<header>/<section>` 等语义化标签） |
| 可嵌套   | 支持多层嵌套（比如 div 里套 div），是网页布局的核心                              |

下面通过 “网页基础布局” 示例，展示 div 如何划分区域、配合 CSS 实现布局：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>div标签示例 - 网页布局</title>
  <style>
    /* 全局样式重置 */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: "微软雅黑";
    }

    /* 用class控制div样式，实现布局分区 */
    .container {
      width: 1200px;
      margin: 0 auto; /* 居中 */
    }
    /* 头部区域 */
    .header {
      height: 80px;
      background-color: #333;
      color: white;
      line-height: 80px;
      padding: 0 20px;
      margin-bottom: 10px;
    }
    /* 主体区域（左侧内容 + 右侧侧边栏） */
    .main {
      display: flex; /* 弹性布局，让两个div并排 */
      gap: 10px; /* 两个div之间的间距 */
    }
    .content {
      flex: 3; /* 占3份宽度 */
      height: 400px;
      background-color: #f5f5f5;
      padding: 20px;
    }
    .sidebar {
      flex: 1; /* 占1份宽度 */
      height: 400px;
      background-color: #eee;
      padding: 20px;
    }
    /* 底部区域 */
    .footer {
      height: 60px;
      background-color: #333;
      color: white;
      line-height: 60px;
      text-align: center;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- 头部 -->
    <div class="header">
      网页头部（导航栏）
    </div>

    <!-- 主体 -->
    <div class="main">
      <!-- 左侧内容区 -->
      <div class="content">
        主要内容区域<br>
        可以放文章、图片、表格等任意内容
      </div>
      <!-- 右侧侧边栏 -->
      <div class="sidebar">
        侧边栏<br>
        放推荐内容、广告、导航等
      </div>
    </div>

    <!-- 底部 -->
    <div class="footer">
      网页底部 - 版权信息 © 2024
    </div>
  </div>
</body>
</html>
```

### div 与 span 的核心区别
很多人会混淆 `<div>` 和 `<span>`，这里用表格清晰区分：
| 特性   | `<div>`            | `<span>`            |
|------|------------------|-------------------|
| 元素类型 | 块级元素             | 行内元素              |
| 排版方式 | 默认独占一行           | 与其他内容同行显示         |
| 宽高设置 | 可设置 width/height | 无法设置（宽高由内容决定）     |
| 主要用途 | 划分大区域（布局）        | 包裹小段文本 / 元素（局部样式） |

示例如下对比：
```html
<!-- div：块级，独占一行 -->
<div style="background: red; width: 200px;">我是div，独占一行</div>
<p>这是一段文字</p>

<!-- span：行内，和文字同行 -->
<p>这是<span style="color: blue; font-weight: bold;">span包裹的文字</span>，和其他文字同行</p>
```

### 语义化
虽然 div 是布局核心，但 HTML5 推出了 `<header>`、`<nav>`、`<main>`、`<footer>`、`<section>` 等语义化标签，它们本质上是 “有含义的 div”：
- 语义化标签：既有 div 的布局能力，又能让浏览器 / 搜索引擎理解内容用途（比如 `<header>` 表示头部）；
- div：无语义，适合作为 “通用容器”（比如包裹一组无特定语义的内容）。

```html
<!-- 推荐：语义化标签 + div 配合使用 -->
<header> <!-- 头部（语义化） -->
  <div class="logo">LOGO</div> <!-- 通用容器，装logo -->
  <nav> <!-- 导航（语义化） -->
    <ul>
      <li>首页</li>
      <li>关于我们</li>
    </ul>
  </nav>
</header>
```

### 总结

1. `<div>` 是**无语义的通用块级容器**，核心作用是划分网页区域、作为样式 / 交互的载体，默认独占一行、无默认样式。
2. `<div>` 用于**大区域布局**，`<span>` 用于**行内局部样式**，二者是块级与行内的核心搭配。
3. 实际开发中优先用 HTML5 语义化标签（`<header>/<main>` 等），div 作为补充的通用容器，兼顾布局灵活性和语义化。

## 表单标签

HTML 表单（`<form>`）是网页与用户交互的核心，用于收集用户输入的信息（比如登录、注册、提交订单、问卷调查等），并将数据提交到服务器处理。

简单来说：表单就是网页上的 “填写单”，用户填完后点击按钮，数据就能传给后端。

### 核心表单标签 / 属性详解(重点)

1. 基础容器与核心属性

| 标签 / 属性     | 作用        | 关键说明                                         |
|-------------|-----------|----------------------------------------------|
| `<form>`      | 表单外层容器    | 所有表单控件必须嵌套在`<form>`内                           |
| `action`      | 表单提交地址    | 数据要发送到的后端接口（比如`action=""/login""`）             |
| `method`      | 提交方式      | 常用GET（参数拼 URL，适合简单数据）/ POST（数据隐藏，适合敏感 / 大量数据） |
| `name`        | 控件命名      | 后端通过 name 获取对应值（必须设置，否则数据传不出去）                 |
| `placeholder` | 提示文本      | 输入框内的灰色提示文字（比如 “请输入手机号”）                     |
| `required`    | 必填验证      | 标记控件为必填，提交时为空会提示用户                           |
| `value`       | 默认值 / 提交值 | 控件的默认内容，或单选 / 复选框的提交值                        |

2. 常用输入控件（核心）

| 标签 / 类型                   | 作用               | 示例代码                                                                                                                  |
|---------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------|
| `<input type=""text"">`     | 单行文本输入（用户名 / 姓名） | `<input type=""text"" name=""username"" placeholder=""请输入用户名"" required>`                                               |
| `<input type=""password"">` | 密码输入（隐藏显示）       | `<input type=""password"" name=""pwd"" placeholder=""请输入密码"" required>`                                                 |
| `<input type=""radio"">`    | 单选按钮（互斥选项，比如性别）  | `<input type=""radio"" name=""gender"" value=""male""> 男<input type=""radio"" name=""gender"" value=""female""> 女`      |
| `<input type=""checkbox"">` | 复选框（多选选项，比如爱好）   | `<input type=""checkbox"" name=""hobby"" value=""read""> 阅读<input type=""checkbox"" name=""hobby"" value=""sport""> 运动` |
| `<input type=""email"">`    | 邮箱输入（自带格式验证）     | `<input type=""email"" name=""email"" required>`                                                                        |
| `<input type=""tel"">`      | 手机号输入（移动端调数字键盘）  | `<input type=""tel"" name=""phone"" placeholder=""请输入11位手机号"">`                                                         |
| `<input type=""submit"">`   | 提交按钮             | `<input type=""submit"" value=""登录"">`                                                                                  |
| `<input type=""reset"">`    | 重置按钮             | `<input type=""reset"" value=""清空"">`                                                                                   |
| `<textarea>`                | 多行文本输入（留言 / 简介）  | `<textarea name=""desc"" rows=""5"" cols=""30"">默认留言</textarea>`                                                        |
| `<select>/<option>`         | 下拉选择框（比如城市 / 年级） | `<select name=""city"">  <option value=""beijing"">北京</option>  <option value=""shanghai"">上海</option></select>`        |
| `<label>`                   | 绑定控件（点击文字也能选中）   | `<label for=""username"">用户名：</label><input type=""text"" id=""username"" name=""username"">`                           |

示例一个包含大部分常用控件的表单示例，附带 CSS 美化：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>表单标签示例 - 用户注册</title>
  <style>
    /* 简单美化，让表单更易读 */
    .form-box {
      width: 400px;
      margin: 50px auto;
      padding: 20px;
      border: 1px solid #eee;
      border-radius: 8px;
    }
    .form-item {
      margin-bottom: 15px;
    }
    label {
      display: inline-block;
      width: 80px;
      text-align: right;
      margin-right: 10px;
    }
    input[type="text"], 
    input[type="password"],
    input[type="email"],
    select {
      width: 250px;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    textarea {
      width: 250px;
      height: 100px;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    .submit-btn {
      width: 340px;
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin-left: 90px;
    }
    .submit-btn:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>
  <div class="form-box">
    <h3 style="text-align: center; margin-bottom: 20px;">用户注册表单</h3>
    <!-- 表单容器：提交方式POST，地址为示例（实际需替换为后端接口） -->
    <form action="/register" method="POST">
      <!-- 1. 用户名 -->
      <div class="form-item">
        <label for="username">用户名：</label>
        <input type="text" id="username" name="username" placeholder="请输入用户名" required>
      </div>

      <!-- 2. 密码 -->
      <div class="form-item">
        <label for="pwd">密&nbsp;&nbsp;&nbsp;&nbsp;码：</label>
        <input type="password" id="pwd" name="pwd" placeholder="请输入6-16位密码" required>
      </div>

      <!-- 3. 性别（单选） -->
      <div class="form-item">
        <label>性&nbsp;&nbsp;&nbsp;&nbsp;别：</label>
        <input type="radio" name="gender" value="male" id="male">
        <label for="male">男</label>
        <input type="radio" name="gender" value="female" id="female">
        <label for="female">女</label>
      </div>

      <!-- 4. 爱好（复选） -->
      <div class="form-item">
        <label>爱&nbsp;&nbsp;&nbsp;&nbsp;好：</label>
        <input type="checkbox" name="hobby" value="read" id="read">
        <label for="read">阅读</label>
        <input type="checkbox" name="hobby" value="sport" id="sport">
        <label for="sport">运动</label>
        <input type="checkbox" name="hobby" value="music" id="music">
        <label for="music">音乐</label>
      </div>

      <!-- 5. 所在城市（下拉框） -->
      <div class="form-item">
        <label for="city">城&nbsp;&nbsp;&nbsp;&nbsp;市：</label>
        <select name="city" id="city">
          <option value="">请选择城市</option>
          <option value="beijing">北京</option>
          <option value="shanghai">上海</option>
          <option value="guangzhou">广州</option>
        </select>
      </div>

      <!-- 6. 个人简介（多行文本） -->
      <div class="form-item">
        <label for="desc">简&nbsp;&nbsp;&nbsp;&nbsp;介：</label>
        <textarea name="desc" id="desc" rows="3" cols="30" placeholder="请输入个人简介（选填）"></textarea>
      </div>

      <!-- 7. 提交/重置按钮 -->
      <div class="form-item">
        <input type="submit" value="提交注册" class="submit-btn">
        <input type="reset" value="清空表单" style="margin-left: 10px; padding: 10px;">
      </div>
    </form>
  </div>
</body>
</html>
```

### 常见坑与注意事项
1. **必须设置name属性**：所有需要提交的控件（输入框、单选、复选等）都要加 `name`，后端靠 `name` 取值（比如 `username` 对应输入的用户名）。
2. **单选按钮的 name 要相同**：同一组单选（比如性别）必须用同一个 `name`，否则无法互斥。
3. `<label>`的绑定技巧：用 `for="控件id"` 绑定，点击文字也能选中控件（提升用户体验），不要只靠空格分隔。
4. **POST vs GET**：
    - `GET`：数据拼在 URL 后（比如 `?username=小明&pwd=123`），适合简单、非敏感数据；
    - `POST`：数据在请求体中（看不到），适合密码、表单等敏感 / 大量数据。
5. **前端验证只是辅助**：`required`、`type="email"` 等前端验证方便用户，但后端必须重新验证（防止恶意绕过）。

### 总结
1. 表单核心是`<form>`容器 + 各类输入控件，**action（提交地址）**、**method（提交方式）**、`name（控件命名）`是三大关键属性。
2. 常用控件：文本框（text）、密码框（password）、单选（radio）、复选（checkbox）、下拉框（select）、多行文本（textarea），需根据场景选择。
3. 开发要点：控件必须加 name，单选按钮 name 要统一，优先用 POST 提交敏感数据，`<label>`绑定提升用户体验。