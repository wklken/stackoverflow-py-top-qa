
### json和simplejson的区别

问题 [链接](http://stackoverflow.com/questions/712791/json-and-simplejson-module-differences-in-python)

json就是simple，加入到标准库. json在2.6加入，simplejson在2.4+,2.6+,更有优势

另外，simplejson更新频率更高，如果你想使用最新版本，建议用simplejson

好的做法是

    try: import simplejson as json
    except ImportError: import json

另外,可以关注二者性能上的比较

### 如何用http下载一个文件

问题 [链接](http://stackoverflow.com/questions/22676/how-do-i-download-a-file-over-http-using-python)

直接使用urllib

    import urllib
    urllib.urlretrieve ("http://www.example.com/songs/mp3.mp3", "mp3.mp3")

使用urllib2,并提供一个进度条

    import urllib2

    url = "http://download.thinkbroadband.com/10MB.zip"

    file_name = url.split('/')[-1]
    u = urllib2.urlopen(url)
    f = open(file_name, 'wb')
    meta = u.info()
    file_size = int(meta.getheaders("Content-Length")[0])
    print "Downloading: %s Bytes: %s" % (file_name, file_size)

    file_size_dl = 0
    block_sz = 8192
    while True:
        buffer = u.read(block_sz)
        if not buffer:
            break

        file_size_dl += len(buffer)
        f.write(buffer)
        status = r"%10d  [%3.2f%%]" % (file_size_dl, file_size_dl * 100. / file_size)
        status = status + chr(8)*(len(status)+1)
        print status,

    f.close()

使用第三方[requests](http://docs.python-requests.org/en/latest/index.html)包

    >>> import requests
    >>>
    >>> url = "http://download.thinkbroadband.com/10MB.zip"
    >>> r = requests.get(url)
    >>> print len(r.content)
    10485760

### argparse可选位置参数

问题 [链接](http://stackoverflow.com/questions/4480075/argparse-optional-positional-arguments)

脚本运行 usage: installer.py dir [-h] [-v]

dir是一个位置参数，定义如下

    parser.add_argument('dir', default=os.getcwd())

我想让dir变为可选，如果未设置，使用os.getcwd()

不幸的是，当我不指定dir时，得到错误 "Error: Too few arguments"

尝试使用 nargs='?'

    parser.add_argument('dir', nargs='?', default=os.getcwd())

例子

    >>> import os, argparse
    >>> parser = argparse.ArgumentParser()
    >>> parser.add_argument('-v', action='store_true')
    _StoreTrueAction(option_strings=['-v'], dest='v', nargs=0, const=True, default=False, type=None, choices=None, help=None, metavar=None)
    >>> parser.add_argument('dir', nargs='?', default=os.getcwd())
    _StoreAction(option_strings=[], dest='dir', nargs='?', const=None, default='/home/vinay', type=None, choices=None, help=None, metavar=None)
    >>> parser.parse_args('somedir -v'.split())
    Namespace(dir='somedir', v=True)
    >>> parser.parse_args('-v'.split())
    Namespace(dir='/home/vinay', v=True)
    >>> parser.parse_args(''.split())
    Namespace(dir='/home/vinay', v=False)
    >>> parser.parse_args(['somedir'])
    Namespace(dir='somedir', v=False)
    >>> parser.parse_args('somedir -h -v'.split())
    usage: [-h] [-v] [dir]

    positional arguments:
    dir

    optional arguments:
    -h, --help  show this help message and exit
    -v
### 有什么方法可以获取系统当前用户名么?

问题 [链接](http://stackoverflow.com/questions/842059/is-there-a-portable-way-to-get-the-current-username-in-python)

至少在Linux和Windows下都可用.就像 os.getuid

    >>> os.getuid()
    42
    >>> os.getusername()
    'slartibartfast'

可以看看 [getpass](http://docs.python.org/2/library/getpass.html) 模块

    >>> import getpass
    >>> getpass.getuser()
    'kostya'

可用: Unix, Windows

### 在Python中如何解析xml

问题 [链接](http://stackoverflow.com/questions/1912434/how-do-i-parse-xml-in-python)

    <foo>
    <bar>
        <type foobar="1"/>
        <type foobar="2"/>
    </bar>
    </foo>

 如何解析获取xml文件中内容

我建议使用 [ElementTree](http://docs.python.org/2/library/xml.etree.elementtree.html) (有其他可用的实现，例如 [lxml](http://lxml.de/)，他们只是更快, ElementTree提供更简单的编程api)

在使用XML建立Element实例之后，例如使用 [XML](http://docs.python.org/2/library/xml.etree.elementtree.html#xml.etree.ElementTree.XML) 函数

    for atype in e.findall('type')
        print(atype.get('foobar'))

### 如何使用Python创建一个GUID

问题 [链接](http://stackoverflow.com/questions/534839/how-to-create-a-guid-in-python)

如何使用原生的python创建一个GUID?

Python2.5及以上版本,使用[uuid](http://docs.python.org/2/library/uuid.html)模块

    >>> import uuid
    >>> uuid.uuid1()
    UUID('5a35a426-f7ce-11dd-abd2-0017f227cfc7')


### 我该使用urllib/urllib2还是requests库

问题 [链接](http://stackoverflow.com/questions/2018026/should-i-use-urllib-or-urllib2-or-requests)

[requests](http://docs.python-requests.org/en/latest/index.html)

推荐使用requests库

1. 支持restful API

        import requests
        ...

        resp = requests.get('http://www.mywebsite.com/user')
        resp = requests.post('http://www.mywebsite.com/user')
        resp = requests.put('http://www.mywebsite.com/user/put')
        resp = requests.delete('http://www.mywebsite.com/user/delete')

2. 无论GET/POST，你不需要关注编码

        userdata = {"firstname": "John", "lastname": "Doe", "password": "jdoe123"}
        resp = requests.post('http://www.mywebsite.com/user', params=userdata)

3. 内建json decoder

        resp.json()

        resp.text #纯文本

4. 其他 建官方文档

urllib2，提供了一些额外的函数，让你可以自定义request headers

    r = Request(url='http://www.mysite.com')
    r.add_header('User-Agent', 'awesome fetcher')
    r.add_data(urllib.urlencode({'foo': 'bar'})
    response = urlopen(r)

注意：urlencode只有urllib中有


### Python中是否存在方法可以打印对象的所有属性和方法

问题 [链接](http://stackoverflow.com/questions/192109/is-there-a-function-in-python-to-print-all-the-current-properties-and-values-of)

使用pprint

    from pprint import pprint
    pprint (vars(your_object))


### 如何展示一个正在运行的Python应用的堆栈踪迹

问题[链接](http://stackoverflow.com/questions/132058/showing-the-stack-trace-from-a-running-python-application)

我一般在这种情况下用这个模块-当一个进程会跑很长时间，但是有时被莫名其妙而且不重复出现的原因卡住。看上去有点黑客的感觉，并且只在unix上生效（依赖信号）：

    import code, traceback, signal

    def debug(sig, frame):
        """Interrupt running process, and provide a python prompt for
        interactive debugging."""
        d={'_frame':frame}         # Allow access to frame object.
        d.update(frame.f_globals)  # Unless shadowed by global
        d.update(frame.f_locals)

        i = code.InteractiveConsole(d)
        message  = "Signal received : entering python shell.\nTraceback:\n"
        message += ''.join(traceback.format_stack(frame))
        i.interact(message)

    def listen():
        signal.signal(signal.SIGUSR1, debug)  # Register handler

使用时，当你启动程序时，只需要调用listen()这个函数（你甚至可以放到site.py里，对所有Python程序都起效），让它运行起来。任何时候，都可以对进程发出一个SIGUSR1的信号，使用kill或者在Python中：

    os.kill(pid, signal.SIGUSR1)

这会使Python控制台程序在当时停止，并给你展示堆栈的踪迹，允许你操作所有的变量。使用control-d(EOF)继续运行（在发出信号的一刻，通过注释你可以打断任何I/O等等，所以它不是完全无害的）。

我还有一个可以做同样事情的脚本，唯一不同的是它可以和正在运行的进程通过pip互通（允许在后台进行程序排错等等）。粘贴到这可能太长了，我已经把它加进了[python cookbook recipe](http://code.activestate.com/recipes/576515/)。

### Python中isinstance()和type()的区别

总结一下其他人的答案（已OK的）作为内容。`isinstance`为继承者们服务（某个父类的实例也是基类的一个实例），而只检查是否相等的`type`则不是这样（他要求某个类型的id，拒绝子类型的实例，也可以叫做子类）

在Python中，通常你希望你的代码支持继承，当然（自从继承变得很容易之后，通过使用你自己的而不是使用它本身来停止某些代码是很不好的），所以`isinstance`看上去比检查id的`type`s要好一些，因为它无缝支持继承。

这不是说`isinstance`就绝对好，提醒一下，只是比对比类型是否相等好一点。通常从Pythonic的角度出发，最好的解决办法是永远的鸭子类型：试着像用一个渴望得到的类型去使用某个参数，用`try/except`声明捕获所有该参数不属于我们的渴望的类型而引起的异常（或者任何类鸭子类型；-），然后在`except`从句中，试着做一些其他的事情（假设这个参数“可能是”某些类型)

`basestring`时一个奇葩，这是一个存在的意义仅仅是为了让你使用`isinstance`的内建类型（同理他们的子类`str`和`Unicode`）。字符串是有序的（你可以循环，排序，切片...），但是通常你对待它们就像标准类型--由于某些原因它不太好用（但是我们相当频繁的使用他们），原因是我们对于所有的字符串（或者其他标准类型，比如一个不能回环的东西）都用同一种方法，所有的容器（列表，集合，字典...），另一方面`basestring`加上`isinstance`可以帮你做这种事情--这个习惯用法的全部结构看起来像这样：

    if isinstance(x, basestring)
        return treatasscalar(x)
    try:
        return treatasiter(iter(x))
    except TypeError:
        return treatasscalar(x)

你可能会说`basestring`是一个抽象类--在子类中没有提供具体的功能，更多的时候充当一个标记，为了让我们使用`isinstance`。从[PEP 3119](http://www.python.org/dev/peps/pep-3119/)开始，这种观念越来越明显，介绍了它的泛型，在Python2.6和3.0中被采用并实现。

PEP明确了抽象基类可以代替鸭子类型，而且实现起来没什么压力（看[这里](https://www.python.org/dev/peps/pep-3119/#abcs-vs-duck-typing)）。最新版本的Python中实现的抽象基类提供了额外的更吸引人的东西：`isinstance`（和`issubclass`)现在不仅仅是一个子类的实例（特别是所有的类都可以注册成为抽象基类并展示为某个子类，它可以实例化抽象基类的实例），抽象基类还为具体的子类提供了更多的便利-自然的通过模板方法来设计模仿应用。（看[这里](http://en.wikipedia.org/wiki/Template_method_pattern)和[这里](http://www.catonmat.net/blog/learning-python-design-patterns-through-video-lectures/)（第三部分）了解Python中特殊不特殊的更多TM DP，以及抽象基类的支持）。

对于那些Python 2.6中潜在的关于抽象基类的技巧，看[这里](https://docs.python.org/2/library/abc.html)，对于3.1版本，同样，看[这里](https://docs.python.org/3.1/library/abc.html)。两种版本的基本库模块[collections](https://docs.python.org/3.1/library/collections.html#module-collections)（这是3.1版本，几乎一样的2.6版本，看[这里](https://docs.python.org/2/library/collections.html#module-collections)）都提供了多种有用的抽象基类。

这个答案的目的是，抽象基类保留的那些关键东西（我们可以超过论证横向比较TM DP的功能性，和Python传统的mixin类，比如[UserDict.DictMixin](https://docs.python.org/2/library/userdict.html#UserDict.DictMixin)）是他们让`isinstance`(和`issubclass`)比之前（Python2.5及之前）更有魅力且无处不在（在Python2.6以及更新的版本中）。因此，相比之下，在Python中检查类型相等更恶心了，比之前的版本还糟糕。