
### 如何结束退出一个python脚本

问题 [链接](http://stackoverflow.com/questions/73663/terminating-a-python-script)

    import sys
    sys.exit()

### foo is None 和 foo == None的区别

问题 [链接](http://stackoverflow.com/questions/26595/is-there-any-difference-between-foo-is-none-and-foo-none)

    if foo is None: pass
    if foo == None: pass

如果比较相同的对象实例，is总是返回True
而 == 最终取决于 "__eq__()"

    >>> class foo(object):
        def __eq__(self, other):
            return True

    >>> f = foo()
    >>> f == None
    True
    >>> f is None
    False

    >>> list1 = [1, 2, 3]
    >>> list2 = [1, 2, 3]]
    >>> list1==list2
    True
    >>> list1 is list2
    False

另外

    (ob1 is ob2) 等价于 (id(ob1) == id(ob2))



### 如何在循环中获取下标

问题 [链接](http://stackoverflow.com/questions/522563/accessing-the-index-in-python-for-loops)

使用enumerate

    for idx, val in enumerate(ints):
        print idx, val

### python中如何将一行长代码切成多行

问题 [链接](http://stackoverflow.com/questions/53162/how-can-i-do-a-line-break-line-continuation-in-python)

例如：

    e = 'a' + 'b' + 'c' + 'd'
    变成
    e = 'a' + 'b' +
        'c' + 'd'

括号中，可以直接换行

    a = dostuff(blahblah1, blahblah2, blahblah3, blahblah4, blahblah5,
                blahblah6, blahblah7)


非括号你可以这么做

    a = '1' + '2' + '3' + \
        '4' + '5'
    或者
    a = ('1' + '2' + '3' +
        '4' + '5')

可以查看下代码风格： [style guide](http://www.python.org/dev/peps/pep-0008/)
推荐是后一种，但某些个别情况下，加入括号会导致错误

### 为何1 in [1,0] == True执行结果是False

问题 [链接](http://stackoverflow.com/questions/9284350/why-does-1-in-1-0-true-evaluate-to-false)

有如下

    >>> 1 in [1,0]             # This is expected
    True
    >>> 1 in [1,0] == True     # This is strange
    False
    >>> (1 in [1,0]) == True   # This is what I wanted it to be
    True
    >>> 1 in ([1,0] == True)   # But it's not just a precedence issue!
                               # It did not raise an exception on the second example.

    Traceback (most recent call last):
      File "<pyshell#4>", line 1, in <module>
          1 in ([1,0] == True)
          TypeError: argument of type 'bool' is not iterable

这里python使用了比较运算符链

    1 in [1,0] == True

将被转为

    (1 in [1, 0]) and ([1, 0] == True)

很显然是false的

同样的

    a < b < c

会被转为

    (a < b) and (b < c) # b不会被解析两次

[具体文档](http://docs.python.org/2/reference/expressions.html#not-in)

### Python中的switch替代语法

问题 [链接](http://stackoverflow.com/questions/60208/replacements-for-switch-statement-in-python)

python中没有switch，有什么推荐的处理方法么

使用字典:

    def f(x):
        return {
            'a': 1,
            'b': 2,
        }.get(x, 9)

Python Cookbook中的几种方式

[Readable switch construction without lambdas or dictionaries](http://code.activestate.com/recipes/410692/)

[Exception-based Switch-Case](http://code.activestate.com/recipes/410695/)

[Using a Dictionary in place of a 'switch' statement](http://code.activestate.com/recipes/181064/)


### 使用 'if x is not None' 还是'if not x is None'

问题 [链接](http://stackoverflow.com/questions/2710940/python-if-x-is-not-none-or-if-not-x-is-none)

我总想着使用 'if x is not None' 会更加简明

但是google的Python风格指南使用的却是 'if x is not None'

性能上没有什么区别，他们编译成相同的字节码

    Python 2.6.2 (r262:71600, Apr 15 2009, 07:20:39)
    >>> import dis
    >>> def f(x):
    ...    return x is not None
    ...
    >>> dis.dis(f)
    2           0 LOAD_FAST                0 (x)
                3 LOAD_CONST               0 (None)
                6 COMPARE_OP               9 (is not)
                9 RETURN_VALUE
    >>> def g(x):
    ...   return not x is None
    ...
    >>> dis.dis(g)
    2           0 LOAD_FAST                0 (x)
                3 LOAD_CONST               0 (None)
                6 COMPARE_OP               9 (is not)
                9 RETURN_VALUE

在风格上，我尽量避免 'not x is y' 这种形式，虽然编译器会认为和 'not (x is y)'一样，但是读代码的人或许会误解为 '(not x) is y'

如果写作 'x is not y' 就不会有歧义

最佳实践

    if x is not None:
        # Do something about x

### 在非创建全局变量的地方使用全局变量

问题 [链接](http://stackoverflow.com/questions/423379/using-global-variables-in-a-function-other-than-the-one-that-created-them)

如果我在一个函数中创建了全局变量，如何在另一个函数中使用？

回答：

你可以在给全局变量赋值的函数中声明 global

    globvar = 0

    def set_globvar_to_one():
        global globvar    # Needed to modify global copy of globvar
        globvar = 1

    def print_globvar():
        print globvar     # No need for global declaration to read value of globvar

    set_globvar_to_one()
    print_globvar()       # Prints 1

我猜想这么做的原因是，全局变量很危险，Python想要确保你真的知道你要对一个全局的变量进行操作

如果你想知道如何在模块间使用全局变量，查看其他回答
