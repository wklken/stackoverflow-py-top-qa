

### 如何flush Python的print输出

问题 [链接](http://stackoverflow.com/questions/230751/how-to-flush-output-of-python-print)
重复问题 [链接](http://stackoverflow.com/questions/107705/python-output-buffering)

默认print输出到sys.stdout

    import sys
    sys.stdout.flush()

参考
[http://docs.python.org/reference/simple_stmts.html#the-print-statement](http://docs.python.org/2/reference/simple_stmts.html#the-print-statement)
[http://docs.python.org/library/sys.html](http://docs.python.org/2/library/sys.html)
[http://docs.python.org/library/stdtypes.html#file-objects](http://docs.python.org/2/library/stdtypes.html#file-objects)

### Python如何检查一个对象是list或者tuple，但是不是一个字符串

问题 [链接](http://stackoverflow.com/questions/1835018/python-check-if-an-object-is-a-list-or-tuple-but-not-string)

原来的做法是

    assert isinstance(lst, (list, tuple))

有没有更好的做法

我认为下面的方式是你需要的

    assert not isinstance(lst, basestring)

原来的方式，你可能会漏过很多像列表，但并非list/tuple的

### Python中检查类型的权威方法

问题 [链接](http://stackoverflow.com/questions/152580/whats-the-canonical-way-to-check-for-type-in-python)

检查一个对象是否是给定类型或者对象是否继承于给定类型？

比如给定一个对象o,如何判断是不是一个str

检查是否是str

    type(o) is str

检查是否是str或者str的子类

    isinstance(o, str)

下面的方法在某些情况下有用

    issubclass(type(o), str)
    type(o) in ([str] + str.__subclasses__())

注意，你或许想要的是

    isinstance(o, basestring)

因为unicode字符串可以满足判定(unicode 不是str的子类，但是str和unicode都是basestring的子类)

可选的，isinstance可以接收多个类型参数，只要满足其中一个即True

    isinstance(o, (str, unicode))

### 如何判断一个变量的类型

问题 [链接](http://stackoverflow.com/questions/402504/how-to-determine-the-variable-type-in-python)

使用type

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

另外一个相同的问题  [链接](http://stackoverflow.com/questions/2225038/python-determine-the-type-of-an-object)

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

### Python中如何注释一段代码块/为什么Python没有多行注释

问题 [链接](http://stackoverflow.com/questions/675442/comment-out-a-python-code-block)

问题 [链接](http://stackoverflow.com/questions/397148/why-doesnt-python-have-multiline-comments)

Python中多行注释的方式是

    #print "hello"
    #print "world"

注意，不要使用多行字符串对代码块进行注释，除非是文档字符串docstring.

### Python中单引号和双引号

问题 [链接](http://stackoverflow.com/questions/56011/single-quotes-vs-double-quotes-in-python)

根据文档，两者貌似没什么区别，有什么风格上的使用建议么？


我偏好于

双引号： 用于插入/改写的字符串， 自然语言消息

单引号： 标识符字符串，例如字典key. 除非该字符串本身有单括号或者我忘了

三引号： 文档字符串docstring 或者 正则表达式中原始字符串raw string

例如：

    LIGHT_MESSAGES = {
        'English': "There are %(number_of_lights)s lights.",
        'Pirate':  "Arr! Thar be %(number_of_lights)s lights."
    }

    def lights_message(language, number_of_lights):
        """Return a language-appropriate string reporting the light count."""
        return LIGHT_MESSAGES[language] % locals()

    def is_pirate(message):
        """Return True if the given message sounds piratical."""
        return re.search(r"(?i)(arr|avast|yohoho)!", message) is not None

这里很偏个人风格，所以，根据自己喜好，保持一致就行


### 你是否能解释Python中的闭包

问题 [链接](http://stackoverflow.com/questions/13857/can-you-explain-closures-as-they-relate-to-python)


参考文章 [Closure on closures](http://mrevelle.blogspot.com/2006/10/closure-on-closures.html)

    对象是数据和方法关联
    比宝石函数和数据关联

例如

    def make_counter():
        i = 0
        def counter(): # counter() is a closure
            nonlocal i
            i += 1
            return i
        return counter

    c1 = make_counter()
    c2 = make_counter()

    print (c1(), c1(), c2(), c2())
    # -> 1 2 1 2

其他解释(感觉英文更精准)

    A function that references variables from a containing scope, potentially after flow-of-control has left that scope

    A function that can refer to environments that are no longer active.
    A closure allows you to bind variables into a function without passing them as parameters.

### Python Lambda - why?

问题 [链接](http://stackoverflow.com/questions/890128/python-lambda-why)

Are you talking about lambda functions? Like

你是指lambda函数? 例如

    f = lambda x: x**2 + 2*x - 5

非常有用, python支持函数式编程, 你可以将函数作为参数进行传递去做一些事情

例子:

    mult3 = filter(lambda x: x % 3 == 0, [1, 2, 3, 4, 5, 6, 7, 8, 9])
    # sets mult3 to [3, 6, 9]

这样相对于完整地函数更为简短

    def filterfunc(x):
        return x % 3 == 0
    mult3 = filter(filterfunc, [1, 2, 3, 4, 5, 6, 7, 8, 9])

当然, 这个例子你也可以使用列表解析进行处理

    mult3 = [x for x in [1, 2, 3, 4, 5, 6, 7, 8, 9] if x % 3 == 0]

甚至是

    range(3,10,3)

lambda function may be the shortest way to write something out

