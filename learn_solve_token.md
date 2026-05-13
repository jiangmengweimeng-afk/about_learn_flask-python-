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