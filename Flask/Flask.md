# 一、Flask 框架介绍

## 1、是什么

- Flask 是<font color="red">**Python 轻量级 Web 开发框架**</font>
- 基于 Werkzeug（WSGI工具集）和 Jinja2（模板引擎）两大核心组件
- 遵循 <font color="red">**MIT 开源协议**</font>，属于<font color="red">**微框架 (Micro‑framework)**</font>
- 对比Django（大而全的框架），Flask更适合小型项目、快速原型开发或需要高度定制化的场景。

> “微” 不是功能弱，而是**内核极简，只保留 Web 最核心能力**，其余功能全部靠第三方扩展实现，按需安装。



## 2、核心两大依赖

1. **Werkzeug**：WSGI 工具库，负责路由分发、请求 / 响应处理、Cookie、HTTP 底层，不自带模板。
2. **Jinja2**：强大的 HTML 模板引擎，支持变量渲染、循环、条件、模板继承。



## 3、Flask 特点

- 优点
  - 轻量小巧，上手简单，代码量少，适合快速原型、小型项目、API 服务
  - 高度灵活，没有强制项目结构，开发者自由组织代码 
  - 扩展生态丰富，需要什么装什么，不捆绑冗余组件，如：Flask-SQLAlchemy（数据库）、Flask-WTF（表单）、Flask-Login（用户认证）等
  - 原生支持 RESTful API，非常适合写后端接口 
  - 适合小网站、内部工具、AI 后端服务、管理后台；大型项目需要自己做分层架构

- 缺点
  - 原生不提供：ORM 数据库、表单校验、会话存储、权限管理，需要引入扩展



## 4、常用第三方扩展

| 扩展包             | 作用                           |
| ------------------ | ------------------------------ |
| Flask‑SQLAlchemy   | ORM 数据库操作（MySQL/SQLite） |
| Flask‑Login        | 用户登录会话管理               |
| Flask‑WTF          | 表单验证、CSRF 防护            |
| Flask‑CORS         | 解决前后端跨域问题             |
| Flask‑Migrate      | 数据库迁移                     |
| Flask‑JWT‑Extended | JWT 令牌鉴权，做接口登录       |



## 5、Flask 项目两种开发模式

1. **单文件模式**：所有代码写在一个 app.py，适合小 demo、测试接口
2. **工厂模式 (Flask Application Factory)**：大型项目推荐，拆分路由、模型、配置，模块化，支持多实例。

工厂模式简单结构：

```
project/
├── app/
│   ├── __init__.py   # create_app()工厂函数
│   ├── routes.py     # 路由
│   └── models.py     # 数据库模型
└── run.py            # 入口启动
```



## 6、Flask vs FastAPI

| 特性         | Flask                   | FastAPI                 |
| ------------ | ----------------------- | ----------------------- |
| Python 版本  | Python3.7+              | Python3.8+              |
| 类型         | 同步 WSGI 框架          | 异步 ASGI 框架          |
| 性能         | 一般                    | 高，支持 async/await    |
| 自动接口文档 | 无（需要 swagger 扩展） | 内置 OpenAPI 文档       |
| 类型提示     | 不强制                  | 强依赖类型提示          |
| 适用         | 传统网页、小型后台      | 高性能 API、AI 服务接口 |

> 如果你要做 AI 后端接口，现在很多新项目优先 FastAPI；简单后台、小型工具服务 Flask 依然非常好用。



## 7、部署注意

- app.run() 只用于<font color="red">**开发调试**</font>，生产环境不能用！ 
- 生产部署搭配：<font color="red">**gunicorn + nginx**</font>



# 二、快速开始

## 1、入门应用

- 导入依赖：pip install flask
- 写代码

~~~python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
~~~

- 代码解释
  - 首先我们导入了 Flask 类。该类的实例将会成为我们的 WSGI 应用。
  - 接着创建一个该类的实例。第一个参数是应用模块或者包的名称。 `__name__` 是一个适用于大多数情况的快捷方式。有了这个参数， Flask 才能知道在哪里可以找到模板和静态文件等东西。
  - 然后我们使用 route() 装饰器来告诉 Flask 触发函 数的 URL 。
  - 函数返回需要在用户浏览器中显示的信息。默认的内容类型是 HTML ，因此 字符串中的 HTML 会被浏览器渲染

- 把它保存为 hello.py 或其他类似名称。请不要使用 flask.py 作为应用名称，这会与 Flask 本身发生冲突。
- 可以使用 flask 命令或者 python -m flask 来运行这个应用。你需要使用 --app 选项告诉 Flask 哪里可以找到应用
- flask --app 文件名 run 

~~~bash
(FlaskDemo) PS D:\pythonProject\FlaskDemo> flask --app test run 
 * Serving Flask app 'test'
 * Debug mode: off
~~~

> 作为一个捷径，如果文件名为 `app.py` 或者 `wsgi.py` ，那么您不 需要使用 `--app` 。详见 [命令行接口](https://dormousehole.readthedocs.io/en/latest/cli.html) 

- 这样就启动了一个非常简单的内建的服务器。现在在浏览器中打开 http://127.0.0.1:5000/ ，应该可以看到 Hello World! 字样。



## 2、外部可见的服务器

- 运行服务器后，会发现只有您自己的电脑可以使用服务，而网络中的其他电脑却不行。缺省设置就是这样的，因为在调试模式下该应用的用户可以执行 您电脑中的任意 Python 代码。

- 如果您关闭了调试器或信任您网络中的用户，那么可以让服务器被公开访问。 只要在命令行上简单的加上 `--host=0.0.0.0` 即可:

```
$ flask run --host=0.0.0.0
```

- 这行代码告诉您的操作系统监听所有公开的 IP



## 3、调试模式

- flask run 命令不只可以启动开发服务器。如果您打开调试模式，那么服 务器会在修改应用代码之后自动重启，并且当请求过程中发生错误时还会在浏 览器中提供一个交互调试器。

- 调试器允许执行来自浏览器的任意 Python 代码。虽然它由一个 pin 保护， 但仍然存在巨大安全风险。不要在生产环境中运行开发服务器或调试器。

- 如果要打开调试模式，请使用 `--debug` 选项。

```bash
$ flask --app hello run --debug
 * Serving Flask app 'hello'
 * Debug mode: on
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: nnn-nnn-nnn
```



## 4、HTML 转义

- 当返回 HTML （ Flask 中的默认响应类型）时，为了防止注入攻击，所有用户 提供的值在输出渲染前必须被转义。使用 Jinja （这个稍后会介绍）渲染的 HTML 模板会自动执行此操作。

- 在下面展示的 escape() 可以手动转义。因为保持简洁的 原因，在多数示例中它被省略了，但您应该始终留心处理不可信的数据。

```python
from markupsafe import escape

@app.route("/<name>")
def hello(name):
    return f"Hello, {escape(name)}!"
```

- 如果一个用户想要提交其名称为 `<script>alert("bad")</script>` ，那么 宁可转义为文本，也好过在浏览器中执行脚本。

- 路由中的 `<name>` 从 URL 中捕获值并将其传递给视图函数。这些变量规则 见下文。









































https://dormousehole.readthedocs.io/en/latest/cli.html