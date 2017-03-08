# Programming standard for python
## 变量声明
    1.类名单词首字母均大写
```python
    class base_cache:
        # bad
        pass
    class BaseCache:
        # good
        pass
```
    2.类私有成员及私有方法以下划线开头，单词小写，单词与单词之间下划线分隔
```python
    class BaseCache:
        # bad
        def make_key(self, *args, **kwargs):
            pass
    class BaseCache:
        # good
        def _make_key(self, *args, **kwargs):
            pass
```
    3.局部变量以及函数定义，单词均小写，单词与单词之间下划线分隔，
```python
    def getmessagefromwechat():
        # bad
        pass
    def get_message_from_wechat():
        # good
        pass
```
    4.常量单词均大写，单词与单词之间以下划线分隔
```python
    http_methods = ('get', 'post', 'head', 'options', 'trace', 'put', 'delete')  # bad
    HTTP_METHODS = ('get', 'post', 'head', 'options', 'trace', 'put', 'delete')  # good
```
    5.避免出现难以理解的数值，如：
```python
  users.filter(sex=1)  # bad
  MALE = 1
  users.filter(sex=MALE)  # good
```
    6.变量应尽量在第一次使用前声明赋值，如：
```python
  car = Car()
  do_something()
  car.run()  # bad
  do_something()
  car = Car()
  car.run()  # good
```
    7.变量名应尽量见名思意
```python
    for item in cats:
        pass  # bad
    for cat in cats:
        pass  # good
```
## 作用域
    1.变量声明时应考虑其作用域，避免污染全局作用域
```python
    car = Car()
    road = Road()
    def run():
        car.run_at(road)
    run()  # bad
    def run():
        car = Car()
        road = Road()
        car.run_at(road)
    run()  # good
```
    2.嵌套的函数，内部函数可使用外部的变量，但不能对其赋值，否则NameError
```python
    # error
    def sum(x):
        def _sum(y):
           x = x
           return x + y
       return _sum
    # good
    def sum(x):
        def _sum(y):
           return x + y
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
    # coding: utf-8
    import time
    import django
    
    from my_module import my_class
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
    def devision(n):
        return 10 / n  # bad
    def devision(n):
        if n == None:
            raise
        else:
            return 10 / n  # good
```
    2.检验数据类型是否正确
```python
    def devision(n):
        return 10 / n  # bad
    def devision(n):
        if type(n) == int:
            raise
        else:
            return 10 / n  # good
```
    3.检验数据边界是否正确
```python
    def devision(n):
        return 10 / n  # bad
    def devision(n):
        if type(n) == int and n != 0 :
            raise
        else:
            return 10 / n  # good
```
## 布尔变量
    1.尽可能使用隐式的false
```python
    if foo != []:  # bad
    if foo:  # good
```
    2.不要将一个布尔变量用==与false比较，使用if not:
```python
    if x == false:  # bad
    if not x:  # good
```
    3.整数变量尽量不与布尔值比较
```python
    x = 1
    if x == false:  # bad
    if x == 0:  # good 
```
## 模块导入
    1.尽量仅对包和模块使用导入
```python
    from my_package.my_module import my_class  # bad
    from my_package import my_module  # good
```
    2.按包的全路径导入模块。下面两种方式导入的模块有不同的内存空间
```python
    import my_module  # bad
    from my_package import my_module  # good
```
## 异常捕获
    1.应考虑代码可能的出错情况，进行异常捕获
```python
    dog = dogs[0]  # bad
    try:
        dog = dogs[0]
    except IndexError:
        dog = Dog()  # good
```
    2.需进行异常捕获的代码越短越好
```python
    # bad
    try:
        dog = dogs[0]
        dog.run()
    except IndexError:
        logger.warning('Dogs index error')
        raise
    # good
    try:
        dog = dogs[0]
    except IndexError:
        logger.warning('Dog index error')
        raise
    dog.run()
```
    3.尽量避免对任何异常都进行捕获的代码，因为很有可能隐藏真正的bug
```python
    # bad
    try:
        data = json.loads(data)
        # ...
    except Exception as e:
        pass
    # good
    try:
        data = json.loads(data)
    except (ValueError, TypeError):
        pass
```
    4.捕获异常后，应有正确的应对策略，而不是一味的raise，比如用户输错了页数
```python
    # bad
    try:
        page = int(page)
    except (ValueError, TypeError):
        raise
    # good
    try:
        page = int(page)
    except (ValueError, TypeError):
        page = 1
```
## 杂项
    1.文件或socket操作完成时应显示关闭，或者放在上下文中操作
```python
    f = open('read.md')  # bad
    with open('read.md') as f:  # good
```
    2.字符串连接时看情况操作
```python
    x = '%s%s' % (a, b)  # bad
    x = a + b  # good
    x = 'hello,' + a + ' ' + 'b' + ' ' + 'world!'  # bad
    x = 'hellp,%s %s world!' % (a, b)  # good
```
    3.类如果不继承自其它类就应继承自object，新式类的继承使用c3算法，旧式类深度优先
```python
    class MyClass:  # bad
    class MyClass(object):  # good
```
    4.可执行脚本被导入时也不应被执行
```python
    main()  # bad
    if __name__ == '__main__':
        main()
```
    5.不要使用分号将两句代码放在一行
```python
    reload(sys); sys.path.append('.')  # bad
    reload(sys)
    sys.path.append('.')  # good
```
## 代码构建
    1.核心类的抽象应仔细斟酌，避免不当的设计，必要时先构建UML图以及伪代码编程
    2.明确类对外提供的接口
    3.类不是方法收容所，不要把什么方法都往类里面塞
    4.类子程序应尽量简短，一个子程序只实现一个小功能
## 日志记录
    1.代码执行到异常情况时应有日志记录，以便追踪错误根源
    2.多进程记录日志时应注意是否对同一个日志文件进行写入操作
    3.确定合适的日志格式
    4.用supervisor托管进程时，可使日志写入console，然后在supervisor中配置日志文件
## 代码测试
    1.为自己编写的类写testcase
    2.对系统对外提供的功能接口进行测试
    3.应注意testcase的错误
    4.使用框架时应结合框架提供的测试模块编写testcase
## 文档注释
    1.声明类时应注明该类的作用
    2.声明方法时注明该方法的作用
    3.类对外提供的接口应写注释
