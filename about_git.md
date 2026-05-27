### Git 命令行
- 关于Git命令行的推送 更行... 老忘记怎么办 记得少 练得少
- git add . 添加所有的修改和新增的文件 包括为最终的文件
- git commit -m 提交并且修改信息
- git remote -v 检查是否关联成功
- git remote add origin htttp://gitbug.com/jiangmengweimeng-afk/目标文件.git  添加到远程仓库
- git push -u origin main 把本地 mian 分支推送到远程origin的mian分支
- git pull origin mian  从远程origin的mian分支拉取最新的代码到本地
- git reomote remove origin 用于删除之前已经关联仓库执行的命令

- cd .. 其中.. 是退出到上一个目录 一个 . 代表一个级别的目录

- ls 查看当前仓库里的文件

- git log 看提交了什么
- git push 把提交推到github

- 如何查看Git命令提交成功呢：
    1.查看最新提交记录：git log -1 显示最后一次提交的信息（提交人、时间、哈希、备注）
    2.简洁版提交日志: git log --oneling （一行显示一条提交，清晰看到所有本地提交记录可以看到刚写的提交备注就是成功的）
    3.直接查看远程仓库提交记录: git log origin/main --oneline（可以看到刚推送的提交就是提交成功）

- 错误信息分别都代表了什么意思：
                            1.fatal: unable to access 'https://github.com/jiangmengweimeng-afk/login_.git/': OpenSSL SSL_read: Connection was reset, errno 10054
                            (这表示是网路层面的错误 git 尝试链接git Hub服务器时候 别强制断开了链接 提交的内容只存在于本地)
                            2.errno 10054 表示是windows系统的的网络错误就是连接被对端重置

- 遇见以上或者类似的问题时候解决办法就是：重新尝试命令git push -u origin main 如果成功就是临时网路问题
- 或者 检测仓库地址和权限：git remote -v 假如地址不对可以重新设置地址：git remote set-url origin xxxxxx

- 我已经在vscode里修改了文件 需要把新的内容也更新到Git仓库 是这样操作的：
    git add .
    git commit -m ""
    git push
### 遇到的情况   
![alt text](imgs/about_git.image-1.png)
- 遇到如图所示的情况是Git调用的分页器 如何退出呢？
- 在当前界面 直接按下键盘上的小写 q 就可以了 如果不想进入分页器 可以执行 git add xxx.md 这样的命令行格式就可以了
- 如果不小心进入了 vim 编辑器，退出办法是：先 Esc 键，然后 :q (不保存不退出) 或者 :wq (保存并退出)， 再按退出