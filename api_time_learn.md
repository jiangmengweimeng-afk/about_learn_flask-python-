### 一、什么是“时间记录API

在软件开发中，“时间 记录API”通常指**用于获取、处理、格式化、计算时间数据的接口**
- 1.系统获取时间
- 2.时间计算与格式化
- 3.时间记录/日志，在业务流程中记录关键时间点
- 4.定时任务/超时控制,等待多久、超时判断

---

### echo 

`echo=True:` 所有执行的SQL语句、参数、执行时间都会打印到控制台，相当于print。`echo` 控制**SQL ALchemy引擎是否将生成的SQL语句输出到日志（标准输出）。** 
- **开发调试和阶段非常有用**，可以实时查看SQL是否正确、是否有N+ 查询等问题。
- **生产环境：**用`echo=False`,因为会打印大量日志拖慢性能；并且日志中可能包含敏感数据，比如插入的用户信息等。

`echo:`最终底层是设置python的`logging`级别, `echo=True`相当于将SQLAlchemgy引擎的日志级别设置为`logging.INFO,` `echo=False`则为`logging.WARNING`

### session

在python异步代码中，`session`参数通常指代**客户端会话 Client Session，** 就是一个连接池和状态管理器，它的核心意义在于**复用** 和 **控制。**

- **资源服用与性能优化**
- **共享全局配置（Cookie与Headers）：** 传入`session`的意义可以在创建`session`时候设置全局的`headers`(User-Agent、Token)或`cookies。` 当将这个`session`传给后续的多个请求函数时，这些配置会自动带在每一个请求上，无需再每个函数里重复编写认证Header。
- **精细化资源控制**

### expire_on_commit=False/True

- 默认为`True:` 每次`commit()`后，会话中的所有对象属性都会被“过期”， 下次访问时会自动从数据库重新加载。
- `False:` 提交后不回过期对象，保持当前内存中的状态，适合读多写少或者缓存场景。
- 如果写错会报错如图所示的代码：`TypeError: async_sessionmaker() got an unexpected keyword argument 'expird_on_commit'`



