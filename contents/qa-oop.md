
### Python 'self' 解释

问题 [链接](http://stackoverflow.com/questions/2709821/python-self-explained)


self关键字的作用是什么？
我理解他用户在创建class时具体化实例，但我无法理解为何需要给每个方法加入self作为参数.

举例，在ruby中，我这么做:

    class myClass
        def myFunc(name)
            @name = name
        end
    end

我可以很好地理解，非常简单.但是在Python中，我需要去加入self:

    class myClass:
        def myFunc(self, name):
            self.name = name

有谁能解释下么？

使用self关键字的原因是，Python没有@语法用于引用实例属性.Python决定用一种方式声明方法:实例对象自动传递给属于它的方法,但不是接收自动化：方法的第一个参数是调用这个方法的实例对象本身.这使得方法整个同函数一致,并且由你自己决定真实的名（虽然self是约定，但当你使用其他名的时候，通常人们并不乐意接受）.self对于代码不是特殊的，只是另一个对象.

Python本来可以做一些用来区分真实的名字和属性的区别 —— 像Ruby有的特殊语法，或者像C++/Java的命令声明,或者其他可能的的语法 —— 但是Python没有这么做.Python致力于使事情变得明确简单，让事情是其本身，虽然并不是全部地方都这么做，但是实例属性石这么做的！这就是为什么给一个实例属性赋值时需要知道是给哪个实例赋值,并且，这就是为什么需要self

举例

    class Vector(object):
        def __init__(self, x, y):
            self.x = x
            self.y = y
        def length(self):
            return math.sqrt(self.x ** 2 + self.y ** 2)

等价于

    def length_global(vector):
        return math.sqrt(vector.x ** 2 + vector.y ** 2)

另外

    v_instance.length()
    转为
    Vector.length(v_instance)
### 为什么Python的'private'方法并不是真正的私有方法

问题 [链接](http://stackoverflow.com/questions/70528/why-are-pythons-private-methods-not-actually-private)

Python允许我们创建'private' 函数：变量以两个下划线开头，像这样： *__myPrivateMethod()*.
但是，如何解释：

    >>> class MyClass:
    ...     def myPublicMethod(self):
    ...             print 'public method'
    ...     def __myPrivateMethod(self):
    ...             print 'this is private!!'
    ...
    >>> obj = MyClass()
    >>> obj.myPublicMethod()
    public method
    >>> obj.__myPrivateMethod()
    Traceback (most recent call last):
    File "", line 1, in
    AttributeError: MyClass instance has no attribute '__myPrivateMethod'
    >>> dir(obj)
    ['_MyClass__myPrivateMethod', '__doc__', '__module__', 'myPublicMethod']
    >>> obj._MyClass__myPrivateMethod()
    this is private!!


dir(obj) 和 obj._MyClass__myPrivateMethod()


回答

‘private'只是用作，确保子类不会意外覆写父类的私有方法和属性.不是为了保护外部意外访问而设计的！

例如:

    >>> class Foo(object):
    ...     def __init__(self):
    ...         self.__baz = 42
    ...     def foo(self):
    ...         print self.__baz
    ...
    >>> class Bar(Foo):
    ...     def __init__(self):
    ...         super(Bar, self).__init__()
    ...         self.__baz = 21
    ...     def bar(self):
    ...         print self.__baz
    ...
    >>> x = Bar()
    >>> x.foo()
    42
    >>> x.bar()
    21
    >>> print x.__dict__
    {'_Bar__baz': 21, '_Foo__baz': 42}

当然，这对于两个同名的类没有作用

另外，可以查看diveintopython的解释 [入口](http://www.faqs.org/docs/diveintopython/fileinfo_private.html#d0e11521)

### Python中类方法的作用是什么

问题 [链接](http://stackoverflow.com/questions/38238/what-are-class-methods-in-python-for)


我现在意识到，我不需要像我在使用java的static方法那样使用类方法，但是我不确定什么时候使用

谁能通过一个好的例子解释下Python中的类方法，至少有人能告诉我什么时候确实需要使用类方法


类方法用在：当你需要使用不属于任何明确实例的方法,但同时必须涉及类.有趣的是，你可以在子类中覆写，这在Java的static方法和Python的模块级别函数中是不可能做到的

如果你有一个MyClass, 并且一个模块级别函数操作MyClass(工厂，依赖注入桩等等), 声明一个类方法.然后这个类方法可以在子类中调用

### Python中 __new__ 和 __init__的用法

问题 [链接](http://stackoverflow.com/questions/674304/pythons-use-of-new-and-init)

我很疑惑，为何__init__总是在__new__之后调用

如下

    class A(object):
        _dict = dict()

        def __new__(cls):
            if 'key' in A._dict:
                print "EXISTS"
                return A._dict['key']
            else:
                print "NEW"
                return super(A, cls).__new__(cls)

        def __init__(self):
            print "INIT"
            A._dict['key'] = self
            print ""

    a1 = A()
    a2 = A()
    a3 = A()

输出

    NEW
    INIT

    EXISTS
    INIT

    EXISTS
    INIT

有木有人可以解释一下

来自 [链接](http://mail.python.org/pipermail/tutor/2008-April/061426.html)


使用__new__,当你需要控制一个实例的生成

使用__init__,当你需要控制一个实例的初始化

__new__是实例创建的第一步.最先被调用，并且负责返回类的一个新实例.

相反的,__init__不返回任何东西，只是负责在实例创建后进行初始化

通常情况下，你不必重写__new__除非你写一个子类继承不可变类型，例如str,int,unicode或tuple


你必须了解到，你尝试去做的用[Factory](http://en.wikipedia.org/wiki/Factory_object)可以很好地解决，并且是最好的解决方式.使用__new__不是一个简洁的处理方式,一个[factory例子](http://code.activestate.com/recipes/86900/)


### 如何获取一个实例的类名

问题 [链接](http://stackoverflow.com/questions/510972/getting-the-class-name-of-an-instance-in-python)

    x.__class__.__name__

### @staticmethod和@classmethod的区别

问题 [链接](http://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python)

staticmethod，静态方法在调用时，对类及实例一无所知

仅仅是获取传递过来的参数，没有隐含的第一个参数，在Python里基本上用处不大，你完全可以用一个模块函数替换它

classmethod, 在调用时，将会获取到其所在的类，或者类实例，作为其第一个参数

当你想将函数作为一个类工厂时，这非常有用: 第一个参数是类，你可以实例化出对应实例对象，甚至子类对象。

可以观察下 dict.fromkey(),是一个类方法，当子类调用时，返回子类的实例

    >>> class DictSubclass(dict):
    ...     def __repr__(self):
    ...         return "DictSubclass"
    ...
    >>> dict.fromkeys("abc")
    {'a': None, 'c': None, 'b': None}
    >>> DictSubclass.fromkeys("abc")
    DictSubclass
    >>>

###  如何定义静态方法(static method)

问题 [链接](http://stackoverflow.com/questions/735975/static-methods-in-python)

使用 [staticmethod](http://docs.python.org/2/library/functions.html#staticmethod)装饰器


    class MyClass(object):
        @staticmethod
        def the_static_method(x):
            print x
    MyClass.the_static_method(2) # outputs 2

### Python中的类变量(环境变量)

问题 [链接](http://stackoverflow.com/questions/68645/static-class-variables-in-python)

在类中定义的变量，不在方法定义中，成为类变量或静态变量

    >>> class MyClass:
    ...     i = 3
    ...
    >>> MyClass.i
    3

i是类级别的变量，但这里要和实例级别的变量i区分开

    >>> m = MyClass()
    >>> m.i = 4
    >>> MyClass.i, m.i
    >>> (3, 4)

这和C++/java完全不同，但和C#区别不大，C#不允许类实例获取静态变量

具体见 [what the Python tutorial has to say on the subject of classes and class objects](http://docs.python.org/2/tutorial/classes.html#SECTION0011320000000000000000)

另外，静态方法

    class C:
        @staticmethod
        def f(arg1, arg2, ...): ...

### 如何判断一个对象是否拥有某个属性

问题 [链接](http://stackoverflow.com/questions/610883/how-to-know-if-an-object-has-an-attribute-in-python)

    if hasattr(a, 'property'):
        a.property

两种风格

EAFP(easier to ask for forgiveness than permission)

LBYL(look before you leap)

相关内容
[EAFP vs LBYL (was Re: A little disappointed so far)](http://web.archive.org/web/20070929122422/http://mail.python.org/pipermail/python-list/2003-May/205182.html)
[EAFP vs. LBYL @Code Like a Pythonista: Idiomatic Python](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html#eafp-vs-lbyl)

    try:
        doStuff(a.property)
    except AttributeError:
        otherStuff()
    or

    if hasattr(a, 'property'):
        doStuff(a.property)
    else:
        otherStuff()

### Python中有没有简单优雅的方式定义单例类

问题 [链接](http://stackoverflow.com/questions/31875/is-there-a-simple-elegant-way-to-define-singletons-in-python)

我不认为有必要，一个拥有函数的模块（不是类）可以作为很好的单例使用，它的所有变量被绑定到这个模块，无论如何都不能被重复实例化

如果你确实想用一个类来实现，在python中不能创建私有类或私有构造函数,所以你不能隔离多个实例而仅仅通过自己的API来访问属性

我还是认为将函数放入模块，并将其作为一个单例来使用是最好的办法

### 理解Python的Super()和init方法

问题 [链接](http://stackoverflow.com/questions/576169/understanding-python-super-and-init-methods)

尝试着去理解super().从表面上看，两个子类都能正常创建.我只是好奇他们两者之间的不同点

    class Base(object):
        def __init__(self):
            print "Base created"

    class ChildA(Base):
        def __init__(self):
            Base.__init__(self)

    class ChildB(Base):
        def __init__(self):
            super(ChildB, self).__init__()

    print ChildA(),ChildB()

回答

Super让你避免明确地引用基类，这是一点。最大的优势是，当出现多重继承的时候，各种[有趣的情况](http://www.artima.com/weblogs/viewpost.jsp?thread=236275)就会出现。查看super[官方文档](http://docs.python.org/library/functions.html#super).

另外注意，在Python3.0中，可以使用super().__init__() 代替 super(ChildB, self).__init__().IMO略有优势.


### 在Python中，如何判断一个对象iterable?

问题 [链接](http://stackoverflow.com/questions/1952464/in-python-how-do-i-determine-if-an-object-is-iterable)

1. 检查__iter__对序列类型有效，但是对例如string，无效


        try:
            iterator = iter(theElement)
        except TypeError:
            # not iterable
        else:
            # iterable

        # for obj in iterator:
        #     pass

2. 使用collections

        import collections

        if isinstance(e, collections.Iterable):
            # e is iterable
