### error
- RefreshToken.query.filter_by(token=cookies_token).first
- 如上所示，如果 first 没有加 () 的话 会发生InvalidRequestError: Object '<function ...>' is not a persistent instance 的报错 
- 原因: 获取到的是这个方法本身的内存地址，而不是查询出来的数据库对象。在python中，first是一个方法

### 遇到python中的测试问题遇到的困难
![alt text](imgs/common_mistakes.image.png)
- 原因分析：你尝试访问 http://127.0.0.1:5000/api/v1/login，但服务器返回了 404。这意味着你的 Flask 后端程序里没有定义这个路径，或者路径写错了。
- 排查步骤：检查定义路由，看的是不是我们app.py 定义的路由
   确认路径：看的是我们定义路由时候的 ('/login')
   查看启动日志: 看有没有打印出所有的路由列表，如果没有显示说明代码里根本没写或者没有加载进来
- SyntaxError: invalid syntax
- 原因：这是你在最后一条命令中遇到的错误。这是因为你在终端里直接粘贴了一大段包含换行符的代码，而 Windows 的命令行（CMD/PowerShell）不支持直接粘贴多行代码块。它把换行符当成了结束符，导致后面的代码变成了无法识别的乱码
- 解决办法：我们需要把多行代码压缩成一行，或者使用分号 ; 连接
  ![alt text](imgs/common_mistakes.image-1.png)
