# Programming standard for python
## 变量声明
    1.类名单词首字母均大写
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
    5.避免出现难以理解的数值，如：
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
    7.变量名应尽量见名思意
```python
    # no
    for item in cats:
        pass

    # yes
    for cat in cats:
        pass
```
## 作用域
    1.变量声明时应考虑其作用域，避免污染全局作用域
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
    2.嵌套的函数，内部函数可使用外部的变量，但不能对其取值同时赋值，否则NameError
```python
    # error
    def sum(x):
        def _sum(y):
            x = x
            return x + y
        return _sum
    # yes
    def sum(x):
        def _sum(y):
            x[0] = x[0]
            return x[0] + y
        return _sum
```
    3.支持在类或函数内部定义类以及函数
```python
    class MyClass(object):
    
        class SubClass:
            pass
    
    def my_function():
        class MyClass:
            pass
```
## 缩进与换行
    1.子语句块缩进4个空格而非1个tab
    2.语句过长应换行，超过80个字符，建议换行，超过100个字符必须换行
    3.字符串换行：
```python
    logger.warning('aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'
                   'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa')
```
    4.参数换行：
```python
    urllib2.urlopen('http://www.google.com',
                    data='aaaaaaaaaaaaaaaaaaaaa',
                    headers={},
                    cookies={})
```
    5.尽量在代码顶部import, 第三方模块与自己的模块之间空一行
```python
    # -*- coding: utf-8 -*-
    import time
    import django
    
    import my_package
```
    6.函数与函数、类与函数、类与类之间空两行
```python
    class MyClass(object):
        pass
        
        
    def my_function():
        pass
```
    7.类子方法以及变量之间空一行
```python
    class MyClass(object):
        
        def __init__(self):
            pass
        
        def func(self):
            pass
```
    8.方法内分组的语句块之间建议空一行
```python
    def my_function():
        dog = Dog()
        dog.bark()
        dog.run()

        sheep = Sheep()
        sheep.bleat()
        sheep.run()
```
## 输入检验(仅针对用户的输入数据)
    1.检验数据是否为空
```python
    # no
    def devision(n):
        return 10 / n

    # yes
    def devision(n):
        if n == None:
            raise
        else:
            return 10 / n
```
    2.检验数据类型是否正确
```python
    # no
    def devision(n):
        return 10 / n

    # yes
    def devision(n):
        if type(n) != int:
            raise
        else:
            return 10 / n
```
    3.检验数据边界是否正确
```python
    # no
    def devision(n):
        return 10 / n

    # yes
    def devision(n):
        if type(n) != int or n == 0 :
            raise
        else:
            return 10 / n
```
## 布尔变量
    1.尽可能使用隐式的false
```python
    # no
    if foo != []:

    # yes
    if foo:
```
    2.不要将一个布尔变量用==与false比较，使用if not:
```python
    # no
    if x == false:

    # yes
    if not x:
```
    3.整数变量尽量不与布尔值比较
```python
    x = 1
    # no
    if x == false:

    # yes
    if x == 0:
```
## 模块导入
    1.尽量仅对包和模块使用导入
```python
    # no
    from my_package.my_module import my_class  

    # yes
    from my_package import my_module
    my_module.my_class
```
    2.按包的全路径导入模块。下面两种方式导入的模块有不同的内存空间
```python
    # no
    import my_module

    # yes
    from my_package import my_module
```
    3.每个导入应独占一行
```
    # no
    import os, sys

    # yes
    import os
    import sys
```
    4.导入顺序按照 标准库导入、第三方库导入、应用程序指定导入
```python
    # no
    import os
    import foo
    import django

    # yes
    import os
    import django

    import foo
```
## 异常捕获
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
        # ...
    except Exception as e:
        pass

    # yes
    try:
        data = json.loads(data)
    except (ValueError, TypeError):
        pass
```
    4.捕获异常后，应有正确的应对策略，而不是一味的raise，比如用户输错了页数
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
        page = 1
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
## 杂项
    1.文件或socket操作完成时应显示关闭，或者放在上下文中操作，如果对象不支持with，但支持close，使用contextlib.closing()
```python
    f = open('read.md')  # no
    with open('read.md') as f:  # yes
    with contextlib.closing(open('read.md')) as f:  # yes
```
    2.字符串连接时看情况操作
```python
    x = '%s%s' % (a, b)  # no
    x = a + b  # yes
    x = 'hello,' + a + ' ' + 'b' + ' ' + 'world!'  # no
    x = 'hellp,%s %s world!' % (a, b)  # yes
```
    3.类如果不继承自其它类就应显示的从object继承，新式类的继承使用c3算法，旧式类深度优先
```python
    class MyClass:  # no
    class MyClass(object):  # yes
```
    4.可执行脚本被导入时也不应被执行
```python
    main()  # no

    if __name__ == '__main__':
        main()  # yes
```
    5.不要使用分号将两句代码放在一行
```python
    reload(sys); sys.path.append('.')  # no

    reload(sys)
    sys.path.append('.')  # yes
```
    6.if条件过多，看是否可以转换成表驱动法
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
    7.#!不是模块文件所必须的，仅在作为脚本时加入此行注释
    8.所有模块均以`# -*- coding: utf-8 -*-`开头
    9.保持使用字符串引号的一致性
```python
    # no
    s = "Why are you hiding your eyes?"
    s = 'The lint. It burns. It burns us.'
    s = "Always the great lint. Watching. Watching."

    # yes
    s = 'Why are you hiding your eyes?'
    s = "I'm scared of lint errors."
    s = '"Good!" thought a happy Python reviewer.'
```
    10.注意可能发生的内存泄露，交叉引用时一方可使用弱引用weakref
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
    11.在简单的情况下使用列表推导式，复杂情况下不建议
```python
    # no
    result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

    # yes
    squares = [x * x for x in range(10)]
```
    12.单行函数可使用lambda
```python
    # no
    def sum(x, y):
        return x + y

    # yes
    sum = lambda x, y: x + y
```
    13.可迭代对象数据量较大时请使用生成器
```python
    # no for python2
    for in in range(100000):
        pass

    # yes
    for i in xrange(100000):
        pass
```
    14.起无限循环时推荐while 1
```python
    # no
    while True:
        pass

    # yes
    while 1:
        pass
```
    15.单行简短函数可使用条件表达式
```python
    # no
    if condition:
        x = 1
    else:
        x = 2

    # yes
    x = 1 if condition else 2
```
    16.单个py文件过大应考虑拆分
    17.模块间尽量避免交叉引用
    18.默认参数应为不可变类型
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
## 类抽象
    1.核心类的抽象应仔细斟酌，避免不当的设计，必要时先构建UML图以及伪代码编程
    2.明确类对外提供的接口
    3.避免创建万能类
    4.消除无关紧要的类
    5.尽量避免metaclass的黑色魔法
    6.继承时应遵循里氏替换原则
    7.类子程序或方法应尽量限制在50行以内，一个子程序或方法只实现一个功能
    8.不定期review，检查类的规范性
## 日志记录
    1.代码执行到异常情况时应有日志记录，以便追踪错误根源
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
    2.多进程记录日志时应注意是否对同一个日志文件进行写入操作
    3.确定合适的日志格式
```python 
    format = ('%(levelname)s %(asctime)s %(module)s '
             '%(pathname)s [%(lineno)d]: %(message)s')
```
    4.用supervisor托管进程时，可使日志写入console，然后在supervisor中配置日志文件
## 代码测试
    1.为自己编写的类写testcase
    2.为系统对外提供的功能接口写testcase
    3.使用框架时应结合框架提供的测试模块编写testcase
    4.应注意testcase的错误
## 文档注释
    1.请使用中文注释
    2.声明类时应注明该类的作用
```python
    class Cache(object):
        """缓存类提供缓存接口

        公有方法为get, set
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
            """汽车在某条道路上行驶

            提供参数road指明汽车行驶的道路
            """
            pass
```
    5.注释使用三重双引号而非三重单引号
    6.为临时代码使用TODO注释
    7.注释的时候建议加上作者、编写日期、修改日期、修改人等内容
    8.注释和代码不要写在同一行
```python
    # no
    b = 4 # this is bad

    # yes
    # this is good
    a = 3
```