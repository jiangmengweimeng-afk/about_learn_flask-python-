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


在python中flask的构建中用raise 返回一个结果和return的区别是什么 并且raise是在主动抛出一个错误的时候使用的吗 我这样说正确吗 请你详细且细致的解答我的问题 并且dayload = jwt.decode(
            token,
            current_app.config['SECRET_KEY'],
            algorithms=['HS256'],
            options={
                'verify_exp': True
            }  像如示的代码中 为什么有的=相距是没有间隔的 他们的区别分别是什么呢:
    * 在正常的Flask视图函数中不可以直接i用raise返回一个成功的结果， raise专门用于抛出异常，而return用户返回正常响应。

    * 特性	return	        raise
        用途	返回正常响应	抛出异常（错误）
        执行流程	正常结束视图函数	中断当前流程，进入异常处理
        HTTP 状态码	默认 200，可自定义	通常对应 4xx/5xx
        能否被捕获	否	可以（通过 @app.errorhandler）
        后续代码	return 后的代码不执行	raise 后的代码不执行，但可能被 except 或错误处理器接管

而我说“raise是在主动抛出一个错误的时候使用的” 这样的说法如同我平时说的一样 是正确的 但是不专业也不精确，可以更好的这样说：
    raise用户主动触发异常（Exception），不一定是错误 还有异常情况。在python中异常可以分为：内置异常（ValueError、TypeError、KeyError），自定义异常（继承 Exception 类），HTTP异常（Flask中的HTTP Exception）

等号位置	是否有空格	示例	原因
变量赋值	✅ 两边有空格	name = "张三"	区分赋值与比较
关键字参数	❌ 没有空格	func(param=value)	避免视觉混乱
函数默认值	❌ 没有空格	def func(x=1):	同上
字典键值对	✅ 两边有空格	{"key": value}	提高可读性
装饰器参数	❌ 没有空格	@app.route('/')	无歧义
类型注解	✅ 两边有空格	name: str = "张三"	PEP 8 建议

而这个 = 是否有间隔是遵循PEP8 规范，有这个区别的原因是因为赋值等号（=）是操作符，需要空格来增强可读性。关键只参数等号不是操作符，而是语法标记，去掉空格更紧凑避免与变量赋值混淆。

return {
            "user_id": payload.get("sub"),
            "username": payload.get("username")
        } 
        如上所示的代码 为什么"user_id": payload.get('sub') 为什么不是user_id, 而是sub是因为在python的JWT里面Sub代表该JWT所面向的用户