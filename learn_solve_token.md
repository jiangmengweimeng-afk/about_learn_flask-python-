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