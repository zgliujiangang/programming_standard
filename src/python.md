# Programming standard for python
## 编码风格
    1.类名单词首字母均大写，如果单词为缩写则此单词应全部大写
```python
    class base_cache:
        # no
        pass

    class BaseCache:
        # yes
        pass
```
    2.类私有成员及私有方法以下划线开头，单词小写，单词与单词之间下划线分隔
```python
    class BaseCache:
        # yes
        def make_key(self, *args, **kwargs):
            pass

        def get(self, key):
            pass

        def set(self, key, value, ttl):
            pass

    class BaseCache:
        # no
        def _make_key(self, *args, **kwargs):
            pass

        def get(self, key):
            pass

        def set(self, key, value, ttl):
            pass
```
    3.局部变量以及函数定义，单词均小写，单词与单词之间下划线分隔，
```python
    def getmessagefromwechat():
        # no
        pass

    def get_message_from_wechat():
        # yes
        pass
```
    4.常量单词均大写，单词与单词之间以下划线分隔
```python
    http_methods = ('get', 'post', 'put', 'delete', 'head', 'options', 'trace', 'connect')  # no
    HTTP_METHODS = ('get', 'post', 'put', 'delete', 'head', 'options', 'trace', 'connect')  # yes
```
    5.避免出现难以理解的数值，枚举常量定义在作用域顶部，如：
```python
    # no
    users.filter(sex=1)

    # yes
    MALE = 1
    users.filter(sex=MALE)
```
    6.变量应尽量在第一次使用前声明赋值，如：
```python
    # no
    car = Car()
    do_something()
    car.run()

    # yes
    do_something()
    car = Car()
    car.run()
```
    7.变量名应见名思意，计数变量可使用单个字母如i、j
```python
    # no
    for item in cats:
        pass

    # yes
    for cat in cats:
        pass
```
    8.变量声明时应避免污染全局作用域
```python
    # no
    car = Car()
    road = Road()
    def run():
        car.run_at(road)
    run()

    # yes
    def run():
        car = Car()
        road = Road()
        car.run_at(road)
    run()
```
    9.使用隐式的boolean
```python
    # no
    if foo != []:
    if foo == True:

    # yes
    if foo:
```
    10.整数变量不与布尔值比较
```python
    x = 1
    # no
    if x == False:

    # yes
    if x == 0:
```
    11.类如果不继承自其它类就应显示的从object继承，新式类的继承使用c3算法，旧式类深度优先
```python
    class MyClass:  # no

    class MyClass(object):  # yes
```
    12.可执行脚本被导入时也不应被执行
```python
    # no
    main()

    # yes
    if __name__ == '__main__':
        main()
```
    13.if条件过多，看是否可以转换成表驱动法
```python
    # no
    if x = 0:
        y = '星期一'
    elif x = 1:
        y = '星期二'
        ...
    else:
        raise
    # yes
    weekdays = ['星期一', '星期二', '星期三', ...]
    if 0 <= y <= 6
        y = weekdays[x]
    else:
        raise
```
    14.默认参数应为不可变类型
```python
    # no
    def f(a, L=[]):
        L.append(a)
        return L

    # yes
    def f(a, L=None):
        L = L or []
        L.append(a)
        return L
```
    15.代码执行到异常情况时应有日志记录，以便追踪错误根源
```python
    # no
    try:
        json.loads(data)
    except (ValueError, TypeError):
        pass

    # yes
    try:
        json.loads(data)
    except (ValueError, TypeError):
        logger.warning('json loads data value error type error: %s', data)
        pass
```
    16.多进程记录日志时应注意是否对同一个日志文件进行写入操作
    17.统一日志格式
```python 
    format = ('%(levelname)s %(asctime)s %(module)s '
             '%(pathname)s [%(lineno)d]: %(message)s')
```
    18.简单的情况下建议使用列表推导式
```python
    # no
    result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

    # yes
    squares = [x * x for x in range(10)]
```
    19.单行函数建议使用lambda
```python
    # no
    def sum(x, y):
        return x + y

    # yes
    sum = lambda x, y: x + y
```
    20.简短函数建议使用条件表达式
```python
    # no
    if condition:
        x = 1
    else:
        x = 2

    # yes
    x = 1 if condition else 2
```
## 布局风格
    1.所有模块均以`# -*- coding: utf-8 -*-`开头
    2.子语句块缩进4个空格而非1个tab
    3.一行语句超过100个字符必须换行
    4.顶级定义之间空两行
```python
    class MyClass(object):
        pass
        
        
    def my_function():
        pass
```
    5.类子方法之间空一行
```python
    class MyClass(object):
        
        def __init__(self):
            pass
        
        def func(self):
            pass
```
    6.方法内分组的语句块之间建议空一行
```python
    def my_function():
        dog = Dog()
        dog.bark()
        dog.run()

        sheep = Sheep()
        sheep.bleat()
        sheep.run()
```
    7.字符串换行：
```python
    logger.warning('aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'
                   'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa')
```
    8.参数换行：
```python
    urllib2.urlopen('http://www.google.com',
                    data='aaaaaaaaaaaaaaaaaaaaa',
                    headers={},
                    cookies={})
```
    9.不要使用分号将两句代码放在一行
```python
    # no
    reload(sys); sys.path.append('.')

    # yes
    reload(sys)
    sys.path.append('.')
```
    10.在模块顶部import。每个import占一行， 第三方模块在上，内部模块在下，之间空一行
```python
    # no
    import time, django
    import my_package

    # yes
    import time
    import django

    import my_package
```
    11.单个py文件过大应考虑拆分
## 模块导入
    1.对一个模块导入超过3个以上的对象时，请直接导入该模块
```python
    # no
    from my_package.my_module import a, b, c, d  

    # yes
    from my_package import my_module
    my_module.a
    my_module.b
    my_module.c
    my_module.d
```
    2.按包的全路径导入模块。下面两种方式导入的模块有不同的内存空间
```python
    # no
    import my_module

    # yes
    from my_package import my_module
    from . import my_module
```
    3.模块间尽量避免交叉导入
## 资源释放
    1.文件或socket操作完成时应显示关闭，或者放在上下文中操作，如果对象不支持with，但支持close，使用contextlib.closing()
```python
    f = open('read.md')  # no

    with open('read.md') as f:  # yes
    with contextlib.closing(open('read.md')) as f:  # yes
```
    2.对象交叉引用时一方可使用弱引用weakref，防止发生内存泄露
```python
    # no
    class A(object):

        def __init__(self, b):
            self._b = b

    class B(object):

        def __init__(self, *args, **kwargs):
            self._a = A(self)


    # yes
    import weakref

    class A(object):

        def __init__(self, b):
            self._b = b

    class B(object):

        def __init__(self, a):
            self._a = A(weakref.proxy(self))
```
    3.可迭代对象数据量较大时请使用生成器
```python
    # no for python2
    for in in range(100000):
        pass

    # yes
    for i in xrange(100000):
        pass
```
## 性能
    1.python2中起无限循环时使用while 1
```python
    # no
    while True:
        pass

    # yes
    while 1:
        pass
```
    2.字符串连接时看情况操作
```python
    x = '%s%s' % (a, b)  # no
    x = a + b  # yes
    x = 'hello,' + a + ' ' + 'b' + ' ' + 'world!'  # no
    x = 'hellp,%s %s world!' % (a, b)  # yes
```
## 防御式编程
    1.检验用户数据是否正确
```python
    # no
    def devision(n):
        return 10 / n

    # yes
    def devision(n):
        if type(n) == int:
            if n == 0:
                    raise ValueError
                return 10 / n
        else:
            raise TypeError
```
## 错误处理
    1.应考虑代码可能的出错情况，进行异常捕获
```python
    # no
    dog = dogs[0]

    # yes
    try:
        dog = dogs[0]
    except IndexError:
        dog = Dog()
```
    2.需进行异常捕获的代码越短越好
```python
    # no
    try:
        dog = dogs[0]
        dog.run()
    except IndexError:
        logger.warning('Dogs index error')
        raise

    # yes
    try:
        dog = dogs[0]
    except IndexError:
        logger.warning('Dog index error')
        raise
    dog.run()
```
    3.避免对任何异常都进行捕获的代码，因为很有可能隐藏真正的bug
```python
    # no
    try:
        data = json.loads(data)
    except Exception as e:
        pass

    # yes
    try:
        data = json.loads(data)
    except (ValueError, TypeError):
        pass
```
    4.捕获异常后，应有正确的应对策略
```python
    # no
    try:
        page = int(page)
    except (ValueError, TypeError):
        raise

    # yes
    try:
        page = int(page)
    except (ValueError, TypeError):
        return {'code': 'error', 'msg': 'page 参数错误'}
```
    5.当捕获异常时，使用as而不要用逗号
```python
    # no
    try:
        raise Error
    except Error, error:
        pass

    # yes
    try:
        raise Error
    except Error as error:
        pass
```
    6.捕获异常后如需重新抛出用raise
```python
    # no
    try:
        raise Error
    except Error as e:
        raise e

    # yes
    try:
        raise Error
    except Error as e:
        raise
```
## 面向对象 
    1.明确类对外提供的接口
    2.避免创建万能类
    3.消除无关紧要的类
    4.尽量避免metaclass的黑色魔法
    5.继承时应遵循里氏替换原则
    6.类子程序限制在50行以内，一个子程序只实现一个功能
    7.不定期review，检查类的规范性
## 文档注释
    1.请使用中文注释
    2.声明类时应注明该类的作用
```python
    class Cache(object):
        """缓存类提供缓存接口
        """
        def get(self, key):
            pass

        def set(self, key, value, ttl):
            pass
```
    3.声明方法时注明该方法的作用
```python
    def sum(x, y):
        """累加计算，返回两个参数的累加值
        """
        return x + y
```
    4.类对外提供的接口应写注释
```
    class Car(object):
        """汽车类，对汽车的抽象
        """

        def run(self, road):
            """汽车在道路上行驶
            """
            pass
```
    5.注释使用三重双引号而非三重单引号
    6.为临时代码使用TODO注释
    7.注释和代码不要写在同一行
```python
    a = 3 # this is bad

    # this is good
    a = 3
```
## 单元测试
    1.为自己编写的类写Testcase
    2.为系统对外提供的功能接口写Testcase
    3.使用框架时应结合框架提供的测试模块编写Testcase
    4.应注意Testcase的错误