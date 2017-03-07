# Programming standard for python
## 变量
    1.类名单词首字母均大写，如：Person、BaseCache、SubSystem
    2.类私有成员及私有方法以下划线开头，单词小写，单词与单词之间下划线分隔，如：_animals, _pages
    3.局部变量以及函数定义，单词均小写，单词与单词之间下划线分隔，如：message, get_message_from_wechat
    4.常量单词均大写，单词与单词之间以下划线分隔，如：HTTP_METHODS, MALE
    5.避免出现难以理解的数值，如：
```python
  users.filter(sex=1)  # bad
  MALE = 1
  users.filter(sex=MALE)  # good
```
    6.变量应尽量在第一次使用前声明赋值，如：
```python
  car = Car()
  # do other things...
  car.run()  # bad
  # do other things
  car = Car()
  car.run()  # good
```
    7.变量名应尽量见名思意，不当例子：xx、item，恰当例子：weigth、price
  
