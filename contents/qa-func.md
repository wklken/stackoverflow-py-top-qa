
### 如何获取一个函数的函数名字符串

问题 [链接](http://stackoverflow.com/questions/251464/how-to-get-the-function-name-as-string-in-python)

    my_function.__name__
    >>> import time
    >>> time.time.__name__
    'time'
### 用函数名字符串调用一个函数

问题 [链接](http://stackoverflow.com/questions/3061/calling-a-function-from-a-string-with-the-functions-name-in-python)

假设模块foo有函数bar:

    import foo
    methodToCall = getattr(foo, 'bar')
    result = methodToCall()

或者一行搞定

    result = getattr(foo, 'bar')()


### Python中**和*的作用

问题  [链接](http://stackoverflow.com/questions/36901/what-does-double-star-and-star-do-for-python-parameters)


*args和**kwargs允许函数拥有任意数量的参数，具体可以查看 [more on defining functions](http://docs.python.org/dev/tutorial/controlflow.html#more-on-defining-functions)

*args将函数所有参数转为序列

    In [1]: def foo(*args):
    ...:     for a in args:
    ...:         print a
    ...:
    ...:

    In [2]: foo(1)
    1


    In [4]: foo(1,2,3)
    1
    2
    3

**kwargs 将函数所有关键字参数转为一个字典

    In [5]: def bar(**kwargs):
    ...:     for a in kwargs:
    ...:         print a, kwargs[a]
    ...:
    ...:

    In [6]: bar(name="one", age=27)
    age 27
    name one

两种用法可以组合使用

    def foo(kind, *args, **kwargs):
        pass

*l的另一个用法是用于函数调用时的参数列表解包(unpack)

    In [9]: def foo(bar, lee):
    ...:     print bar, lee
    ...:
    ...:

    In [10]: l = [1,2]

    In [11]: foo(*l)
    1 2

在Python3.0中，可以将*l放在等号左边用于赋值  [Extended Iterable Unpacking](http://www.python.org/dev/peps/pep-3132/)

    first, *rest = [1,2,3,4]
    first, *l, last = [1,2,3,4]

