
### 如何使用 pip 更新所有包

问题 [链接](http://stackoverflow.com/questions/2720014/upgrading-all-packages-with-pip)

如何使用pip更新python的所有包

没有内置的标志可以实现

但是你可以这么做

    pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs pip install -U

### 如何删除Python easy_install安装的包

问题 [链接](http://stackoverflow.com/questions/1231688/how-do-i-remove-packages-installed-with-pythons-easy-install)

[pip](https://pypi.python.org/pypi/pip/), setuptools/easy_install的另一种选择，提供uninstall命令

首先，移除依赖

    $ easy_install -m [PACKAGE]

然后，手动删除egg文件

    $ rm -rf .../python2.X/site-packages/[PACKAGE].egg

### 如何获取Python的site-packages目录位置

问题 [链接](http://stackoverflow.com/questions/122327/how-do-i-find-the-location-of-my-python-site-packages-directory)

参考 [How to Install Django" documentation](http://docs.djangoproject.com/en/dev/topics/install/#remove-any-old-versions-of-django)

可以在shell中执行

    python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"

更好的可读性

    from distutils.sysconfig import get_python_lib
    print(get_python_lib())

### setup.py安装后如何卸载

问题 [链接](http://stackoverflow.com/questions/1550226/python-setup-py-uninstall)


使用下面命令安装的包如何卸载

    python setup.py install

手工删除的话

    python setup.py install --record files.txt
    cat files.txt | xargs rm -rf

### 如何获取安装的python模块列表

问题 [链接](http://stackoverflow.com/questions/739993/get-a-list-of-installed-python-modules)

    >>> help('modules')
