import markdown
Token本身是无状态的 
token是通过Refresh Token刷新token的新功能

为了使装饰器在多个文件中使用 同事保持项目结构清晰 应该封装为公共模块

你的错误是 TypeError: Expected a string value:
首先确认在运行服务器的终端中 环境变量是否设置 解决方法如下：1.在Git Bash中切换到所在的项目目录 然后设置环境变量 之后运行服务器；2. 在Powershell终端中 设置和1的步骤一样；3. 在代码中提供默认值 但是只用于开发环境 生产

如果vscode变成了全面屏模式  按F11就可以了


如果在获取token的过程中 我们已经注册过新用户并且存在之前登录成功并且返回了token 但是我们删除了旧的 powershell 我们可以进行一下操作获取已经存在的token：
1. 重启服务器 或者是Ctrl + c 终止服务器 
2.设置环境变量在 powershell 中：$env:JWT_SECRET_KEY="my_test_secret_key_12345"
在 Git Bash 中设置环境变量：export JWT_SECRET_KEY="my_test_secret_key_12345"
3.重新启动服务器
4.重新登录获取新的token 打开第二个powershell or Git Bash


如果我们想测试token 可以使用交互模式 具体操作如下：
import requests
token = "your_token"
print("token长度:", len(token))
headers = {"Authorization": f"Bearer {token}"}
r = requests.get("你的flask服务器的地址和项目目录", headers=headers)
print("状态码:", r.status_code)
print("响应:", r.text)

交互模式的优势如下：不需要处理shell转义；没有引号嵌套问题；可以分布调试；错误信息更清晰明显。


掌握token需要了解以下的功能：
1.token的生成和验证
2.用户认证（注册和登录接口）
3.权限控制（基于角色的访问控制）
4.安全模块：密码和哈希存储
            JWT Token 生成
            JWT Token 验证装饰器
            受保护路由
5.运维：日志系统
        配置环境区分
6.数据库：SQLALchemy集成 


添加新的token机制：
                1.Access Token:
                2.Refresh Token:



什么是cookie: 就是相当于一个记录用户状态的笔记 专业的来讲就是服务器存在我的浏览器里的一小段数据，用来挤住用户状态

cookie: 
        1.本质就是类似于字典 键值对 比如我想获取一个cookie 我就可以 request.cookie.get("key", " ")  这个Key就是一个占位符， 用来表示我想获取的cookie的键名。" " 是默认值 如果cookie不存在 就会返回这个默认值空字符串
        2.创建响应对象: resp = make_cookie("xxxx")
        3.设置cookie和有效时间: resp.set_cookie("xx", "flask", max_age=3600)
        4.删除cookie: resp.del_cookie('xxx')
        5.获取所有的cookie: all_cookie = request.cookie
        6.获取指定key的cookie：cookie_value = request.cookies.get("key", " ")

什么是session：就是服务器的一个存储机制 用来存储用户的状态信息 服务器会为每个用户分配一个唯一的session ID 这个ID会存储在cookie里面 当用户发出请求时 会携带这个session ID服务器会通过这个ID来识别用户的状态， 从而来实现用户认证和权限控制等功能


from functools import wraps  如例所示 这行代码从顶端导入和从函数内部导入的区别：
    顶端导入：
            作用域是全局 更符合PEP8 的书写规范 而且从顶端导入避免了每次装饰器运行时候的重复导入查找，并且无循环依赖风险 好处多多 这样模块加载时间更短 效率更高
    内部导入：
            作用域是函数内部 外部看不到，并且也不知道他的存在效率较低 开销非常小 局部导入最常见且合理的的用途之一就是循环依赖。


📝 algorithm='HS256' 和 algorithms=["HS256"] 的区别
    encode函数
                参数名是单数algorithm 只需要要给字符串 表示“我要用哪种算法来签名”
    decode函数
                参数名是负数algorithms 表示“允许接受哪些算法签名的token” 这是为了安全防止攻击者修改token的头部算法字段
    一句话来说就是生成时用单数字符串，验证时候用负数列表


primary_key: 主键
unique=True: 确保系统中没有重名的用户 保证是独一无二的 没有重复
nullable=False: 强制要求注册时必须提供用户名

存储方式	能否被 JS 读取	防 XSS 攻击	防 CSRF 攻击	推荐场景
HttpOnly Cookie	❌ 不能	✅ 能（JS 读不到）	⚠️ 需要额外防护	绝大多数场景（Refresh Token 首选）
LocalStorage	✅ 能	❌ 不能（XSS 直接偷）	✅ 天然防护	只有前端 SPA 且 XSS 防护极强时才
用


时间有效期 是一个为1.{"sub": username, "exp": datetime.utcnow() + timedelta(minutes=15)}, 另一个是2.{"sub": username, "exp": datetime.utcnow() + timedelta(days=7)}, 这是为什么呢 我需要你的详细且细致的解释:
                1.: 生成的令牌中 exp 字段记录了当前时间往后推15分钟的时间戳 一旦当前服务器时间超过这个时间点 任何校验该令牌的接口就都会直接拒绝请求(报错Token Expired)
                2.: 这意味着这个令牌的有效期长达7天 在这7天内 它被允许用来刷新身份

            *这样的设计是出于双令牌机制的安全行考虑 降低被盗用的风险、保证用户体验、最小化攻击面


如何设置httponly cookie的参数\属性:
    key='refresh_token'(长期令牌)
    value=refresh_token(生产环境必须开启https)
    samesite='Lax'(防CSRF攻击的重要配置) 这是一个跨站请求控制属性
    max_age=7*24*60*60(有效期要和refresh_token保持一致)

    通常Access Token 是放在HTTP响应体（JSON）里给前端用的 而Refresh Token 才是放在Cookie里用来续期的


is＿revoked: 是Flask-JWT/Flask-JWT-Extended扩展里的核心函数， 专门用来判断JWT令牌（Token）是否已经被吊销或者拉黑 简单的来说就是校验当前请求携带的Token，是不是已经被用户主动注销或者强制失效了
核心作用：假如是有过期时间到期自动失效，或者是用户主动登出、管理员强制踢人、账号被盗需要立即封禁（立即失效所有关联的Token），就需要让Token提前失效 is_revoked 就是这个作用  返回True = 已吊销（拒绝访问），False = 有效（允许访问）
函数名可以自定义 但是必须用 @jwt.token_in_blocklist_loader 装饰


关于cookie的samesite的参数：
    Lax: 允许在用固话点击链接跳转时发送cookie,但是跨站POST请求或图片加载时不发送，这对大多数登录场景（包括前后端分离）都很友好
    None：如果发现前端跨域请求时cookie总是传不过去，且确定用了HTTPS（生产环境）那时候才需要改成None'
关于cookie 的 secure 参数：
如果是本地开发（HTTP）暂时改为 secure=False
如果是生产环境（HTTPS）保持是 secure=True 如果是True会导致本地cookie无法保存 如果要测试的话一定记得给False  不然在生产环境下回直接忽略cookie
更进阶的写法是 secure=current_app.config.get('ENV') == 'production'

app.run(debug=True)不能在生产环境使用，会暴漏敏感信息， 而且Flask自带的app.run()也不适合生产部署,建议使用 run.app(debug=app.config.get('DEBUG', False))

如果要测试cookie 假如忘记了密码或者用户 我们可以用 res = requests.post("http://localhost:5000/access_record/register", json={"username": "testuser", "password": "1234567"}) printS(res.status_code, res.json())  只要返回是201 就是代表注册新用户成功了

登录并获取cookie:   s = requests.Session()
                    res  = s.post("http://localhost:5000/access_record/login/password", json={"username": "testuser", "password": "1234567"})
                    print("状态码:", res.status_code)
                    print("返回内容:", res.json())
                    print("服务器返回的 cookie:", s.cookie.get_dict())

如果我们可以注册成功新的用户和密码之后一直还是测试不了cookie 并且报错TypeError: Expected a string value
File "jwt/utils.py", line 22, in force_bytes
  raise TypeError("Expected a string value")  就是证明我的JWT SECRET_KEY是空的 不是字符串 所以导致jwt.encode()直接崩溃服务器返回500，在这样的情况下我们可以对config.py 或者是 app.py 文件里面的 config 进行生产环境上的配置 顾名思义就是给他一个默认的值来对cookie 进行测试。
  解决方案：给Flask配置一个固定的 SECRET_KEY 我们可以在app = Flask(__name__)后面加上这行，给一个固定的密钥（开发环境用）app.config['SECRET_KEY'] = 'dev-secret-key-for-testing-only'
  如例：>>> import requests
>>> s = requests.Session()
>>> login_res = s.post("http://localhost:5000/access_record/login/password", json={"username": "testuser", "password": "1234567"})
>>> print("登录状态:", login_res.status_code)
登录状态: 200
>>> print("登录返回:", login_res.json())
登录返回: {'access_token': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjozLCJleHAiOjE3Nzg5MTk2MTgsInR5cGUiOiJhY2Nlc3MifQ.zUoSD-0l_9cWSFATlIocR62XD7gNhz_yygerpvQHYqQ'}
>>> print("服务器返回的 Cookie:", s.cookies.get_dict())
服务器返回的 Cookie: {'refresh_token': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjozLCJleHAiOjE3Nzk1MjM1MTgsInR5cGUiOiJyZWZyZXNoIn0.HNWbAIB-sJSCsvYPncGQ2vlHuU4QbYAIjfM6hgpCLCk'}   这就是测试成功的样子

而且我们正确的配置顺序是：先加载 config.py 里的配置 env = os.environ.get('FLASK_ENV', 'development')
app.config.from_object(config[env])

# 再手动设置开发环境的默认密钥，覆盖 config.py 里的空值
if env == 'development':
    app.config['SECRET_KEY'] = 'dev-secret-key-for-testing-onlyS'

    开发环境下 手动配置不会被覆盖， 生产环境下，还是会用 config.py里面的配置不会被覆盖。

    此外，我们必须保证我们的config.py 里面确保 development环境里有 SECRET_KEY = 'dev-secret-key-from-config' 这必须有值 如果这里是空的或者写的是 os.environ.get('SECRET_KEY')但没设置环境变量，就会变成None，导致报错

如果 jwt.encode()必须传入字符串类型的密钥，传None就会直接抛出TypeError: Expected a string value

路由直接挂下根目录下面是不规范的 我应该使用更规范的写法RESTful风格
    用户相关：/api/user/login, /api/user/register
    业务相关：/api/records/list, /api/records/detail


url = 域名 + 注册蓝图的前缀 + 定义蓝图的前缀 + 函数路由

allow_expired=True表示即使refresh_token过期了 只要签名时也可以解出数据

get_jwt()用户获取当前令牌的完整payload，包括jti,exp等，get_jwt_identify()只获取用户标识
JWT只是编码不是加密 所以不要在JWT的payload里面存储敏感信息密码、身份信息等


如果运行一个文件时候出现如下所示的报错，那么我们可以考虑这个虚拟环境和安装包的python的版本不同
Traceback (most recent call last):
  File "app01.py", line 10, in <module>
    from access_record.views import access_record
  File "C:\Users\GK\Desktop\work\learn\app\access_record\views.py", line 2, in <module>
    from flask_jwt_extended import decode_token, create_access_token, create_refresh_toen
ModuleNotFoundError: No module named 'flask_jwt_extended'
如果使用了虚拟环境的话 我们要在vscode的右下角python那里选择带有 ./venv/...或者是 ./.venv/.. 路径的那个 这里既然提到了虚拟环境我们可以考虑如何几乎哦虚拟环境的命令在windows里面：
venv/Scripts/activate （激活） deactivate （退出虚拟环境）

venv 标记：vscode 会自动识别项目中的虚拟环境，并打上 venv 标签。这通常意味着我的项目依赖是安装在带有venv标签的独立环境中而不是在全局环境里面

我们可以在所选的虚拟环境的终端下运行 pip list 命令查看是否有我们需要的 库 如果没有我们可以执行 pip install flask flask-jwt-extended 命令 pip list 可以查看到我们需要的库就是成功

