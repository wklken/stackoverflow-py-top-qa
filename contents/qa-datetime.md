
### 如何将一个Python time.struct_time对象转换为一个datetime对象

问题 [链接](http://stackoverflow.com/questions/1697815/how-do-you-convert-a-python-time-struct-time-object-into-a-datetime-object)

使用 [time.mktime()]() 将time元组(本地时间)转成秒， 然后使用 datetime.fromtimestamp() 转成datetime对象

    from time import mktime
    from datetime import datetime

    dt = datetime.fromtimestamp(mktime(struct))

### python中如何获取当前时间

问题 [链接](http://stackoverflow.com/questions/415511/how-to-get-current-time-in-python)

时间日期

    >>> import datetime
    >>> datetime.datetime.now()
    datetime(2009, 1, 6, 15, 8, 24, 78915)

如果仅获取时间

    >>> datetime.datetime.time(datetime.datetime.now())
    datetime.time(15, 8, 24, 78915))
    #等价
    >>> datetime.datetime.now().time()

可以从文档中获取更多 [文档](http://docs.python.org/2/library/datetime.html)

如果想避免额外的datetime.

    >>> from datetime import datetime
