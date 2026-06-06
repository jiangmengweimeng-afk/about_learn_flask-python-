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

### 如果遇到当前运行代码的python环境与安装的模块环境不一致
- 1.1 使用 python -m pip 确保安装到当前运行的python环境 如果是python3 可以 python3 -m pip install pandas
- 1.2 验证安装的模块是否真的安装在当前环境中，在当前终端中进入python交互模式 
  - import pandas as pd
  - print(__version__)
- 输出版本号就是安装成功 反之 没有成功
- 1.3 切换 vs code 的解释器
  - 在界面中按快捷键 ctrl + shift + p
  - 输入 Python: Select Interpreter 选择与 vc code 右下角状态栏显示的 version 一样
  - 在弹出的列表中 寻找带有 VmessAction 字样的选项
  - 删除当前的终端 运行时打开一个新的终端 重新运行代码

### 为什么会总是遇到上面的问题呢 
- 我使用 git 命令安装的时候没有将流畅逐步的进行完毕
  - git clone <仓库地址> 下载源码到文件夹
  - cd <文件夹名> 
  - python setup.py install 这一步真正把模块注册到python环境里面 我只进行了下载代码 没有执行安装脚本
## 解决方案
### 重新执行安装命令
- 1. 打开终端，确保我的 vscode 终端已经激活了我之前选好的 VmessAction 环境，这一步时确保终端激活了正确的python环境，装到正确的地方
- 2.进入目录使用cd 命令进图我的git clone 下来的那个文件夹，这是为了运行项目代码 这一步解决的时运行时候可以找到项目文件
- 3.执行安装运行命令 pip install / 老项目 python setup.py install，这一步解决的是把代码变成可以导入的库
  ![alt text](imgs/common_mistakes.image-2.png)

## pip
- 一定要注意环境一致性，不管是在Windows命令提示符cmd里输入命令还是vscode或者git bash都要注意环境一致性
### 一致性解决办法
![alt text](imgs/common_mistakes.image-6.png)
- 在vscode里面按 Ctrl + ` 打开终端，不要着急执行命令，看一眼命令提示符，如果是(env) or (.venv)开头说明我是在虚拟环境里面。为了保险起见，直接在我写代码的(.py)文件标签上右键，选择"在终端中运行python文件"。之后把命令修改一下，把python xxx.py 替换成 pip intall mplfinance 再回车键执行，这样就可以保证百分之百装对了地方
  ![alt text](imgs/common_mistakes.image-3.png)
  ![alt text](imgs/common_mistakes.image-4.png)
- 关于安装是否成功输入： import mplfinance as mpf print(mpf.__version__)  如果可以打印出版本号，就是安装成功了
- python -m pip install mplfinance 更推荐这个安装方式，python -m pip 可以明确地告诉python解释器，去找属于你的pip 工具来安装。这样的好处是可以避免我的电脑里有多个python版本时候，pip命令装到了错误的环境里面。
  ![alt text](imgs/common_mistakes.image-5.png)