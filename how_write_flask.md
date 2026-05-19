我们想写u一个结构完整且健康的最基本的简单框架 我们从如下的结构由简入深的考虑：
1. 首先我们要确定构建项目的方向
2. 我们要确定项目的结构
3. 可以从最基本的app.py 开始 一步一步完善代码的功能
4. 知道什么时候添加数据库、表单等 而且要知道添加到哪里：
        1: 数据库模型（User, Post），一般都是放在 app/models.py
        2: 表单类（LoginForm, PostForm），一般都是在 app/forms.py
        3: 处理博客文章的路由，app/views/main.py 或者是新建的 app/views/posts.py 并且注册蓝图
        4： run.py（启动命令）
            config.py （配置变量）
            app/__init__.py （创建app、初始化扩展、注册蓝图）
            app/models.py （数据库表结构）
            app/forms.py （前端表单类）
            app/views.py（路由和业务逻辑）
            app/templates/ （HTML页面）
            app/static/ （CSS\Js\图片）

**整个Flask后端逻辑（路由、视图函数、数据库模型、表单类、配置、应用工厂）全部由python编写 只有前端暂时部分（HTML\CSS\JS）不是python

*蓝图是如何注册的呢？
        1: 首先规划我们的项目目录结构，分出来认证模块，博客模块，主模块
        2： 创建蓝图实例，这时候我们要导入需要的模块和包（from flask import Blueprint, render_template, redirect, url_for）
        3： 之后创建蓝图实例

理解核心原则，flask本身不强制规定结构，并且中型以上的项目会再用blueprint来模块化 下面是flask标准的结构：
        my_flask_app/
        ├── app/                     # 主应用目录
        │   ├── __init__.py          # 创建 Flask 实例，初始化扩展
        │   ├── models.py            # 数据库模型（类）
        │   ├── forms.py             # 表单类（如果用 WTForms）
        │   ├── views/               # 视图函数（路由）
        │   │   ├── __init__.py
        │   │   ├── main.py          # 主页面相关路由
        │   │   └── auth.py          # 登录/注册相关路由
        │   ├── templates/           # HTML 模板
        │   └── static/              # CSS/JS/图片
        ├── config.py                # 配置类（数据库地址、密钥等）
        ├── requirements.txt         # 依赖包列表
        └── run.py                   # 项目启动入口

在python中制造轮子指的是开发者自己重复实现了一个已经存在的并且成熟的解决方案或者库的功能， 而不是直接使用现成的工具。下面是常见的不同扩展的导入方式对比：
Flask-JWT-Extended  pip install flask-jwt-extended   from flask_jwt_extended import JWTManager, create_access_token, jwt_required
Flask-Login  pip install flask-login  from flask_login import LoginManager, login_user, login_required, current_user
Flask-SQLAlchemy  pip install flask-sqlalchemy  from flask_sqlalchemy import SQLAlchemy
Flask-WTF  pip install flask-wtf  from flask_wtf import FlaskForm