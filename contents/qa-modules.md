
### \_\_init\_\_.py是做什么用的

问题 [链接](http://stackoverflow.com/questions/448271/what-is-init-py-for)


这是包的一部分，[具体文档](http://docs.python.org/2/tutorial/modules.html#packages)

\_\_init\_\_.py让Python把目录当成包，

最简单的例子，\_\_init\_\_.py仅是一个空文件，但它可以一样执行包初始化代码或者设置\_\_all\_\_变量，后续说明

### 如何使用绝对路径import一个模块

问题 [链接](http://stackoverflow.com/questions/67631/how-to-import-a-module-given-the-full-path)


    import imp

    foo = imp.load_source('module.name', '/path/to/file.py')
    foo.MyClass()

###  获取Python模块文件的路径

问题 [链接](http://stackoverflow.com/questions/247770/retrieving-python-module-path)

如何才能获取一个模块其所在的路径

回答

    import a_module
    print a_module.__file__

获取其所在目录，可以

    import os
    path = os.path.dirname(amodule.__file__)
