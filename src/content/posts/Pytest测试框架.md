---
title: Pytest测试框架
published: 2026-04-28
description: ''
image: ''
tags: [软件测试]
category: '测试框架'
draft: false 
lang: ''
slug: pytest
---

## 测试框架

测试框架是抽象（隐藏底层实现细节）出来的工具集合，包含一套工具、组件和功能，用以降低测试开发成本、提升用例可维护性、执行效率与结果可信度。

测试框架一般提供`用例发现、用例管理、环境管理、用例执行和测试报告`。

## Pytest

Pytest 是 Python 生态中`最主流`的单元测试与接口自动化测试框架，以简洁灵活、插件丰富著称，`完美兼容` unittest，是测试工程师的首选工具。

核心特点：

- 简单易用：`无需继承类`，直接用函数写测试，`断言用原生 Python 语法（assert）`，学习成本极低

- 强大的 Fixture 机制：替代 unittest 的 setUp/tearDown，支持`灵活的作用域`（函数 / 类 / 模块 / 会话）和依赖注入

- 参数化测试：`同一用例可执行多组数据`，大幅提升场景覆盖率

- 丰富的`插件生态`：拥有超 1400 个插件，覆盖接口测试、性能测试、报告生成等全场景

- `兼容` unittest：可直接运行 unittest 风格的测试用例，无缝迁移旧项目

- 详细的失败信息：自动展示断言失败的具体差异，方便`快速定位`问题


### 快速入门

**安装**最新版本：`pip install pytest -U`

#### 基本用法

- 测试文件命名：以 `test_` 开头或 `_test`.py 结尾（如 test_demo.py）

- 测试函数命名：以 `test_` 开头（如 test_add()）

- 断言方式：直接用 Python 原生 `assert`

#### 示例

```python
# test_demo.py
def add(a, b):
    return a + b

def test_add_positive():
    assert add(1, 2) == 3

def test_add_negative():
    assert add(-1, -2) == -3
```

运行测试：在`终端`执行 `pytest` 即可`自动发现`并执行`所有`测试用例。也可以 `导入pytest包` 后，键入 `pytest.main()` 后右键运行。

Pytest 会先遍历项目中所有除 `venu`(Python项目构建自动生成) 和 `.`(Linux隐藏目录) 开头的目录，之后遍历以 `test_` 开头或 `_test` 结尾的文件，再遍历文件中以 `Test` 开头的类，最后收集所有的以 `test_` 开头的函数和方法。

需要注意的是，所定义的测试方法`不能有自定义参数和返回值`。

### 配置

**配置优先级（从高到低）**：

- **命令行参数**：单次执行的临时配置，优先级最高

    | **参数**                         | **作用**                               | **示例**                              |
    |:------------------------------:|:------------------------------------:|:-----------------------------------:|
    | **-v / --verbose**             | 详细输出用例执行结果                           | `pytest -v`                           |
    | **-s**                         | 关闭输出捕获，允许 `print` 打印                   | `pytest -s`                           |
    | **-k**                         | 按关键字筛选用例（支持 `and/or/not`）              | `pytest -k ""test_add and not slow""` |
    | **-m**                         | 按标记筛选用例                              | `pytest -m ""smoke and api""`         |
    | **--tb=short**                 | 简化报错信息                               | `pytest --tb=short`                   |
    | **--maxfail=3**                | 失败 3 条后停止执行                          | `pytest --maxfail=3`                  |
    | **--reruns=2**                 | 失败用例自动重试 2 次（需 `pytest-rerunfailures`） | `pytest --reruns=2`                   |
    | **-n auto**                    | 自动检测 CPU 核心数并行执行（需 `pytest-xdist`）     | `pytest -n auto`                      |
    | **--html=report.html**         | 生成 HTML 报告（需 `pytest-html`）            | `pytest --html=report.html`           |
    | **--alluredir=allure-results** | 生成 Allure 报告数据（需 `pytest-allure`）      | `pytest --alluredir=allure-results`   |
    | **--env=staging**              | 自定义参数（需在 `conftest.py` 定义）             | `pytest --env=staging`                |

- **项目配置文件**：`pyproject.toml`（推荐，PEP 621 标准）> `pytest.ini`（传统主流）> `tox.ini/setup.cfg`

- 本地 `conftest.py`：目录级配置，仅对当前目录及子目录生效

- 默认配置：Pytest 内置默认规则

**推荐配置方案**：

1. 全局项目配置：统一用 `pyproject.toml`（现代 Python 项目标准）

    ```toml
    [tool.pytest.ini_options]
    # -------------------------- 1. 测试发现规则 --------------------------
    testpaths = ["tests", "api_test"]  # 指定测试用例搜索目录（默认当前目录）
    python_files = ["test_*.py", "*_test.py"]  # 测试文件命名规则（默认）
    python_classes = ["Test*"]  # 测试类命名规则（默认）
    python_functions = ["test_*"]  # 测试函数命名规则（默认）
    norecursedirs = ["venv", ".git", "build", "dist"]  # 排除搜索的目录

    # -------------------------- 2. 自定义标记（避免警告） --------------------------
    markers = [
        "smoke: 冒烟测试用例",
        "regression: 回归测试用例",
        "api: 接口测试用例",
        "slow: 慢用例（默认跳过）"
    ]

    # -------------------------- 3. 执行规则 --------------------------
    addopts = "-v -s --strict-markers --tb=short"  # 默认命令行参数（每次执行自动添加）
    # 解释：
    # -v: 详细输出用例执行结果
    # -s: 关闭输出捕获，允许 print 打印
    # --strict-markers: 严格模式，未注册的标记会报错
    # --tb=short: 简化报错信息
    minversion = "8.0"  # 要求最低 pytest 版本
    timeout = 30  # 单条用例超时时间（需安装 pytest-timeout）
    maxfail = 5  # 失败 5 条用例后停止执行

    # -------------------------- 4. 日志配置 --------------------------
    log_cli = true  # 控制台实时输出日志
    log_cli_level = "INFO"  # 日志级别
    log_cli_format = "%(asctime)s - %(name)s - %(levelname)s - %(message)s"  # 日志格式
    log_file = "logs/pytest.log"  # 日志文件路径
    log_file_level = "DEBUG"

    # -------------------------- 5. 插件配置 --------------------------
    # pytest-html 报告配置
    htmlpath = "reports/report.html"
    self_contained_html = true  # 生成单文件 HTML 报告（包含所有资源）

    # pytest-allure 报告配置
    allure_report_dir = "allure-results"
    ```


2. 目录级 Fixture / 钩子：用 `conftest.py`（Pytest 的本地配置文件），自动对当前目录及子目录生效
    - 共享 Fixture（跨测试文件复用）
    - 定义钩子函数（自定义 Pytest 行为）
    - 添加自定义命令行参数

    ```python
    import pytest
    import requests

    # -------------------------- 1. 共享 Fixture --------------------------
    @pytest.fixture(scope="session")
    def base_url():
        """全局基础 URL，所有用例复用"""
        return "https://api.example.com"

    @pytest.fixture(scope="session")
    def login_token(base_url):
        """登录获取 Token，会话级仅执行 1 次"""
        res = requests.post(f"{base_url}/login", json={"username": "test", "password": "123456"})
        return res.json()["token"]

    # -------------------------- 2. 自定义命令行参数 --------------------------
    def pytest_addoption(parser):
        """添加 --env 参数，支持切换测试环境（dev/staging/prod）"""
        parser.addoption(
            "--env",
            action="store",
            default="dev",
            help="指定测试环境: dev/staging/prod"
        )

    @pytest.fixture(scope="session")
    def env(request):
        """获取命令行传入的环境参数"""
        return request.config.getoption("--env")

    # -------------------------- 3. 钩子函数：自定义测试报告标题 --------------------------
    def pytest_html_report_title(report):
        report.title = "接口自动化测试报告"

    # -------------------------- 4. 钩子函数：跳过标记为 slow 的用例 --------------------------
    def pytest_collection_modifyitems(config, items):
        for item in items:
            if "slow" in item.keywords:
                item.add_marker(pytest.mark.skip(reason="慢用例，默认跳过"))
    ```

3. 临时执行：用命令行参数

## 自定义标记

通过 `@pytest.mark.自定义标签名` 给测试用例 / 类 / 模块打分类标签，实现用例的精准筛选执行。

直接使用未注册的自定义标记，Pytest 会抛出 `PytestUnknownMarkWarning 警告`；若开启了 `--strict-markers` 严格模式，会直接执行报错。推荐所有自定义标记先注册再使用。

### 注册标记

1. `pyproject.toml（主流）`：在 `[tool.pytest.ini_options]` 下的 `markers` 字段注册，一行一个标记，可添加描述说明。

    ```toml
    [tool.pytest.ini_options]
    # 开启严格模式，未注册的标记直接报错
    addopts = "-v --strict-markers"
    # 注册自定义标记
    markers = [
        "smoke: 冒烟测试用例 - 核心流程校验，上线前必跑",
        "regression: 回归测试用例 - 全量功能校验，迭代后必跑",
        "api: 接口测试用例",
        "ui: 前端UI测试用例",
        "slow: 慢执行用例 - 默认跳过，仅全量回归时执行",
        "prod: 生产环境专属用例 - 仅线上巡检时执行",
        "severity: 用例优先级标记，支持传参(critical/high/medium/low)"
    ]
    ```

2. `pytest.ini（传统）`：适用于老项目

    ```ini
    [pytest]
    addopts = -v --strict-markers
    markers =
        smoke: 冒烟测试用例 - 核心流程校验，上线前必跑
        regression: 回归测试用例 - 全量功能校验，迭代后必跑
        api: 接口测试用例
        slow: 慢执行用例 - 默认跳过，仅全量回归时执行
    ```

3. `conftest.py` 动态注册：通过 `pytest_configure` 钩子函数动态注册，适合需要动态生成标记的`大型项目`
    ```python
    def pytest_configure(config):
        # 注册自定义标记
        config.addinivalue_line(
            "markers", "smoke: 冒烟测试用例 - 核心流程校验，上线前必跑"
        )
        config.addinivalue_line(
            "markers", "slow: 慢执行用例 - 默认跳过，仅全量回归时执行"
        )
    ```

执行以下命令，可查看所有已注册的标记（包含内置标记 + 自定义标记）：

```bash
pytest --markers
```

### 添加自定义标记

支持`函数级、类级、模块级`三种作用范围，可给单个用例打多个标记。

1. 函数级标记（最常用）：仅对当前测试函数生效

    ```python
    import pytest
    import time

    # 被测函数
    def add(a, b):
        return a + b

    # 单个标记
    @pytest.mark.smoke
    def test_add_basic():
        assert add(1, 2) == 3

    # 多个标记（同时打多个标签）
    @pytest.mark.smoke
    @pytest.mark.api
    def test_add_negative():
        assert add(-1, -2) == -3

    # 带参数的标记
    @pytest.mark.severity("critical")
    def test_add_zero():
        assert add(0, 5) == 5

    # 慢用例标记
    @pytest.mark.slow
    def test_add_large_number():
        time.sleep(3)
        assert add(10000, 20000) == 30000
    ```

2. 类级标记：标记会自动继承到类内所有测试方法
    ```python
    import pytest

    @pytest.mark.api
    @pytest.mark.regression
    class TestOrder:
        # 自动继承 api + regression 标记
        def test_create_order(self):
            assert True

        # 额外新增 smoke 标记，最终标记：api + regression + smoke
        @pytest.mark.smoke
        def test_query_order(self):
            assert True
    ```

3. 模块级标记：文件内所有测试函数 / 类都会继承该标记，需在文件顶部定义 `pytestmark 变量`

    ```python
    import pytest

    # 单个模块级标记
    # pytestmark = pytest.mark.api

    # 多个模块级标记
    pytestmark = [pytest.mark.api, pytest.mark.regression]

    # 自动继承 api + regression 标记
    def test_user_login():
        assert True

    # 自动继承 api + regression 标记，额外新增 smoke 标记
    @pytest.mark.smoke
    def test_user_info():
        assert True
    ```

4. 参数化用例的标记

    ```python
    import pytest

    @pytest.mark.parametrize("a, b, expected", [
        # 给这组数据打smoke标记
        pytest.param(1, 2, 3, marks=pytest.mark.smoke),
        # 给这组数据打slow标记
        pytest.param(10000, 20000, 30000, marks=pytest.mark.slow),
        # 无标记
        (-1, -2, -3),
        (0, 5, 5)
    ])
    def test_add(a, b, expected):
        assert a + b == expected
    ```

#### 执行自定义标记

通过命令行 `-m` 参数，配合`逻辑运算符`，精准筛选要执行的用例，这是自定义标记最核心的用途。

| **需求**            | **执行命令**                                      | **说明**                       |
|:-----------------:|:---------------------------------------------:|:----------------------------:|
| **仅执行指定标记的用例**    | `pytest -m smoke`                               | 只跑打了 smoke 标记的用例             |
| **执行同时包含多个标记的用例** | `pytest -m ""smoke and api""`                   | 只跑同时打了 smoke 和 api 标记的用例     |
| **执行包含任意一个标记的用例** | `pytest -m ""smoke or regression""`             | 跑打了 smoke 或 regression 标记的用例 |
| **排除指定标记的用例**     | `pytest -m ""not slow""`                        | 跑所有没打 slow 标记的用例             |
| **复杂逻辑组合**        | `pytest -m ""smoke and not prod and not slow""` | 跑冒烟用例，排除生产环境和慢用例             |

### 跳过

1. `@pytest.mark.skip`：直接跳过指定用例，`无论任何情况都不执行`，适用于已知暂不执行的用例（如功能未开发完成、用例废弃）

    ```python
    import pytest

    @pytest.mark.skip(reason="功能暂未实现，待开发完成后执行")
    def test_unfinished_feature():
        assert False  # 不会执行到这里
    ```

2. `@pytest.mark.skipif（最常用）`：`满足特定条件时才跳过`，不满足条件则正常执行，适用于多环境适配、版本兼容、依赖缺失等场景。

    ```python
    import pytest
    import sys
    import os

    # 场景1：Python版本低于3.10时跳过
    @pytest.mark.skipif(sys.version_info < (3, 10), reason="仅支持Python 3.10及以上版本")
    def test_python_310_feature():
        assert True

    # 场景2：Windows系统下跳过（仅Linux/macOS执行）
    @pytest.mark.skipif(os.name == "nt", reason="仅支持Linux/macOS系统")
    def test_linux_only():
        assert True

    # 场景3：环境变量未设置时跳过
    @pytest.mark.skipif(os.getenv("API_KEY") is None, reason="未设置API_KEY环境变量，跳过接口测试")
    def test_api_with_key():
        assert True

    # 场景4：依赖库未安装时跳过
    try:
        import pandas
    except ImportError:
        pandas = None

    @pytest.mark.skipif(pandas is None, reason="未安装pandas库，跳过相关测试")
    def test_pandas_feature():
        assert True
    ```

### 参数化

用 `@pytest.mark.parametrize` 实现同一用例多组数据执行。

```python
import pytest

# 参数，数据
@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (-1, -2, -3),
    (0, 5, 5)
])
def test_add(a, b, expected):
    assert add(a, b) == expected
```

### Fixture 机制

用于测试数据准备、环境初始化等`前置 / 后置`操作，支持作用域控制。

```python
import pytest

# 定义 Fixture，作用域为函数级（默认）
@pytest.fixture
def login():
    print("登录操作")
    yield "token_123"  # yield 前是前置，后是后置
    print("退出登录")

# 测试函数直接注入 Fixture
def test_order(login):
    print(f"使用 {login} 下单")
    assert True
```