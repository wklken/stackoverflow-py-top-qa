
### 如何flush Python的print输出

问题 [链接](http://stackoverflow.com/questions/230751/how-to-flush-output-of-python-print)
重复问题 [链接](http://stackoverflow.com/questions/107705/python-output-buffering)

默认print输出到sys.stdout
```py
import sys
sys.stdout.flush()
```
参考
[http://docs.python.org/reference/simple_stmts.html#the-print-statement](http://docs.python.org/2/reference/simple_stmts.html#the-print-statement)
[http://docs.python.org/library/sys.html](http://docs.python.org/2/library/sys.html)
[http://docs.python.org/library/stdtypes.html#file-objects](http://docs.python.org/2/library/stdtypes.html#file-objects)

### Python如何检查一个对象是list或者tuple，但是不是一个字符串

问题 [链接](http://stackoverflow.com/questions/1835018/python-check-if-an-object-is-a-list-or-tuple-but-not-string)

原来的做法是
```py
assert isinstance(lst, (list, tuple))
```
有没有更好的做法

我认为下面的方式是你需要的
```py
assert not isinstance(lst, basestring)
```
原来的方式，你可能会漏过很多像列表，但并非list/tuple的

### Python中检查类型的权威方法

问题 [链接](http://stackoverflow.com/questions/152580/whats-the-canonical-way-to-check-for-type-in-python)

检查一个对象是否是给定类型或者对象是否继承于给定类型？

比如给定一个对象o,如何判断是不是一个str

检查是否是str
```py
type(o) is str
```
检查是否是str或者str的子类
```py
isinstance(o, str)
```
下面的方法在某些情况下有用
```py
issubclass(type(o), str)
type(o) in ([str] + str.__subclasses__())
```
注意，你或许想要的是
```py
isinstance(o, basestring)
```
因为unicode字符串可以满足判定(unicode 不是str的子类，但是str和unicode都是basestring的子类)

可选的，isinstance可以接收多个类型参数，只要满足其中一个即True
```py
isinstance(o, (str, unicode))
```
### 如何判断一个变量的类型

问题 [链接](http://stackoverflow.com/questions/402504/how-to-determine-the-variable-type-in-python)

使用type
```py
>>> i = 123
>>> type(i)
<type 'int'>
>>> type(i) is int
True
>>> i = 123456789L
>>> type(i)
<type 'long'>
>>> type(i) is long
True
>>> i = 123.456
>>> type(i)
<type 'float'>
>>> type(i) is float
True
```
另外一个相同的问题  [链接](http://stackoverflow.com/questions/2225038/python-determine-the-type-of-an-object)
```py
>>> type( [] ) == list
True
>>> type( {} ) == dict
True
>>> type( "" ) == str
True
>>> type( 0 ) == int
True

>>> class Test1 ( object ):
    pass
>>> class Test2 ( Test1 ):
    pass
>>> a = Test1()
>>> b = Test2()
>>> type( a ) == Test1
True
>>> type( b ) == Test2
True
>>> type( b ) == Test1
False
>>> isinstance( b, Test1 )
True
>>> isinstance( b, Test2 )
True
>>> isinstance( a, Test1 )
True
>>> isinstance( a, Test2 )
False
>>> isinstance( [], list )
True
>>> isinstance( {}, dict )
True
```
