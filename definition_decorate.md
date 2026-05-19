*定义一个装饰器就是写一个函数，在里面封装我想要的功能。任何装饰器都遵循三层结构 从里到外（接受被装饰的函数、接受原函数的参数、调用原函数、返回wrapper替换原函数）。装饰器的本质就是一个接受函数，一个返回函数的函数
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"正在执行函数: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"函数执行完毕")
        return result  # 返回新的函数
    return wrapper 

@logger  # 这个装饰器的命名就是定义装饰器时用的函数的名字
def say_hello(name):  # 传入参数 name 
    print(f"hello {name}")

say_hello("张三") # 调用name，并且传入参数

入股我想定义自己选哟的装饰器，但不知道如何下手可以从一下几个方面考虑：
    1: 需要清楚自己想增强的功能
    2: 写出最内层的wrapper
        def wraper(*args, **kwargs):
            print("begin")
            result = func(..)
            print("end")
            return return
    3: 加上外层框架：
        def decorator_name(func):
            def wrapper(*args, **kwargs):
                return func(*args, **kwargs)
            return wrapper
    4: 如果需要传参我就需要再加一层函数：
        def decorator_name(param):
            def outer_wrapper(func):
                def wrapper(*args, **kwargs):
                    return func(*args, **kwargs)
                return wrapper
            return outer_wrapper

常见的应用场景装饰器的作用：
    日志记录 打印函数调用信息
    性能计时 统计执行时间
    重试机制 失败后自动重试
    权限验证 检查用户权限
    缓存结果 避免重复计算
    事务管理 数据库自动提交/roll back

装饰器的核心结构就是:def decorator(func):
                        def wrapper(*args, **kwargs):
                        return wrapper # 必须返回wraper,否则原函数会被替换为None
用@wraps保留被装饰器函数的元信息，参数传递用 *args, **kwargs才能接收任意参数

