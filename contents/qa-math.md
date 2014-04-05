
### 在Python中如何展示二进制字面值

问题 [链接](http://stackoverflow.com/questions/1476/how-do-you-express-binary-literals-in-python)

十六进制可以

    >>> 0x12AF
    4783
    >>> 0x100
    256

八进制可以

    >>> 01267
    695
    >>> 0100
    64

二进制如何表示？

Python 2.5 及更早版本: 可以表示为 int('01010101111',2)  但没有字面量

Python 2.6 beta: 可以使用0b1100111 or 0B1100111 表示

Python 2.6 beta: 也可以使用 0o27 or 0O27 (第二字字符是字母 O)

Python 3.0 beta: 同2.6，但不支持027这种语法
### 如何将一个十六进制字符串转为整数

问题 [链接](http://stackoverflow.com/questions/209513/convert-hex-string-to-int-in-python)

    >>> int("a", 16)
    10
    >>> int("0xa",16)
    10


详细 [文档](http://docs.python.org/2/library/sys.html)

### 如何强制使用浮点数除法

问题 [链接](http://stackoverflow.com/questions/1267869/how-can-i-force-division-to-be-floating-point-in-python)

如何强制使除法结果c是浮点数

    c = a / b
可以使用__future__

    >>> from __future__ import division
    >>> a = 4
    >>> b = 6
    >>> c = a / b
    >>> c
    0.66666666666666663

或者转换,如果除数或被除数是浮点数，那么结果也是浮点数

    c = a / float(b)

### 如何将一个整数转为字符串

问题 [链接](http://stackoverflow.com/questions/961632/converting-integer-to-string-in-python)

回答

    >>> str(10)
    '10'
    >>> int('10')
    10

对应文档 [int()](http://docs.python.org/2/library/functions.html#int) [str()](http://docs.python.org/2/library/functions.html#str)



