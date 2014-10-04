

### 应该在学习Python3之前学习Python2，还是直接学习Python3

问题 [链接](http://stackoverflow.com/questions/170921/should-i-learn-python-2-before-3-or-start-directly-from-python-3)

你可以从python2开始，2和3主要的语法格式和风格相同

3要替代2不是短时间内能完成的，将会是一个很长的过程，所以学习Python2并没有什么坏处

我建议你关注下2和3的不同之处  [This slides gives you a quick introduction of the changes in Python 2 and 3](http://stackoverflow.com/questions/170921/should-i-learn-python-2-before-3-or-start-directly-from-python-3)





### 在virtualenv中如何使用不同的python版本

问题 [链接](http://stackoverflow.com/questions/1534210/use-different-python-version-with-virtualenv)

在创建virtualenv实例时，使用-p选项

    virtualenv -p /usr/bin/python2.6 <path/to/new/virtualenv/>

### 如何离开virtualenv

问题 [链接](http://stackoverflow.com/questions/990754/how-to-leave-a-python-virtualenv)

使用virtualenv时

    me@mymachine:~$ workon env1
    (env1)me@mymachine:~$ workon env2
    (env2)me@mymachine:~$ workon env1
    (env1)me@mymachine:~$

如何退出某个环境

    $ deactivate

### Python中什么项目结构更好

问题 [链接](http://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application)

假设你要开发一个较大的客户端程序(非web端),如何组织项目目录和递归？


不要太在意这个.按你高兴的方式组织就行.Python项目很简单，所以没有那么多愚蠢的规则

    /scripts or /bin  命令行脚本
    /tests 测试
    /lib C-语言包
    /doc 文档
    /apidoc api文档

并且顶层目录包含README和Config

难以抉择的是，是否使用/src树. /src,/lib,/bin在Python中没有明显的区别，和Java/c不同

因为顶层/src文件夹显得没有什么实际意义，你的顶层目录可以是程序顶层架构的目录

    /foo
    /bar
    /baz

我建议将这些文件放入到"模块名"的目录中，这样，如果你在写一个应用叫做quux, /quux目录将包含所有这些东西

你可以在PYTHONPATH中加入 /path/to/quux/foo,这样你可以QUUX.foo中重用模块


另一个回答

    Project/
    |-- bin/
    |   |-- project
    |
    |-- project/
    |   |-- test/
    |   |   |-- __init__.py
    |   |   |-- test_main.py
    |   |
    |   |-- __init__.py
    |   |-- main.py
    |
    |-- setup.py
    |-- README


### 在Python中使用Counter错误

问题 [链接](http://stackoverflow.com/questions/13311094/counter-in-collections-module-python)

当使用Counter时，出现异常

    AttributeError: 'module' object has no attribute 'Counter'

    from collections import Counter
    ImportError: cannot import name Counter

原因：

版本问题，Counter在 python2.7中才被加入到这个模块，你可能使用了Python2.6或更老的版本

可以看下 [文档](http://docs.python.org/2/library/collections.html#collections.Counter)

如果要在 Python2.6或2.5版本使用，可以看 [这里](http://code.activestate.com/recipes/576611-counter-class/)


### 在Python中如何连接mysql数据库

问题 [链接](http://stackoverflow.com/questions/372885/how-do-i-connect-to-a-mysql-database-in-python)

首先，安装mysqldb

然后

    #!/usr/bin/python
    import MySQLdb

    db = MySQLdb.connect(host="localhost", # your host, usually localhost
                         user="john", # your username
                          passwd="megajonhy", # your password
                          db="jonhydb") # name of the data base

    # you must create a Cursor object. It will let
    #  you execute all the queries you need
    cur = db.cursor()

    # Use all the SQL you like
    cur.execute("SELECT * FROM YOUR_TABLE_NAME")

    # print all the first cell of all the rows
    for row in cur.fetchall() :
        print row[0]

### Python框架flask和bottle有什么区别

问题 [链接](http://stackoverflow.com/questions/4941145/python-flask-vs-bottle)

区别：flask基于其他现有的存在很长一段时间的技术像Werkzeug和Jinja2，它没有尝试去重复发明轮子

另外一方面，Bottle，试图用一个文件解决一切.

我想去合并他们，但是Bottle的开发者貌似对偏离“一个文件”的处理方式不是很感兴趣

关于可扩展性： 可以使用其他模板引擎，例如Flask-Genshi使用了mako模板

Flask, Werkzeug and Jinja2 的开发者亲自回答的...碉堡了，翻译不是很准确


### 如何测试一个python脚本的性能

问题 [链接](http://stackoverflow.com/questions/582336/how-can-you-profile-a-python-script)


引入

    import cProfile
    cProfile.run('foo()')

执行脚本

    python -m cProfile myscript.py

结果

    1007 function calls in 0.061 CPU seconds

    Ordered by: standard name
    ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.061    0.061 <string>:1(<module>)
    1000    0.051    0.000    0.051    0.000 euler048.py:2(<lambda>)
        1    0.005    0.005    0.061    0.061 euler048.py:2(<module>)
        1    0.000    0.000    0.061    0.061 {execfile}
        1    0.002    0.002    0.053    0.053 {map}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler objects}
        1    0.000    0.000    0.000    0.000 {range}
        1    0.003    0.003    0.003    0.003 {sum}

一个PyCon演讲 [入口](http://blip.tv/pycon-us-videos-2009-2010-2011/introduction-to-python-profiling-1966784)


### 如何获取5分钟之后的unix时间戳

问题 [链接](http://stackoverflow.com/questions/2775864/python-create-unix-timestamp-five-minutes-in-the-future)

使用 [calendar.timegm](http://docs.python.org/3.3/library/calendar.html#calendar.timegm)

    future = datetime.datetime.now() + datetime.timedelta(minutes = 5)
    return calendar.timegm(future.utctimetuple())

strftime的%s在windows中无法使用



### 在python中如何调用外部命令?

问题 [链接](http://stackoverflow.com/questions/89228/calling-an-external-command-in-python) 

Look at the subprocess module in the stdlib:

可以看下标准库中的 [subprocess](http://docs.python.org/library/subprocess.html)

    from subprocess import call
    call(["ls", "-l"])

subprocess相对于system的好处是, 更灵活

但是 quick/dirty/one time scripts, os.system is enough

