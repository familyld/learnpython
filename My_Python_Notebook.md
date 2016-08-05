#Python学习笔记
##数据类型和变量
###数据类型：
***
Python可以直接表达的数据类型包括：整数，浮点数，复数，字符串，布尔值和空值。

* 整数

    任意大小，正负皆可。并且可以用0x和0o分别表示十六进制数和八进制数。

* 浮点数

    可使用科学记数法，即1.23x10^9就是1.23e9 ，0.000012可以写成1.2e-5，等等。

    整数运算永远是精确的(结果也是整数)，浮点数运算则有四舍五入的误差。

    Python提供两种除法+求余运算：
    * 【 **/** 】 结果为浮点数，即使两数整除，结果也是浮点数。
    * 【 **//** 】 结果为整数，也称地板除，对结果向下取整。
    * 【 **%** 】 结果为整数，用于计算余数。

* 复数

    Python中复数用j表示虚数部分，比如说 `x=2.4 + 5.6j`， 并且可以通过复数的类属性 `real` 取得实部，类属性 `imag` 取得虚部，通过类方法 `conjugate()` 获得共轭复数。

* 字符串

    使用单引号或双引号括起。而引号本身需要使用转义符【 \ 】来表达。 用【 \' 】来表示【 ' 】。

    转义符还可以用来转义很多字符，如【 **\n** 】表示换行。【 **\t** 】表示制表符。 【 \ 】本身也需要转义用 **双斜杠** 来代替。

    如果一个字符串中很多字符都要转义就会很麻烦，所以Python又提供一种简便的写法，【 **r''** 】表示两个引号之间的内容是不需要转义的。 对于多行的字符串，为了避免加入【 **\n** 】的不方便，可以使用【 ’’’…’’’ 】的格式，即用三个引号来括起字符串，这样字符串的换行会被保留。

* 布尔值

    只有True和False两种，首字母大写。如果输入一个判断式，则Python会给出布尔值的结果。比方说输入3>2，运行后会得到True。 对于布尔值有三个逻辑运算符，分别是**and**，**or**和**not**。 本质上True用1存储，False用0存储。

* 空值

    用**None**表示，不同于数值0。比方说字符串【 '' 】就是空值。

###变量：
***
Python变量名不可以以数字开头，构成可以包括大小写英文，数字及下划线。变量没有固定类型，可以把任意数据类型复制给一个变量并反复赋值。因此Python是一种动态语言。

当我们写：a=’ABC’ 时，Python解释器干了两件事情：

1. 在内存中创建了一个'ABC'的字符串；
2. 在内存中创建了一个名为a的变量，并把它指向'ABC'。

###常量：
***
Python中没有常量，因为变量都可以重复赋值。但是一般约定，用全部字母大写的单词来表示一个常量，如：PI=3.14。

##字符串和编码
###字符编码：
***
在计算机存储中内存统一使用Unicode编码，而硬盘保存或者传输数据就用UTF-8编码。比方说打开记事本时，数据会从硬盘中UTF-8编码格式转换为Unicode格式，保存时则相反。

浏览网页时，服务器端动态生成的内容是Unicode编码，而传输时则会变为传输UTF-8编码格式的网页。所以常常在网页源码中看到<meta charset="UTF-8" />的语句。

* ASCII

    最早的编码，包含大小写英文，数字及一些符号。大写A编码为65，小写a编码为97。字符0编码为48。使用8位二进制组合(1个字节)，可表示128个不同字符(最高位用于校验)。
* GB2312

    处理中文时一个字节显然不足，至少需要用两个字节，并且不与ASCII冲突，因为有了中国自己制定的GB2312编码，可用于编码中文。
* Shift_JIS和Euc-kr

    日本编码为Shift_JIS，韩文编码为Euc-kr。这样各个国家都用自己不同的标准和编码就很容易在多语言的环境产生冲突，变成乱码。因此又有了通用的一套编码。
* Unicode

    Unicode将所有语言统一到一套编码中，一般使用2个字节，部分非常偏僻的字符会用到4个字节。但是使用Unicode编码，如果在大量英文的环境又非常浪费空间，因为存储和传输时大小是使用ASCII编码的两倍，不划算，于是又有了新的方式。
* UTF-8

    UTF-8是可变长编码，对Unicode字符不同的数字大小编码成1-6个字节，英文字母为1个字节，汉字一般是3个字节。需要用到4-6字节的字符非常少，这样比较节省空间。

**Example**：

| 字符 | ASCII | Unicode  | UTF-8 |
|:------:|:------:|:------:|:------:|
| A | 01000001 | 00000000 01000001 | 01000001 |
| 中 | (超出范围) | 01001110 00101101 | 11100100 10111000 10101101 |

###字符串：
***
Python字符串采用Unicode编码，支持多语言。

* ord()和chr()

    ord函数可以获取字符的十进制整数表示，chr函数可以将十进制整数转换为Unicode编码下对应的字符。此外使用【\u】转义也表示这是一个Unicode字符。

    比如：【ord(‘中’)】结果是【20013】，而【chr(20013)】结果是【’中’】，并且20013的十六进制表示是0x4e2d，因此使用【’\u4e2d’】得到的结果也是【’中’】。

* bytes

    因为Python中字符串用Unicode编码，一个字符对应若干字节，要在网络中传输或存储到硬盘中就要把字符串变为以bytes类型。

    Python显示bytes类型的数据会用b作前缀，如 **b'ABC'**，注意和 **'ABC'** 进行区分，尽管内容一样，但前者的每个字符都只占1个字节，而后者在Python中以Unicode进行编码，每个字符占两个字节。

    Unicode字符串可以通过【 encode() 】方法编码为指定的bytes，如ASCII编码，utf-8编码etc，bytes类型的数据如果字符不属于ASCII码的范围，就用【 **'\x##** 】的格式表示。 相应地，如果读取字节流的数据，就要用【 decode() 】方法解码。 此外，还可以用【 len() 】方法来计算字符串的长度，对bytes类型使用的话则得到字节数。

**例子**：

    >>> 'ABC'.encode('ascii')
    b'ABC'
    >>> '中文'.encode('utf-8')
    b'\xe4\xb8\xad\xe6\x96\x87'
    >>> b'ABC'.decode('ascii')
    'ABC'
    >>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
    '中文'

###格式化：
***
和C语言类似，用【 ％ 】占位符实现。【 ％s 】表示用字符串替换，【 ％d 】，【 ％f 】，【 ％x 】分别表示用整数，浮点数和十六进制数替换。其中【 ％s 】可以将任意数据类型转换为字符串。【 ％d 】和【 ％f 】可以指定整数和小数部分的位数。

**例子**：

    >>> '%2d-%02d' % (3, 1)
    ' 3-01'
    >>> '%.2f' % 3.1415926
    '3.14'

***notice***：

* 只有一个占位符时，后面变量不需要用括号括起。
* 如果字符串本身包含【 ％ 】，则需要转义，用【 ％％ 】来表示百分号。

###格式化：
***
和C语言类似，用【 ％ 】占位符实现。【 ％s 】表示用字符串替换，【 ％d 】，【 ％f 】，【 ％x 】分别表示用整数，浮点数和十六进制数替换。其中【 ％s 】可以将任意数据类型转换为字符串。【 ％d 】和【 ％f 】可以指定整数和小数部分的位数。

**例子**：

    >>> '%2d-%02d' % (3, 1)
    ' 3-01'
    >>> '%.2f' % 3.1415926
    '3.14'

***notice***：

* 只有一个占位符时，后面变量不需要用括号括起。
* 如果字符串本身包含【 ％ 】，则需要转义，用【 ％％ 】来表示百分号。

##使用list和tuple
### list
***
list是一种Python内置的数据类型，表示有序集合，可动态删除和插入。通过索引可以访问列表元素，索引从0开始，即访问第一个列表元素。并且列表是循环的，可以通过索引－1访问最尾的元素，索引－2访问倒数第二个元素。

**例子**：

    >>> classmates = ['Michael', 'Bob', 'Tracy']
    >>> classmates
    ['Michael', 'Bob', 'Tracy']
    >>> classmates[0]
    'Michael'

另外，还可以用「len()」函数获取列表的元素个数。

list 是一个可变的有序表，所以，可以往 list 中追加元素到末尾：

    >>> classmates.append('Adam')
    >>> classmates
    ['Michael', 'Bob', 'Tracy', 'Adam']

也可以把元素插入到指定的位置，比如索引号为 1 的位置：

    >>> classmates.insert(1, 'Jack')
    >>> classmates
    ['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']

要删除 list 末尾的元素，用 pop() 方法：

    >>> classmates.pop()
    'Adam'
    >>> classmates
    ['Michael', 'Jack', 'Bob', 'Tracy']

要删除指定位置的元素，用 pop(i) 方法，其中 i 是索引位置：

    >>> classmates.pop(1)
    'Jack'
    >>> classmates
    ['Michael', 'Bob', 'Tracy']

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：

    >>> classmates[1] = 'Sarah'
    >>> classmates
    ['Michael', 'Sarah', 'Tracy']

list 里面的元素的数据类型也可以不同，比如：

    >>> L = ['Apple', 123, True]

list 元素也可以是另一个 list，比如：

    >>> s = ['python', 'java', ['asp', 'php'], 'scheme']
    >>> len(s)
    4

要注意s只有4个元素，s[2]作为一个list类型的元素。要拿到'php'可以用s[2][1]，即把s看作一个二维数组，这样的嵌套可以有很多层。

如果一个 list 中一个元素也没有，就是一个空的 list，它的长度为 0：

    >>> L = []
    >>> len(L)
    0

    ### tuple
***
tuple也是一种有序列表，但tuple一旦初始化就无法再修改，也因为这个特性，所以代码更安全。和list不同，tuple用小括号来括起。

**例子**：

    >>> classmates = ('Michael', 'Bob', 'Tracy')
    定义空的tuple如下如下：
    >>> t = ()
    >>> t
    ()

但是，要定义一个只有1个元素的 tuple，就要注意一下，如果使用t = (1)来定义，则得到的不是一个tuple，而是整数1，因为括号既可以表示tuple又可以表示数学公式中的小括号。这种情况下默认为后者。要定义1个元素的tuple格式如下，使用一个逗号进行区分：

    >>> t = (1,)
    >>> t
    (1,)

tuple也有「可变」的例子，如果tuple的其中一个元素是list，则这个list元素的内容是可以修改的，如下：

    >>> t = ('a', 'b', ['A', 'B'])
    >>> t[2][0] = 'X'
    >>> t[2][1] = 'Y'
    >>> t
    ('a', 'b', ['X', 'Y'])

这个实际修改的是list而不是tuple，tuple指向位置不会变，而list指向的位置可变，上面的例子实质是创建了字符串X和Y，然后让tuple的第三个元素即list元素指向这两个新的字符串。

##条件判断
### 条件判断
***
Python中代码块是以缩进作区分的，if－else条件判断注意要在判定条件后写冒号，并且代码块都正确对齐。多个判断条件可以使用多个elif来实现。

**例子**：

    age = 20
    if age >= 6:
        print('teenager')
    elif age >= 18:
        print('adult')
    else:
        print('kid')

判断条件并不一定要是一个判断式，可以简写为一个变量，当变量为非零数值，非空字符串，非空list等时，判断为True，否则为False。

### 再议input
***
有时会采取【 **input('提示语句')** 】的方式读取用户输入，作为判定条件。要注意用户输入的是字符串，要进行数值比较必须先转换为对应的数据类型，否则会报错。

Python提供int(), float(), str()等方法进行数据类型的转换。

### 循环
***
Python提供两种循环写法，一种是 **for...in** 循环，一种是 **while** 循环。for循环依次把list或tuple中的每个元素赋值给目标标识符，格式如下：


    names = ['Michael', 'Bob', 'Tracy']
    for name in names:
        print(name)

当列表为连续数值时，可以用range方法生成，格式如下：

    sum = 0
    for x in range(101):
        sum = sum + x
    print(sum)

while循环的写法和if－else很相似，也是在判定条件后面加一个冒号。

**例子**：

    sum = 0
    n = 99
    while n > 0:
        sum = sum + n
        n = n - 2
    print(sum)

另外，对于死循环的程序，可以通过***Ctrl＋C***强制终止。

##使用dict和set
###dict
***
dict即字典，英语存储键值对，查找速度极快。如果使用list来存键值对就需要两个list，要先从key list找出key，再从value list找到对应项的值，因此list越长，耗时越长。用dict实现，可以直接根据key来找value。格式如下：

    >>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
    >>> d['Michael']
    95

dict速度快是因为Python内部像字典一样建立了索引，字典有部首表，Python内部也会根据不同key算出一个存放的「页码」(哈希算法)，所以速度非常快。出了初始化赋值还可以对一个key多次赋值，会进行覆盖，如果key不存在就会对dict插入一个新的键值对：

    >>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
    >>> d['Michael']
    95

要判断key是否在dict里面有两种方法：

1.使用in关键字，有则返回True，无则返回False

    >>> 'Thomas' in d
    False

2.使用dict提供的get方法，有则返回key对应的value，无则返回空值None或者自己指定的值。

    >>> d.get('Thomas')
    None
    >>> d.get('Thomas', -1)
    -1

删除一个key则对应的value也会从dict中删除，使用pop方法来实现：

    >>> d.pop('Bob')
    75

dict的插入和查找速度极快，不回随着key的增加而增加，但需要占用大量的内存，内存浪费多。list则相反，插入和查找时间随元素增加而增加，但占用空间少。所以dict是一种用空间换时间的方法，注意dict的key是不可变对象，无法修改key，不然dict就混乱了。字符串和整数等都可以作为key，list无法作为key。

###set
***
set和dict的原理是一样的，同样不可以放入可变对象做key，唯一的区别是set是只有key没有value。显然set里面是没有重复的元素的，不然哈希会出错。set是有序的，需要使用列表做初始化，定义方式如下：

    >>> s = set([1, 2, 3])
    >>> s
    {1, 2, 3}

列表中有重复元素时set会自动被过滤，添加可以使用add方法，如s.add(4)。删除则用remove方法，如s.remove(4)。

集合可以看成数学意义上无序和无重复的集合，可以做交集，并集等操作。

    >>> s1 = set([1, 2, 3])
    >>> s2 = set([2, 3, 4])
    >>> s1 & s2
    {2, 3}
    >>> s1 | s2
    {1, 2, 3, 4}

###再议不可变对象
***
Python中一切皆对象，而对象又分可变和不可变两类，具体来说就是对象的内容可变不可变。不可变对象包括int, float, string, tuple，空值等，可变对象包括list, dict, set等。

* 当一个可变对象作为函数参数时，函数内对参数修改会生效。
* 当一个不可变对象作为函数参数时，函数内对参数修改无效，因为实际上函数是创建一个新的对象并返回而已，没有修改传入的对象。

**例子**：

    >>> a = ['c', 'b', 'a']
    >>> a.sort()
    >> a
    ['a', 'b', 'c']

list是可变对象，使用函数操作，内容会变化，注意区分一下**变量**和**对象**，这里a是一个变量，它指向一个列表对象，这个列表对象的内容才是['c', 'b', 'a']。

**例子**：

    >>> a = 'abc'
    >>> b = a.replace('a', 'A')
    >>> b
    'Abc'
    >>> a
    'abc'
字符串是不可变对象，所以replace函数并不会修改变量a指向的对象，实际上调用不可变对象的任意方法都不改变该对象自身的内容，只会创建新对象并返回，如果使用一个新的变量来接收返回的对象，就能得到操作结果。不接收则直接打印，原变量指向的对象不会变化。

**Notice:** 虽然tuple是不可变对象，但是如果使用tuple作为dict或者set的key，还是有可能产生错误，因为tuple允许元素中包含列表，列表内容可变。如果使用了带有列表元素的tuple作为key就会报【 ***TypeError: unhashable type: 'list'*** 】的错误。

##注释
Python中的注释分为单行注释和多行注释。中文注释必须额外进行声明。

###单行注释
***
单行注释使用 ***#*** 号，***#*** 号右边内容在代码执行时会被忽略。

###多行注释
***
多行注释使用 ***'''*** 号，用三个引号来括起所有注释内容，单引号双引号都可以如：

    '''
    这是一行注释。
    这是一行注释。
    这是一行注释。
    '''
    """
    这是一行注释。
    这是一行注释。
    这是一行注释。
    """

###中文注释
当Python代码涉及中文时，需要在文件开头声明保存编码的格式，否则会默认使用ASCII码保存中文，这样会产生乱码。

在文件开头加上 ```#coding=utf-8``` 或者 ```#coding=gbk```就可以了。

##函数
###调用函数
***
Python支持函数，不仅可以灵活地自定义函数，而且本身也内置了很多有用的函数。

使用help(函数名)可以查看内置函数(built-in function, BIF)的使用方法和用途。 还可以直接查看[官方文档](https://docs.python.org/3/library/functions.html#abs)。

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”。 这里#号后面的内容是***注释***。

    >>> a = abs # 变量a指向abs函数
    >>> a(-1) # 所以也可以通过a调用abs函数
    1

###定义函数
***
定义函数要使用**def**语句，依次写出*函数名*、*括号*、*括号中的参数*和*冒号*:，然后，在缩进块中编写函数体，函数的返回值用**return**语句返回。

**例子**：

    def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x

注意，如果没有return语句，则函数执行完毕也会返回**None**，如果想要函数返回**None**，除了写 `return None` 之外还可以直接写 `return`。

可以直接在命令行定义函数，也可以把函数放在py文件中定义。若采用后者则使用函数时要先把工作目录跳转到文件保存的目录，再启动Python，然后用 `from 文件名 import 函数名` 即可导入函数。(这里文件名不需要包含文件扩展名.py)

###空函数
***
如果还没想好怎么写一个函数，可以用pass语句来实现一个空函数，如：

    def nop():
        pass
pass语句什么都不做，但可以用来做占位符。用在其他语句中也可以，如：

    if age >= 18:
        pass

###参数检查
***
当参数个数不对时，Python解释器会抛出***TypeError***错误，但是当参数类型错误时，如果函数里面没有给出对应的方法，Python解释器就无法抛出正确的错误提示信息。

上面实现的my_abs函数还不够完善，使用Python的内置函数isinstance()和raise语句来实现类型检查并报错的功能，如下：

    def my_abs(x):
        if not isinstance(x, (int, float)):
            raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x

###返回多个值
***
举一个返回坐标点的例子：

    import math

    def move(x, y, step, angle=0):
        nx = x + step * math.cos(angle)
        ny = y - step * math.sin(angle)
        return nx, ny

这里用到math包的函数cos和sin，返回坐标点的两个维度的值。接收时：

    >>> x, y = move(100, 100, 60, math.pi / 6)
    >>> print(x)
    151.96152422706632
    >>> print(y)
    70.0

或者：

    >>> r = move(100, 100, 60, math.pi / 6)
    >>> print(r)
    (151.96152422706632, 70.0)

实际上Python函数返回的仍然是单一值，它会返回一个 **tuple**，而语法上返回一个tuple可以省略括号。 并且我们可以使用多个变量来接收一个tuple，按位置来赋对应的值。

##函数的参数
定义函数的时候，我们把参数的名字和位置确定下来，函数的接口定义就完成了。

Python的函数定义非常简单，但灵活度却非常大。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数。

###位置参数
***
传入值按位置顺序依次赋给参数。

    def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

如这个幂函数，调用时使用 `power(5,2)` 这样的格式即可，5和2会按位置顺序赋给变量x和n。

###默认参数
***
比方说我们希望使用幂函数默认计算平方，不需要传入参数n。 这时就可以使用默认参数来实现。

    def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

这样幂函数就不必指定n，用 `power(5)` 的格式也能调用，计算5的平方。

**Notice：**

1. 必选参数在前，默认参数在后，否则Python的解释器会报错。
2. 有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。

**例子**：

    def enroll(name, gender, age=6, city='Beijing'):
        print('name:', name)
        print('gender:', gender)
        print('age:', age)
        print('city:', city)

这个例子中age和city是默认参数，调用时可以不提供。并且提供默认参数时既可以按顺序也可以不按顺序：

    enroll('Bob', 'M', 7)
    enroll('Adam', 'M', city='Tianjin')

按顺序不需指定参数名，不按顺序时则必须提供参数名，这样其他未提供的参数依然使用默认参数的值。

注意默认参数必须***指向不可变对象***！举个例子：

    def add_end(L=[]):
        L.append('END')
        return L

多次使用默认参数时：

    >>> add_end()
    ['END']
    >>> add_end()
    ['END', 'END']
    >>> add_end()
    ['END', 'END', 'END']

可以看到这里默认参数的内容改变了，因为L是可变对象，每次调用add_end，函数会修改默认参数的内容。 所以切记默认参数要指向不可变对象，要实现同样的功能，使用None就可以了。

    def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L

使用不可变对象做参数，在多任务环境下读取对象不需要加锁，同时读没有问题。因此能使用不可变对象就尽量用不可变对象。

###可变参数
***
可变参数即传入的参数个数可变，传入任意个参数都可以 。
先看一个例子：

    def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

这个求和函数只有一个参数，必须是一个list或者tuple才行，即 `calc([1, 2, 3，7])` 或者 `calc((1, 3, 5, 7))`。如果使用可变参数，则：

    def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

只是在参数前面加了一个 **\*** 号，函数内容不需要改变。这样定义的函数可以使用任意个数的参数，包括0个。

    >>> calc(1, 2)
    5
    >>> calc()
    0

传入参数时不需要构建list或者tuple，函数接收参数时会自动构建为一个**tuple**。 如果已经有一个list或者tuple要调用可变参数也很方便，将它变成可变参数就可以了。

    >>> nums = [1, 2, 3]
    >>> calc(*nums)
    14

同样只需要加一个 **\*** 号即可完成转换。

###关键字参数
***
可变参数允许传入0个或任一个参数，这些可变参数会自动组装为一个tuple。 而关键字参数允许传入0个或任意个**含参数名**的参数，这些关键字参数会自动组装为一个**dict**。

    def person(name, age, **kw):
        print('name:', name, 'age:', age, 'other:', kw)

调用时：

    >>> person('Michael', 30)
    name: Michael age: 30 other: {}
    >>> person('Bob', 35, city='Beijing')
    name: Bob age: 35 other: {'city': 'Beijing'}
    >>> person('Adam', 45, gender='M', job='Engineer')
    name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

kw是一个dict类型的对象，没有传入时就是一个空的dict。 和可变参数类似，先组装一个dict然后再传入也是可以的。

    >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
    >>> person('Jack', 24, city=extra['city'], job=extra['job'])
    name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}

或者进行转换：

    >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
    >>> person('Jack', 24, **extra)
    name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}

注意这里使用 **\*****\*** 转换的实质是把extra拷贝一份，然后令kw指向这个拷贝，所以函数内的操作不会对函数外的extra有任何影响。

###命名关键字参数
***
关键字参数的自由度很大，但有时我们需要限制用户可以传入哪些参数，这时就需要用到命名关键字参数。

    def person(name, age, *, city, job):
        print(name, age, city, job)

和关键字参数不同，这里采用一个 **\*** 号作为分隔符，**\*** 号后面的参数被视为关键字参数。 调用如下：

    >>> person('Jack', 24, city='Beijing', job='Engineer')
    Jack 24 Beijing Engineer

错误举例：
1.没有给参数名

    >>> person('Jack', 24, 'Beijing', 'Engineer')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: person() takes 2 positional arguments but 4 were given

命名关键字参数必须传入参数名，如果没有参数名，Python解释器会视其为位置参数，从而报参数个数超出的错误。

2.没有传入参数

    >>> person('Jack', 24)
    Traceback (most recent call last):
      File "<pyshell#83>", line 1, in <module>
    person('Jack', 24)
    TypeError: person() missing 2 required keyword-only arguments: 'city' and 'job'

命名关键字参数若没有定义默认值则被视为必选参数。 可以为命名关键字参数设置默认值， 比如 `def person(name, age, *, city='Beijing', job):`，这样即使不传入也不会报错了。

3.传入没有定义的参数

    >>> person('Jack', 24, city='Beijing', joc='Engineer')
    Traceback (most recent call last):
      File "<pyshell#84>", line 1, in <module>
    person('Jack', 24, city='Beijing', joc='Engineer')
    TypeError: person() got an unexpected keyword argument 'joc'

命名关键字参数限制了可以传入怎样的参数，如果传入参数的参数名不在其中也会报错。

###参数组合
***
在Python中定义函数除了可变参数和命名关键字参数无法混合，可以任意组合必选参数、默认参数、可变参数、关键字参数和命名关键字参数。

**Notice:**
**参数定义的顺序**必须是： `必选参数 -> 默认参数 -> 可变参数/命名关键字参数 -> 关键字参数` 。

**例子**：

    def f1(a, b, c=0, *args, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

    def f2(a, b, c=0, *, d, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

调用：

    >>> f1(1, 2)
    a = 1 b = 2 c = 0 args = () kw = {}
    >>> f1(1, 2, c=3)
    a = 1 b = 2 c = 3 args = () kw = {}
    >>> f1(1, 2, 3, 'a', 'b')
    a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
    >>> f1(1, 2, 3, 'a', 'b', x=99)
    a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
    >>> f2(1, 2, d=99, ext=None)
    a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}

除了这种普通的调用方式，通过tuple和dict也可以很神奇地调用！

    >>> args = (1, 2, 3, 4)
    >>> kw = {'d': 99, 'x': '#'}
    >>> f1(*args, **kw)
    a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
    >>> args = (1, 2, 3)
    >>> kw = {'d': 88, 'x': '#'}
    >>> f2(*args, **kw)
    a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}

赋值是按照上面的固定顺序来进行的！对于**任意函数**，都可以通过类似 `func(*args, **kw)` 的形式调用它，**无论它的参数是如何定义**的。

###小结
***
Python的函数具有非常灵活的参数形态，既可以实现简单的调用，又可以传入非常复杂的参数。

默认参数一定要用**不可变对象**，如果是可变对象，程序运行时会有逻辑错误！

要注意定义可变参数和关键字参数的语法：

\*args是可变参数，args接收的是一个**tuple**；

\*\*kw是关键字参数，kw接收的是一个**dict**。

以及调用函数时如何传入可变参数和关键字参数的语法：

可变参数既可以**直接传入**：func(1, 2, 3)，又可以**先组装list或tuple**，再通过 \*args 传入： `func(*(1, 2, 3))`；

关键字参数既可以**直接传入**：func(a=1, b=2)，又可以**先组装dict**，再通过 \*\*kw 传入： `func(**{'a': 1, 'b': 2})`。

使用 **\*args** 和 **\*\*kw** 是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。

命名关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。

定义命名关键字参数不要忘了写**分隔符** **\*** ，否则定义的将是位置参数。

##递归函数
若一个函数在函数内部调用自身，则该函数是一个递归函数。如：

    def fact(n):
        if n==1:
            return 1
        return n * fact(n - 1)

阶乘函数就是一个递归函数，使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过**栈（stack）**这种数据结构实现的。

每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致**栈溢出**。

解决递归调用栈溢出的方法是通过**尾递归**优化，尾递归和循环效果一样，实际上可以把循环看作特殊的尾递归函数。

尾递归要求函数返回时调用自身本身而**不能包含表达式**。这样编译器或解释器就可以把尾递归进行优化，无论递归了多少次都只占用一个栈帧。

    def fact(n):
      return fact_iter(n, 1)

    def fact_iter(num, product):
        if num == 1:
            return product
        return fact_iter(num - 1, num * product)

之前的函数定义有乘法表达式，所以不是尾递归。

计算过程如下：

    ===> fact(5)
    ===> 5 * fact(4)
    ===> 5 * (4 * fact(3))
    ===> 5 * (4 * (3 * fact(2)))
    ===> 5 * (4 * (3 * (2 * fact(1))))
    ===> 5 * (4 * (3 * (2 * 1)))
    ===> 5 * (4 * (3 * 2))
    ===> 5 * (4 * 6)
    ===> 5 * 24
    ===> 120

这里改为在函数调用前先计算product，每次递归仅调用函数本身就可以了。

计算过程如下：

    ===> fact_iter(5, 1)
    ===> fact_iter(4, 5)
    ===> fact_iter(3, 20)
    ===> fact_iter(2, 60)
    ===> fact_iter(1, 120)
    ===> 120

可惜**Python标准的解释器没有对尾递归做优化**，所以这里即使改为尾递归的写法还是有可能产生栈溢出。

###汉诺塔：
***
a有n个盘子，要求只借助a，b，c三个支架，把所有盘子移动到c。并且重的盘子不可以在轻的盘子上。

    def move(n, a, b, c):
    if n == 1:
         print(a,' --> ',c)
     else:
         move(n-1,a,c,b)   #将前n-1个盘子从A移动到B上
         print(a,' --> ',c)#将最底下的最后一个盘子从A移动到C上
         move(n-1,b,a,c)   #将B上的n-1个盘子移动到C上

代码很短，思路很清晰，基于规则，每次只能把余下盘子中最重的移到c上。 这里通过改变传入参数的顺序可以灵活使用三个支架。 a在一次移动中可能充当b的角色，b，c也能充当a的角色。

但总的来说，我们都是希望把充当a的支架上n-1个盘子先移到充当b的支架上，再把a的剩下的最重的一个盘子移动到充当c的支架上，然后递归，这时充当b的支架就变成a，充当a的支架就变成b，直到最后完成所有移动。

##高级特性
在Python中，代码越少越简单约好。基于这一思想，后面的几个篇章介绍Python一些非常有用的高级特性。

比方说构造一个1~99的奇数列表，可以简单地用循环实现：

    L = []
    n = 1
    while n <= 99:
        L.append(n)
        n = n + 2

###切片
***
切片即取一个list或tuple部分元素的操作。 当我们需要取列表前n个元素，即索引0~N-1的元素时，有两种方法：

1.方法1是用循环

    >>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
    >>> r = []
    >>> n = 3
    >>> for i in range(n):
    ...     r.append(L[i])
    ...
    >>> r
    ['Michael', 'Sarah', 'Tracy']

2.方法2是利用**切片操作符**

    >>> L[0:3]
    ['Michael', 'Sarah', 'Tracy']

如果经常要取指定的索引范围，用循环太过繁琐，Python提供了切片操作简化这个过程。

如果索引从0开始，还可以改写为 `L[:3]`。 如果索引到列表最后结束，同样可以简略写为 `L[0:]`。

此外，Python还支持倒数切片。列表最后一项的索引在倒数中为-1。

    >>> L[-2:]
    ['Bob', 'Jack']
    >>> L[-2:-1]
    ['Bob']

特别地，切片操作还支持每隔k个元素取1个这样的操作：

    >>> L = list(range(100))
    >>> L
    [0, 1, 2, 3, ..., 99]

先创建一个0~99的整数列表。

    >>> L[-10:]
    [90, 91, 92, 93, 94, 95, 96, 97, 98, 99]

取后10个只需起始索引为-10即可。

    >>> L[:10:2]
    [0, 2, 4, 6, 8]

前十个数隔两个取一个。

    >>> L[::5]
    [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]

所有数，隔五个取一个。

**Notice**：对list进行切片操作得到的还是list；对tuple进行切片操作得到的还是tuple。 特别地，字符串也可看为一种list，同样可以使用切片操作。

###迭代
***
Python中可迭代的对象包括字符串，list，tuple，dict，set和文件等等。 对这些可迭代对象可以使用 `for...in` 循环来遍历。  Python对for循环的抽象程度高于Java和C，所以即使没有下标也能迭代。

比如循环遍历一个dict：

    >>> d = {'a': 1, 'b': 2, 'c': 3}
    >>> for key in d:
    ...   print(key, d[key])
    ...
    a 1
    c 3
    b 2

直接打印key会打印所有dict中的key，更改迭代的写法为 `for value in d.values()` 就变为迭代dict中所有的value。 如果同时要访问key和value，还可以使用 `for k, v in d.items()`。

字符串同样可以用for循环迭代：

    >>> for ch in 'ABC':
    ...     print(ch)
    ...
    A
    B
    C

要判断一个对象是否可迭代对象可以通过**collections模块**的**Iterable类型**来判断。

    >>> from collections import Iterable
    >>> isinstance('abc', Iterable) # str是否可迭代
    True
    >>> isinstance([1,2,3], Iterable) # list是否可迭代
    True
    >>> isinstance(123, Iterable) # 整数是否可迭代
    False

正如上面迭代dict一样，for循环可以同时引用两个甚至多个变量：

    >>> for i, value in enumerate(['A', 'B', 'C']):
    ...     print(i, value)
    ...
    0 A
    1 B
    2 C

    >>> for x, y in [(1, 1), (2, 4), (3, 9)]:
    ...     print(x, y)
    ...
    1 1
    2 4
    3 9

例子里的enumerate方法通过[enumerate官方文档](https://docs.python.org/3/library/functions.html#enumerate)了解，它返回一个枚举对象，并且传入参数可迭代时它就是一个可迭代的对象。

可以用 `list(enumerate(可迭代对象))` 把一个可迭代对象变为一个元素为tuple类型的list，每个tuple有两个元素，形式如：(序号，原可迭代对象内容)。

并且使用enumerate时可以指定开始的序号，`enumerate(iterable, start=0)`，不写时默认参数为0，即序号从0开始。 可以自己指定为其他数。

###列表生成式
***
列表生成式即List Comprehensions，是Python内置的用于创建list的生成式。

比方说生成1到10，可以：

    >>> list(range(1, 11))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

要生成 `[1x1, 2x2, 3x3, ..., 10x10]`平方序列，笨办法是用循环：

    >>> L = []
    >>> for x in range(1, 11):
    ...    L.append(x * x)
    ...
    >>> L
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

用列表生成式只用一个语句就可以了：

    >>> [x * x for x in range(1, 11)]
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来。

在for循环后面还可以加上if判断，比方说这个例子用于筛选出偶数的平方数：

    >>> [x * x for x in range(1, 11) if x % 2 == 0]
    [4, 16, 36, 64, 100]

使用两层循环还可以生成全排列：

    >>> [m + n for m in 'ABC' for n in 'XYZ']
    ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

列出当前目录下所有文件和目录名也非常简单：

    >>> import os # 导入os模块，模块的概念后面讲到
    >>> [d for d in os.listdir('.')] # os.listdir可以列出文件和目录
    ['.emacs.d', '.Trash',  'Applications', 'Desktop', 'Documents']

前面一节提到for循环迭代可以同时用两个变量，这里列表生成式同样可以用两个变量来生成list。

    >>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
    >>> [k + '=' + v for k, v in d.items()]
    ['y=B', 'x=A', 'z=C']

把list中所有字符串的大写字母换成小写：

    >>> L = ['Hello', 'World', 'IBM', 'Apple']
    >>> [s.lower() for s in L]
    ['hello', 'world', 'ibm', 'apple']

###生成器
***
通过列表生成式可以简单地创建列表，但受到内存限制，列表容量是有限的。如果列表元素很多，而我们仅需访问前面一部分元素，则会造成很大的存储空间的浪费。

**生成器(generator)**就意在解决这个问题，允许在循环过程中不断推算出后续元素，而不用创建完整的list。在Python中，这种边循环边计算的机制称为生成器。

和列表生成式的区别很简单，仅仅是把外层的**[]方括号**换成**()圆括号**。

    >>> L = [x * x for x in range(5)]
    >>> L
    [0, 1, 4, 9, 16]
    >>> g = (x * x for x in range(10))
    >>> g
    <generator object <genexpr> at 0x1022ef630>

生成器无法通过索引访问，因为它**保存的是算法**，要遍历生成器需要通过**next()函数**。

    >>> next(g)
    0
    >>> next(g)
    1
    >>> next(g)
    4
    >>> next(g)
    9
    >>> next(g)
    16
    >>> next(g)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    StopIteration

当到达最后一个元素时，再使用next()就会抛出StopIteration的错误。 当然，实际遍历生成器时不会这样一个一个用next方法遍历，用for循环迭代即可。

    >>> g = (x * x for x in range(5))
    >>> for n in g:
    ...     print(n)
    ...
    0
    1
    4
    9
    16

当算法比较复杂，用简单for循环无法写出来时，还可以通过函数来实现：

    def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'

比方说这个计算斐波那契数列的函数，稍微改写一下即可变成generator：

    def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b  #只修改这里
        a, b = b, a + b
        n = n + 1
    return 'done'

这是定义generator的另一种方法，如果一个函数定义中包含yield关键字，则函数就变为一个generator。

    >>> f = fib(6)
    >>> f
    <generator object fib at 0x104feaaa0>

函数是顺序执行，遇到return语句或到达最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，**遇到yield就返回**，下次执行会从yield的地方开始。

    >>> for n in fib(6):
    ...     print(n)
    ...
    1
    1
    2
    3
    5
    8

同样地，把函数改成generator后，我们不需要用next()方法获取写一个返回值，而是只借用for循环进行迭代。

但是这样拿不到fib函数return语句的值(即字符串done)，要获取这个值必须捕获**StopIteration**这个错误，然后打印：

    >>> g = fib(6)
    >>> while True:
    ...     try:
    ...         x = next(g)
    ...         print('g:', x)
    ...     except StopIteration as e:
    ...         print('Generator return value:', e.value)
    ...         break
    ...
    g: 1
    g: 1
    g: 2
    g: 3
    g: 5
    g: 8
    Generator return value: done

生成器的工作原理是：在for循环的过程中不断计算出下一个元素，并在适当的条件结束for循环。

对于函数改成的generator来说，**遇到return语句或者执行到函数体最后一行语句**，就是**结束generator**的指令，for循环随之结束。

普通函数和生成器函数可以通过调用函数区分，普通函数直接返回结果，生成器函数返回的是一个生成器对象。

####杨辉三角
要求使用生成器生成1~10行的杨辉三角。 提示：把每一行当作一个list。

    def triangles(max):
    n = 0
    b = [1]
    while n < max:
        yield b
        b = [1] + [ b[i] + b[i + 1] for i in range(len(b) - 1)] + [1]
        n = n + 1

这段代码非常短，但是已经充分实现了题目要求，值得欣赏!

    >>> for L in triangles(6):
    ...     L
    ...
    [1]
    [1, 1]
    [1, 2, 1]
    [1, 3, 3, 1]
    [1, 4, 6, 4, 1]
    [1, 5, 10, 10, 5, 1]

代码里面有两个窍门，一是列表相加，注意不是列表元素相加。 列表相加相当于把后一个列表的元素全部append到前一个列表。
如：

    >>> L = [1,2]
    >>> R = [3,4]
    >>> L+R
    [1, 2, 3, 4]

上面代码中的b即把每一行当作一个list，因为每一行的开头结尾都是1，所以可以每一行的list看作三个list的相加，一头一尾两个list是只有1个元素1的list，中间的list用列表生成式生成。

另一个窍门就是这里的列表生成式。 注意这里计算时还没赋值，引用列表b的内容是上一行的信息，所以能巧妙地借助上一行计算相邻两数之和，最终得到n-2项的中间的list。

###迭代器
***
迭代器即**Iterator**， 前面说到可以通过**collections模块**的**Iterable类型**来判断一个对象是否可迭代对象。 这里引入Iterator的概念，可以通过类似的方式判断。

list，dict，str虽然是Iterable，却不是Iterator。 生成器都是Iterator。Iterator的特性允许对象通过**next()函数**不断返回下一个值。

    >>> from collections import Iterator
    >>> isinstance((x for x in range(10)), Iterator)
    True
    >>> isinstance([], Iterator)
    False
    >>> isinstance({}, Iterator)
    False
    >>> isinstance('abc', Iterator)
    False

要把list，dict，str变为Iterator可以使用iter()函数：

    >>> isinstance(iter([]), Iterator)
    True
    >>> isinstance(iter('abc'), Iterator)
    True

Python的Iterator对象表示的其实是一个**数据流**，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。

可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算，也因此能够节省空间。

Iterator甚至可以表示一个**无限大的数据流**，例如全体自然数。而使用list是永远不可能存储全体自然数的。

##函数式编程
首先理解函数式编程是一种**高度抽象**的编程范式，纯粹的函数式编程语言编写的函数**没有变量**，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为**没有副作用**。

而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。

Python仅对函数式编程**提供部分支持**。由于Python允许使用变量，因此，Python**不是**纯函数式编程语言。

对编程语言来说，越**低级**的语言，越贴近计算机，抽象程度**低**，执行效率**高**，比如汇编和C语言；越**高级**的语言，越贴近计算，抽象程度**高**，执行效率**低**，比如Lisp语言。

###三大特性：
***

1. **immutable data**
变量不可变，或者说没有变量，只有常量。 函数式编程输入确定时输出就是确定的，函数内部的变量和函数外部的没有关系，不会受到外部操作的影响。

2. **first class functions**
第一类函数(也称高阶函数)，意思是函数可以向变量一样用，可以像变量一样创建、修改、传递和返回。 这就允许我们把大段代码拆成函数一层层地调用，这种面向过程的写法相比循环更加直观。

3. **尾递归优化**
之前已经提及过了，就是递归时返回函数本身而非表达式。 可惜Python中没有这个特性。

###函数式编程的几个技术
***

- **map &
- ** ：

函数式编程最常见的技术就是对一个集合做Map和Reduce操作。这比起过程式的语言来说，在代码上要更容易阅读。（传统过程式的语言需要使用for/while循环，然后在各种变量中把数据倒过来倒过去的。

- **pipeline**：

这个技术的意思是把函数实例成一个一个的action，然后把一组action放到一个数组或是列表中，然后把数据传给这个action list，数据就像一个pipeline一样顺序地被各个函数所操作，最终得到我们想要的结果。

- **recursing** 递归 ：

递归最大的好处就简化代码，他可以把一个复杂的问题用很简单的代码描述出来。注意：递归的精髓是描述问题，而这正是函数式编程的精髓。

- **currying**：

把一个函数的多个参数分解成多个函数， 然后把函数多层封装起来，每层函数都返回一个函数去接收下一个参数这样，可以简化函数的多个参数。

- **higher order function**

高阶函数：所谓高阶函数就是函数当参数，把传入的函数做一个封装，然后返回这个封装函数。现象上就是函数传进传出，就像面向对象对象满天飞一样。

###函数式编程的几个好处
***

- **parallelization 并行**：

在并行环境下，各个线程之间不需要同步或互斥(变量都是内部的，不需要共享)。

- **lazy evaluation 惰性求值：**

表达式不在它被绑定到变量之后就立即求值，而是在该值被取用的时候求值。

- **determinism 确定性：**

输入是确定的，输出就是确定的。

###简单举例
***

以往面向过程式的编程需要引入额外的逻辑变量以及使用循环：

    upname =['HAO', 'CHEN', 'COOLSHELL']
    lowname =[]
    for i in range(len(upname)):
        lowname.append( upname[i].lower() )

而函数式编程则非常简洁易懂：

    def toUpper(item):
      return item.upper()

    upper_name = map(toUpper, ["hao", "chen", "coolshell"])
    print upper_name

再看一个计算一个列表中所有正数的平均数的例子：

    num =[2, -5, 9, 7, -2, 5, 3, 1, 0, -3, 8]
    positive_num_cnt = 0
    positive_num_sum = 0
    for i in range(len(num)):
        if num[i] > 0:
            positive_num_cnt += 1
            positive_num_sum += num[i]

    if positive_num_cnt > 0:
        average = positive_num_sum / positive_num_cnt

    print average

如果采用函数式编程：

    positive_num = filter(lambda x: x>0, num)
    average = reduce(lambda x,y: x+y, positive_num) / len( positive_num )

可以看到函数式编程减少了变量的使用，也就减少了出Bug的可能，维护更加方便。可读性更高，代码量也更小。

更多的例子和解析详见[函数式编程](http://coolshell.cn/articles/10822.html)。

###高阶函数
***
上一章节已经提及到了，这里主要再写几个例子加深理解。

####变量可以指向函数

    >>> abs
    <built-in function abs>
    >>> f = abs
    >>> f
    <built-in function abs>
    >>> f(-10)
    10

这个例子表明在Python中变量是可以指向函数的，并且这样赋值的变量能够作为函数的别名使用。

####函数名也是变量

    >>> abs = 10
    >>> abs(-10)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'int' object is not callable

这里把abs函数赋值为10，这样赋值以后abs就变成一个整形变量，指向int型对象10而不指向原本的函数了。所以无法再作为函数使用。

想恢复abs函数要重启Python交互环境。 abs函数定义在 `__builtin__` 模块中，要让修改abs变量的指向在其它模块也生效，要用 `__builtin__.abs = 10`。 当然实际代码却不可这样写..

####传入函数
函数能够作为参数传递，接收这样的参数的函数就称为高阶函数。 简单举例：

    def add(x, y, f):
        return f(x) + f(y)

    >>> add(-5, 6, abs)
    11

这里abs函数作为一个参数传入我们编写的add函数中，add函数就称为高阶函数。

####map/reduce
map()函数和reduce()函数是Python的两个内建函数(BIF)。

- **Map函数**

map()函数接收两个参数，一个是函数，一个是**Iterable对象**，map将传入的函数依次作用到序列的每个元素，并把结果作为**Iterator对象**返回。例如：

    >>> def f(x):
    ...     return x * x
    ...
    >>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> list(r)
    [1, 4, 9, 16, 25, 36, 49, 64, 81]

这里直接使用list()函数将迭代器对象转换为一个列表。

写循环也能达到同样效果，但是显然没有map()函数直观。 map()函数作为高阶函数，大大简化了代码，更易理解。

    >>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
    ['1', '2', '3', '4', '5', '6', '7', '8', '9']

将一个整数列表转换为字符列表仅仅需要一行代码。

- **Reduce函数**

reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把每次运算的结果继续和序列的**下一个元素**做累积计算。

    >>> from functools import reduce
    >>> def add(x, y):
    ...     return x + y
    ...
    >>> reduce(add, [1, 3, 5, 7, 9])
    25

这里举了一个最简单的序列求和作例子(当然实际上我们直接用sum()函数就好了~)。 这里reduce函数每次将add**作用于序列的前两个元素**，并**把结果返回序列的头部**，直到序列只剩下1个元素就返回结果。

    >>> from functools import reduce
    >>> def fn(x, y):
    ...     return x * 10 + y
    ...
    >>> def char2num(s):
    ...     return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s] #字符对应整数的dict，返回传入字符对应的整数
    ...
    >>> reduce(fn, map(char2num, '13579'))
    13579

可以整理一下，作为一个整体的str2int函数：

    def str2int(s):
        def fn(x, y):
            return x * 10 + y
        def char2num(s):
            return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]
        return reduce(fn, map(char2num, s))

使用lambda匿名函数还可以进一步简化：

    def char2num(s):
        return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]

    def str2int(s):
        return reduce(lambda x, y: x * 10 + y, map(char2num, s))

- **练习**

1.利用map()函数，把规范的英文名字，变为首字母大写，其他小写的规范名字。

    def normalize(name):
        return name[0].upper()+name[1:].lower()

    L1 = ['adam', 'LISA', 'barT']
    L2 = list(map(normalize, L1))
    print(L2)

**Hint**:

- 字符串支持切片操作，并且可以用加号做字符串拼接。

- 转换大写用upper函数，转换小写用lower函数。

2.编写一个prod()函数，可以接受一个list并利用reduce()求积。

    from functools import reduce
    def prod(L):
        return reduce(lambda x,y: x*y,L)

    print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))

**Hint**:

- 用匿名函数做两数相乘
- 用reduce函数做归约，得到列表元素连乘之积。

3.利用map和reduce编写一个str2float函数，把字符串'123.456'转换成浮点数123.45。

    from functools import reduce
    from math import pow

    def chr2num(s):
        return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]

    def str2float(s):
        return reduce(lambda x,y:x*10+y,map(chr2num,s.replace('.',''))) / pow(10,len(s)-s.find('.')-1)

    print(str2float('985.64785'))

**Hint**:

- 这题的思路是找到小数点的位置i(从个位开始数i个数字之后)，然后让转换出的整数除以10的i次方。

- 另外一种思路是在转换时遇到小数点后，改变转换的方式由 `num*10+当前数字` 变为 `num+当前数字/point`。 point初始为1，每次加入新数字前除以10。

####filter
filter()函数同样是内建函数，用于过滤序列。 filter()接收一个函数和一个序列。 和map()不同的时，filter()把传入的函数依次作用于每个元素，然后**根据返回值**是True还是False决定**保留还是丢弃该元素**。

简单的奇数筛选例子：

    def is_odd(n):
        return n % 2 == 1

    list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
    # 结果: [1, 5, 9, 15]

晒掉列表的空字符串：

    def not_empty(s):
        return s and s.strip()

    list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
    # 结果: ['A', 'B', 'C']

其中，strip函数用于删除字符串中特定字符，格式为：`s.strip(rm)`，删除s字符串中开头、结尾处，位于rm删除序列的字符。 rm为空时默认删除空白符(包括'\n', '\r',  '\t',  ' ')。

注意到filter()函数返回的是一个Iterator，也就是一个**惰性序列**，所以要强迫filter()完成计算结果，需要用list()函数获得所有结果并返回list。

filter函数最重要的一点就是正确地定义一个**筛选函数**（即传入filter作为参数的那个函数)。

- **练习**

这里使用**埃氏筛法**。

    首先，列出从2开始的所有自然数，构造一个序列：

    2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, ...

    取序列的第一个数2，它一定是素数，然后用2把序列的2的倍数筛掉：

    3, 5, 7, 9, 11, 13, 15, 17, 19, ...

    取新序列的第一个数3，它一定是素数，然后用3把序列的3的倍数筛掉：

    5, 7, 11, 13, 17, 19, ...

    以此类推...

1.用filter筛选素数

    def _odd_iter():
        n = 1
        while True:
            n = n + 2
            yield n

首先构造一个生成器，输出3开始的奇数序列。

    def _not_divisible(n):
        return lambda x: x % n > 0
然后定义一个筛选函数，传入n，判断x能否除尽n：

    def _not_divisible(n):
        return lambda x: x % n > 0

这里x是匿名函数的参数，**由外部提供**。

然后就是定义返回素数的生成器了。

1.首先输入素数2，然后初始化奇数队列，每次输出队首(必然是素数，因为前一轮的过滤已经排除了比当前队首小且非素数的数)。

2.构造新的队列，每次用当前序列最小的数作为除数，检验后面的数是否素数。

定义如下：

    def primes():
        yield 2
        it = _odd_iter() # 初始序列
        while True:
            n = next(it) # 返回序列的第一个数
            yield n
            it = filter(_not_divisible(n), it) # 构造新序列

这里因为it是一个迭代器，每次使用next就得到队列的下一个元素，实际上就类似队列的出列操作，挤掉队首，不用担心重复。

然后这里filter的原理，就是把当前it队列的每个数都放进_not_divisible(n)中检测一下，注意不是作为参数n传入而是作为匿名函数的参数x传入！

\_not\_divisible(n)实际是一个整体来看的，它**返回**的是一个带有参数n的**另一个函数**(这里即那个匿名函数)，然后filter再把列表每一个元素传入这个返回的函数中。一定要搞清楚这一点。


3.最后因为primes产生的也是一个无限序列，我们一般不需要求那么多，简单写个循环用作退出即可：

    # 打印1000以内的素数:
    for n in primes():
        if n < 1000:
            print(n)
        else:
            break

2.用filter筛选回文数

    def is_palindrome(n):
        return str(n) == str(n)[::-1]
    print(list(filter(is_palindrome, range(0,1001))))

**Hint**:

- str转换整数为字符串
- [::-1]可以得到逆序的列表。

####sorted
Python内置的sorted()函数就可以对list进行排序：

    >>> sorted([36, 5, -12, 9, -21])
    [-21, -12, 5, 9, 36]

并且sorted作为一个高阶函数还允许接受一个key函数用于自定义排序，例如：

    >>> sorted([36, 5, -12, 9, -21], key=abs)
    [5, 9, -12, -21, 36]

key指定的函数将作用于list的每一个元素上，并**根据key函数返回(映射)的结果进行排序**，最后**对应回列表中的元素**进行输出。

再看一个字符串排序例子：

    >>> sorted(['bob', 'about', 'Zoo', 'Credit'])
    ['Credit', 'Zoo', 'about', 'bob']

默认情况下是按ASCII码排序的，但我们往往希望按照字典序来排，思路就是把字符串变为全小写/全大写再排：

    >>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
    ['about', 'bob', 'Credit', 'Zoo']

默认排序是由小到大，要反相排序只需把reverse参数设为True。 温习前面的知识，这里reverse参数是一个**命名关键字参数**。

    >>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
    ['Zoo', 'Credit', 'bob', 'about']

使用好sorted函数的关键就是定义好一个映射函数。

**练习**：给出成绩表，分别按姓名和成绩进行排序。

    >>> L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
    >>> L2 = sorted(L, key = lambda x:x[0])    #按姓名排序
    >>> L2
    [('Adam', 92), ('Bart', 66), ('Bob', 75), ('Lisa', 88)]
    >>> L3 = sorted(L, key = lambda x:x[1])    #按成绩排序
    >>> L3
    [('Bart', 66), ('Bob', 75), ('Lisa', 88), ('Adam', 92)]

###返回函数
***

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

看一个例子：

    def count():
        fs = []
        for i in range(1, 4):
            def f():
                 return i*i
            fs.append(f)
        return fs

    f1, f2, f3 = count()

结果是：

    >>> f1()
    9
    >>> f2()
    9
    >>> f3()
    9

和我们预期的1，4，9不同。 这是因为当返回一个函数时，返回的函数内部的代码是**没有执行**的！ 只有在**调用这个返回的函数时才会执行**！

调用count函数时，实际上返回了3个新的函数，循环变量i的值也变为3。然后调用这3个返回的函数时，代码中引用的i的值就是3。

####闭包:

Python中的闭包从表现形式上定义为：如果在一个内部函数里，对外部作用域（**非全局作用域**）的变量进行引用，那么这个**内部函数就被认为是闭包**(closure)。 如上面例子中的f就是一个闭包。

必须注意：

- 返回闭包时，闭包中**不要引用任何循环变量或者后续会发生变化的变量**。

- 意思即不要在闭包的代码中使用外部函数用来做循环的变量或者有改变的变量。

- 另外在闭包中是**不允许修改**外部函数的局部变量的。

如果一定要在闭包中用到外部的循环变量，也不是不可。 定义一个新的函数，**用参数绑定循环变量**即可。 这样无论后面循环变量怎么变，**已经绑定到参数的值是不会变**的。

    def count():
        def f(j):
            def g():
                return j*j
            return g
        fs = []
        for i in range(1, 4):
            fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
        return fs

    >>> f1, f2, f3 = count()
    >>> f1()
    1
    >>> f2()
    4
    >>> f3()
    9

这里闭包g用到的变量j是**外部作用域f**的，并且**j作为参数绑定在f中不再改变**，不同于i。 所以执行count返回的3个函数，每个的结果的不同。

**Notice**:

**返回的函数都是不同的**！

即使**两次对函数a传入相同的参数**(函数a是一个能返回函数的高阶函数)，函数a**返回的两个函数也是不相等的**！即使用 `==` 判断得到的是 `False`。

###匿名函数
***

当我们在传入函数时，有些时候，**不需要显式地定义函数**，直接传入匿名函数更方便。

    >>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
    [1, 4, 9, 16, 25, 36, 49, 64, 81]

举个简单例子，计算 `f(x)=x²` 时，不需要显示定义f(x)，而是直接一行解决，这样写非常简洁。

关键字**lambda表示匿名函数**，冒号前面的**x表示函数参数**。

匿名函数有个限制，就是**只能有一个表达式**，不用写return，**返回值就是该表达式的结果**。

所以上面这个匿名函数就是：x作为参数传入，返回 `x*x` 的结果。

用匿名函数有个好处，因为函数没有名字，**不必担心函数名冲突**。此外，匿名函数也是一个**函数对象**，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

    >>> f = lambda x: x * x
    >>> f
    <function <lambda> at 0x101c6ef28>
    >>> f(5)
    25

并且匿名函数作为一个函数对象，也能被函数返回(像上一节那样)：

    def build(x, y):
        return lambda: x * x + y * y

###装饰器
***

函数对象有一个__name__属性，可以拿到函数的名字：

    >>> def now():
    ...     print('2015-3-25')
    ...
    >>> now.__name__
    'now'

假设我们要增强now()函数的功能，比如，在函数调用前后自动打印日志，但又**不希望修改now()函数的定义**，这种**在代码运行期间动态增加功能**的方式，称之为“装饰器”（Decorator）。

比方说定义一个打印日志的decorator：

    def log(func):
        def wrapper(*args, **kw):
            print('call %s():' % func.__name__)
            return func(*args, **kw)
        return wrapper

像正常函数一样定义，接收一个函数作为参数，并且返回一个函数wrapper。

    @log
    def now():
        print('2016-2-10')

使用时借助Python的**@语法**，把decorator放在函数定义前。

运行时：

    >>> now()
    call now():
    2015-3-25

原理如下：

把@log放在now()函数前实际上即执行了：

    now = log(now)

now这个函数名作为变量指向了log(now)返回的wrapper函数，如果查看now的函数名：

    >>> now.__name__
    'wrapper'

就可以看到这样的结果。 然后当我们调用now()函数时，wrapper的代码就会被执行，也就是打印了 `call now():`。

接下来因为wrapper函数返回的是**执行原函数**(传入log函数的参数)得到**的结果**，所以就可以看到原函数执行的结果。

**Notice**：

wrapper函数的参数是 `*args, **kw`，按照前面章节的说法，wrapper函数**可以接收任何参数**！

并且因为使用**@语法**之后，now指向的就是wrapper函数，所以这里传入wrapper函数的参数在调用时就是传入now函数中，所以如果要传入参数到wrapper中，调用时 `now(参数列表)` 就可以了。

####带参数的decorator
如果decorator本身还要传入参数，就要在外层再写一个返回decorator的高阶函数，比如自定义打印的文本：

    def log(text):
        def decorator(func):
            def wrapper(*args, **kw):
                print('%s %s():' % (text, func.__name__))
                return func(*args, **kw)
            return wrapper
        return decorator

使用decorator时：

    @log('execute')
    def now():
        print('2015-3-25')

执行结果如下：

    >>> now()
    execute now():
    2015-3-25

这时把@log放在now()函数前实际上即执行了：

    >>> now = log('execute')(now)

其中'execute'是log函数的参数，log函数返回decorator函数，将传入参数now，然后decorator再返回wrapper，并且赋值给变量now。

####属性复制
前面已经提到使用**@语法**之后，now变量指向的函数名字等属性都改变了，变成了wrapper的，实际我们希望now的属性依然是now()函数的属性，这时就需要进行**属性复制**。

我们不需要编写 `wrapper.__name__ = func.__name__` 这样的代码，Python内置的 `functools.wraps` 可以满足我们的需求。

    import functools

    def log(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('call %s():' % func.__name__)
            return func(*args, **kw)
        return wrapper

完整的decorator定义代码如上，唯一修改的就是加上了 `@functools.wraps(func)` 这一句。 并且要把functools模块import进来。

####小结
在面向对象（OOP）的设计模式中，decorator被称为**装饰模式**。OOP的装饰模式需要通过继承和组合来实现。

而Python除了能支持OOP的decorator外，直接**从语法层次**支持decorator。Python的decorator可以用函数实现，**也可以用类实现**。

####练习
1.编写一个decorator，能在函数调用的前后打印出'begin call'和'end call'的日志。

    def decorator(func):
        def warps(*args,**kw):
            print("begin call")
            a = func(*args,**kw)
            print("end call")
            return a
        return warps

    @decorator
    def now():
        print("haha")

    now()

**解析**：

这题now()函数没返回，return的实际就是None，所以a指向的是None。 调用now()返回的也就是None，但执行warps的过程中打印了我们需要的结果。

流程：`执行warps时先打印了begin call` -> `然后执行func时打印了haha` -> `最后执行完func打印end call`。

2.写出一个@log的decorator，使它既支持：

    @log
    def f():
        pass

又支持：

    @log('execute')
    def f():
        pass

**解析**：

思路很简单，和前文的带参数decorator一样，不过把参数设置为**默认参数**，然后在wrapper中判断一下参数是否默认值，再作对应处理就可以了。

    >>> import functools
    >>> def log(text=''):
            def decorator(func):
            @functools.wraps(func)    #属性复制
            def warps(*args,**kw):
                if text=='':
                    print('%s():' % func.__name__)
                    return func(*args, **kw)
                else:
                    print('%s %s():' % (text, func.__name__))
                    return func(*args, **kw)
            return warps
        return decorator

    >>> @log('execute')  #带参数text的decorator
    ... def now():
        print('2016-2-10')

    >>> now()
    execute now():
    2016-2-10

    >>> @log()           #无参数的decorator
    ... now():
    print('2016-2-10')

    >>> now()
    now():
    2016-2-10

###偏函数
***

Python的functools模块提供了很多有用的功能，其中一个就是偏函数（Partial function）。

`functools.partial(f, *args, **kw)` 的作用就是创建一个偏函数，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。

举个例子，字符串转整型数的函数int，可以使用**关键字参数base**，指定字符串的进制是多少，然后转换为int是会进行进制转换，把字符串**转换成十进制整数**。 如：

    >>> int('1000000', base=2)
    64

如果有大量的二进制字符串要转换，每次都写base=2很麻烦，我们就会希望定义一个新函数，把base参数固定为2，无须每次指定：

    def int2(x, base=2):
        return int(x, base)

实际上我们不需要自己定义，使用 `functools.partial` 就可以轻松创建偏函数：

    >>> import functools
    >>> int2 = functools.partial(int, base=2)
    >>> int2('1000000')
    64
    >>> int2('1010101')
    85

运行int('1000000')实际相当于：

    kw = { 'base': 2 }
    int('1000000', **kw)

**Notice**：

这里创建偏函数只是设定了默认值为2，调用函数时依然可以把base参数设置为其他值。

    >>> int2('1000000', base=10)
    1000000

`functools.partial` 不但可以接收关键字参数，还可以接收可变参数 `*args`，如：

    >>> max2 = functools.partial(max, 10)
    >>> max2(5, 6, 7)
    10

相当于max函数每次接收到若干数字时，都默认再放入整数10，然后再找最大值。

##模块
在开发过程中，一个文件里代码越长就越不容易维护。

为了编写易于维护的代码，我们把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。

在Python中，**一个.py文件**就称之为一个**模块**（Module）。

###使用模块的几个好处
***

- 大大提高了代码的可维护性。
- 编写代码不必从零开始。当一个模块编写完毕，就可以被其他地方引用。
- 避免函数名和变量名冲突。相同名字的函数和变量完全可以分别存在不同的模块中。 但还是要注意**变量名尽量不要与BIF名字冲突**。

###模块名相同怎么办？引入包名！
***

如果不同的人编写的模块名相同，为了**避免模块名冲突**，Python又引入了**按目录来组织模块**的方法，称为包（Package）。

![图片](http://www.liaoxuefeng.com/files/attachments/001388366035986b515b38d149b4efaaac3f2c721813d2c000/0)

假设图中abc和xyz两个模块的名字和外面其他模块名字冲突了，可以通过包来组织模块，避免冲突。 只要**顶层包名**(这里是创建了一个**mycompany**文件夹)不同即可。

此时abc的**模块名**变为 `mycompany.abc`, xyz的**模块名**变为 `mycompany.xyz`。



**Notice**：

- **注意区分模块和模块名**！两者不一定相同！

- **每个包**目录下都有一个 `__init__.py` 文件，必须存在！ 否则Python就不会把这个文件夹当作一个包。  `__init__.py`可以是空文件，也可以有Python代码，本身就是一个模块，并且它的模块名是包名(这里是mycompany)。

![图片](http://www.liaoxuefeng.com/files/attachments/00138836605526535c9bebcbf414c3dae2430c50bbeef29000/0)

包结构可以是多级的。比方说这里www.py的**模块名**就是 `mycompany.web.www` 。 两个utils.py的模块名分别是 `mycompany.utils` 和 `mycompany.web.utils` 。 `mycompany.web` 这个模块名对应的就是web目录下的 `__init__.py` 模块。

**Notice**:

自己创建的模块的模块名**不要和Python自带的模块的模块名冲突**！ 比如系统自带sys模块，自己的模块就不要命名sys.py，否则无法import系统自带的sys模块。

###使用模块
***

Python本身就内置了很多模块可以直接import使用。 下面自己编写一个hello模块作为例子：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-

    ' a test module '

    __author__ = 'Michael Liao'

    import sys

    def test():
        args = sys.argv
        if len(args)==1:
            print('Hello, world!')
        elif len(args)==2:
            print('Hello, %s!' % args[1])
        else:
            print('Too many arguments!')

    if __name__=='__main__':
        test()

####例子解析
- ***Python模块的标准文件模板***

第1行和第2行是**标准注释**，第1行注释可以让这个hello.py文件直接在Unix/Linux/Mac上运行，第2行注释表示.py文件本身使用标准UTF-8编码；

第4行是一个字符串，表示模块的文档注释，**任何模块代码的第一个字符串都被视为模块的文档注释；**

第6行使用__author__变量记录模块作者。

以上就是**Python模块的标准文件模板**，不写其实也没关系，但养成良好的习惯更好。

- ***正式代码部分***

1.导入sys模块

使用Python内置的sys模块，首先要导入模块： `import sys`。 导入之后，相当于创建了一个**变量sys**，该变量**指向sys模块**，通过这个变量可以访问sys模块的全部功能。

2.使用argv变量获取参数列表

**sys模块有一个argv变量**，用list存储了**命令行的所有参数**。 argv至少有一个元素，因为第一个参数永远是该.py文件的名称，例如：

在命令行执行 `python3 hello.py` 获得的 `sys.argv` 就是 `['hello.py']`；

在命令行执行 `python3 hello.py Michael` 获得的 `sys.argv` 就是 `['hello.py', 'Michael]`。 要获取字符串 `'Michael'` 只需要调用 `sys.argv[1]`。 **argv本身是一个list对象**。

3.条件判断

    if __name__=='__main__':
        test()

在hello模块中我们定义了一个test函数，这个条件判断的意思就是，如果执行这个.py文件时，`__name__` 变量等于`'__main__'`的话就执行test()函数。

其中 `__name__` 变量是个特殊变量，当我们**在命令行执行**hello.py时，Python解释器就会把它赋值为 `__main__`。

如果在别的地方导入hello模块，不会有对 `__name__` 赋值的操作，if判断就会失败。 借助这个特性，我们可以**在if判断中编写一些额外的代码**，用于在命令行运行时执行。**运行测试**就需要这样做。

####在命令行执行
先切换到保存hello.py的目录，然后执行：

    $ python3 hello.py
    Hello, world!
    $ python hello.py Michael
    Hello, Michael!

####在交互环境下执行
比方说使用IDLE或者在命令行中输入python进入。

    >>> import hello
    >>> hello.test()
    Hello, world!

import将不会促发if判断内的语句，要使用test函数就要通过 `模块名.函数名` 的方式调用，使用模块内的其他变量也同理。

###命名规范和作用域
***
在模块中我们会定义很多函数和变量，有些是希望给别人用的，有些则希望仅仅在模块内部使用。

- **公开(public)的变量和函数**

命名格式如： `abc`，`x123`，`PI`， 如果是有特殊用途则**在名称前后各加上两个下划线**，如：`__author__`，`__name__`。

    >>> hello.__doc__    #文档注释
    ' a test module '
    >>> hello.__name__    #模块名
    'hello'
    >>> hello.__author__    #作者名
    'Michael Liao'
    >>> hello.__file__    #文件路径
    'F:\\Python35\\hello.py'

可以看到 `__doc__` 变量返回了我们前面模块例子的代码中第一个字符串，也即文档注释。

- **私有(private)的变量和函数**

命名格式是在**名称前**加一个或两个下划线。 即：`_xxx` 和 `__xxx`。 这样的函数或变量**不应该被外部直接引用**(即通过 `模块名.变量名` 的方式调用)。

    def _private_1(name):
        return 'Hello, %s' % name

    def _private_2(name):
        return 'Hi, %s' % name

    def greeting(name):
        if len(name) > 3:
            return _private_1(name)
        else:
            return _private_2(name)

当用户使用模块时不需要关心私有的变量和函数，直接使用公开的变量和函数就可以了。 这中**代码封装和抽象**的方法非常使用。

**Notice**：

- 这里说私有函数和变量 “***不应该***” 被直接引用，而不是 “***不能***” 被直接引用。 Python没有方法可以限制用户使用私有函数和变量，所以这样命名只是一种编程习惯。

- 良好的习惯是**外部不需要引用**的函数全部定义成private，只有**外部需要引用的**函数才定义为public。

###安装第三方模块
***
在Python中，安装第三方模块，是通过**包管理工具pip**完成的。

安装Python时选择了安装pip的话，就可以直接在命令行中使用pip工具了。

    pip install Pillow

在命令行键入 `pip install 第三方模块名` 后， pip就会自动帮用户下载并安装第三方模块。

![安装1](http://imglf0.nosdn.127.net/img/dnpRZUpJZlB5VUQ2UTdlUDluSm9RZEZVQ0hEMjZrWjJReENQRmwvNVp4U0tnN3VUV004RE1BPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg)

![安装2](http://imglf2.nosdn.127.net/img/dnpRZUpJZlB5VUQ2UTdlUDluSm9RZU9hUFRtT0dMaFRWTENkaW1SREVrN25rd3VXSlVBKy9nPT0.png?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg)

这里安装的是**Python Imaging Library**这个第三方库，是一个Python下非常强大的图像处理工具库。 因为PIL只支持到Python2.7，所以这里用的是基于PIL开发的支持Python3的**Pillow**。

安装完成后打开 `F:\Python35\Lib\site-packages` 文件夹就会发现多了两个文件夹，一个是 `Pillow-3.1.1.dist-info`, 另一个是  `PIL`。 前者包含该库的一些基本信息，后者就是我们需要用到的包了，里面是有 `__init__.py` 文件的。

安装好的包我们可以直接用 `from 包名 import 模块名` 来import我们要用的模块，而不需要再转到这个目录下了。

    >>> from PIL import Image
    >>> im = Image.
    >('test.png')
    >>> print(im.format, im.size, im.mode)
    PNG (400, 300) RGB
    >>> im.thumbnail((200, 100))    #创建缩略图
    >>> im.save('thumb.jpg', 'JPEG')
    >>> im.show()    #打开图片

这一段就是利用PIL包的Image模块生成图片缩略图的代码，最后用show()函数打开图片可以浏览结果。

其他常用的第三方库还有MySQL的驱动：mysql-connector-python，用于科学计算的NumPy库：numpy，用于生成文本的模板工具Jinja2，等等。

###模块搜索路径
***

在Python中导入模块时，Python解释器都会搜索指定好的路径。

    >>> import sys
    >>> sys.path
    ['', 'F:\\Python35\\Lib\\idlelib', 'F:\\Python35\\python35.zip', 'F:\\Python35\\DLLs', 'F:\\Python35\\lib', 'F:\\Python35', 'F:\\Python35\\lib\\site-packages']

通过sys模块的path变量，我们可以看到搜索路径包括当前目录(`''`)，以及所有已安装的内置模块和第三方模块的目录。

如果需要使用自己的模块，可以把它们放到这些目录中。 也可以**增加搜索路径**。

1.**方法一**是**直接修改sys.path变量**，这种方法只在运行时有效，重启Python交互环境后会恢复原来的路径。

    >>> import sys
    >>> sys.path.append('C:\\Users\\Administrator\\Desktop')

2.**方法二**是**配置环境变量PYTHONPATH**(和PATH环境变量类似)，注意只需要增加自己的搜索路径，Python本身默认的不会被覆盖。

###文件搜索路径
***

**Notice**:

注意区分**文件搜索路径**和**模块搜索路径**！ 前者使用的是当前工作目录，后者则是前文提及的sys.path列表。

要获取当前工作路径可以：

    >>> import os
    >>> os.getcwd()
    'F:\\Python35'

要调用的文件如果放在当前工作路径上就可以直接用 `文件名.文件格式` 指定，比方说前文的test.jpg就是直接放在 `F:\\Python35` 的。 注意Python中路径的斜杠需要转义，所以要用 `\\`。

如果要使用自己的路径则分两种情况：

1.**绝对路径**

    im = Image.open('C:/Users/Administrator/Desktop/test.jpg')

这里用了左斜杠 `/`，用 `\\` 也是可以的，填写完整的路径即可。

2.**相对路径**

    im = Image.open('./Desktop/test.jpg')

相对路径即相对当前工作路径而言的路径，可以避免路径过长，用 `.` 来代替当前工作路径即可。 同样既可以用左斜杠 `/`，也可以用 `\\`。

##面向对象编程
面向对象编程——Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含**数据**和**操作数据的函数**。

***
**对比**：

- 面向过程：

把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

- 面向对象：

把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。
***

下面以打印学生成绩表为例，分别展示面向过程编程和面向对象编程的不同：

**面向过程**：

    std1 = { 'name': 'Michael', 'score': 98 }
    std2 = { 'name': 'Bob', 'score': 81 }

    def print_score(std):
        print('%s: %s' % (std['name'], std['score']))

存储学生和成绩可以用dict，而打印学生成绩则可以通过函数实现。

**面向对象**：

    class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))

    bart = Student('Bart Simpson', 59)
    lisa = Student('Lisa Simpson', 87)
    bart.print_score()
    lisa.print_score()

如果采用面向对象的程序设计思想，则首先想的不是流程，而是学生这个**类型**(class)应该应该被视为一个对象，这个对象拥有 `name` 和 `score` 两个**属性**(propoty)。

如果想打印学生的成绩就要给这个对象**发一个消息**(print_score)，让这个对象自己把成绩打印出来。

给对象发消息实际上就是调用对象的对应函数，成为对象的**方法**(method)。

***
####小结：
面向对象的设计思想是从自然界中来的，因为在自然界中，类（Class）和实例（Instance）的概念是很自然的。

Class是一种**抽象概念**，比如我们定义的Class——Student，是指学生这个概念，而实例（Instance）则是一个个**具体的**Student，比如，Bart Simpson和Lisa Simpson是两个具体的Student。

类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”，**每个对象都拥有相同的方法，但各自的数据可能不同**。 各个实例互不影响，创建实例的过程称为**实例化对象**。

所以，面向对象的设计思想是**抽象出Class**，**根据Class创建Instance**。

面向对象的**抽象程度又比函数要高**，因为一个Class既包含数据，又包含操作数据的方法。

####面向对象编程的三大特点：

1. **数据封装**
2. **继承**
3. **多态**

###类和实例
***

在Python中定义类是通过 `class` 关键字完成的，像前面例子一样：

    class Student(object):
        pass

定义类的格式是 `class 类名(继承的类名)` 。 类名一般用大写字母开头，继承在后面的章节讲，**object类**是所有类最终都会继承的类。

这样定义后，即使没有构造函数和属性，我们也可以实例化对象：

    >>> bart = Student()
    >>> bart
    <__main__.Student object at 0x10a67a590>
    >>> Student
    <class '__main__.Student'>

可以看到变量bart指向的就是一个Student类的实例，**每个实例的地址是不一样的**。

与静态语言不同，Python作为动态语言，我们可以自由地给一个实例变量绑定属性：

    >>> bart.name = 'Bart Simpson'
    >>> bart.name
    'Bart Simpson'

绑定的属性在定义类时无须给出，我们随时可以给一个实例绑定属性。 但是！这个属性**仅仅绑定在这个实例上**，别的实例是没有的！ 也就是说对于**同一个类的多个实例**，每个实例拥有的属性都可能不同！

对于我们认为**必须绑定的属性**，可以通过定义特殊的 `__init__` 方法，在创建实例时就进行绑定：

    class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

使用 `__init__` 方法要注意：

1.第一个参数永远是 `self`，指向创建的**实例本身**。

2.有了 `__init__` 方法，创建实例时就不能传入空的参数，必须**传入与 `__init__` 方法匹配的参数**，**self不用传**，Python解释器会自动传入。

    >>> bart = Student('Bart Simpson', 59)
    >>> bart.name
    'Bart Simpson'
    >>> bart.score
    59

####数据封装
这是面向对象编程特点之一，对于一个类的每个实例而言，属性访问可以通过函数来实现。 既然**实例本身拥有属性数据**，那么访问数据就不需要通过外面的函数实现，可以**直接在类的内部定义访问数据的函数**。

利用内部定义的函数，就把数据**封装**起来了，这些函数和类本身是关联的，称为**类的方法**。

    class Student(object):

        def __init__(self, name, score):
            self.name = name
            self.score = score

        def print_score(self):
            print('%s: %s' % (self.name, self.score))

依然是前面的例子，可以看到在类中定义方法和外面定义的函数**唯一区别**就是**第一个参数永远是实例变量 `self`**。

其他一致，仍然可以用默认参数，可变参数，关键字参数和命名关键字参数。 调用时**不需传入self**，**其他参数正常传入**。

对于外部，**类的方法实现细节也不用了解**，只需要知道怎样调用，能返回什么就可以了。

###访问限制
***

实例化对象后，我们还是可以直接通过属性名来访问一个实例的属性值，并且自由地修改。  如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线 `__` ，这样属性就变成一个私有变量。

     class Student(object):

        def __init__(self, name, score):
            self.__name = name
            self.__score = score

此时外部无法直接访问 `__name` 和 `__score` 属性：

    >>> bart = Student('Bart Simpson', 98)
    >>> bart.__name
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'Student' object has no attribute '__name'

通过访问限制的保护，外部代码不能随意修改对象内部的数据，代码更加健壮。

**Notice**：

1.还是可以给实例绑定属性：

    >>> bart.__name='123'
    >>> bart.__name
    '123'

所以如果对bart绑定一个 `__name` 属性，是能够成功的！ Python没有任何机制预防这一点，只有自己注意！

2.不能访问的实质是Python解释器**对外**把 `__name` 变量改成了 `_Student__name` 变量。 **通过后者依然能够直接访问和修改**，但是不建议这样做，Python没有机制阻止，全靠自觉。

    >>> bart._Student__name
    'Bart Simpson'
    >>> dir(bart)
    ['_Student__name', '_Student__score', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'print_score']

通过 `dir(bart)` 可以查看实例的所有变量。

3.在Python中，变量名类似 `__xxx__` 的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，**特殊变量可以直接访问**，注意**私有变量不要这样取名**。

4.有时会看到以**一个下划线开头**的实例变量名，比如 `_name`，这样的实例变量外部是可以访问的，但是，按照约定，这样的变量将**视为私有变量，不应**在外部直接访问。

####获取和修改限制访问的变量
对于限制访问的变量，外部代码还是需要获取和修改，我们可以在类中定义对应的get方法和set方法：

    class Student(object):
        ...

        def get_name(self):
            return self.__name

        def get_score(self):
            return self.__score

        def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')

这样做而不直接在外部修改有一个明显的好处，我们可以在类的方法中**对参数做检查**，**避免传入无效的参数**。 比如这里可以限制修改成绩时成绩的范围必须是0~100，超出就报错。

###继承和多态
***

在OOP程序设计中，当我们定义一个class的时候，可以从某个现有的class继承，新的class称为**子类**（Subclass），而被继承的class称为**基类、父类或超类**（Base class、Super class）。

    >>> class Animal(object):
            def run(self):
                print('Animal is running...')

比方说已经有了Animal类，并且类有一个run方法。

    >>> class Dog(Animal):
           pass

    >>> dog=Dog()
    >>> dog.run()
    Animal is running...

定义一个Dog类继承Animal类，虽然什么都没做，但是Dog类可以**获得父类的全部功能**，所以自动获得了 `run()` 方法。

子类还可以增加一些别的方法：

    class Dog(Animal):

        def run(self):
            print('Dog is running...')

        def eat(self):
            print('Eating meat...')

继承的另一个好处就是**多态**，如果子类也定义一个和父类相同的方法，则执行时**总是调用子类的方法**。

####实例的数据类型

我们定义一个类时，实际就是定义了一种数据类型，和list，str等没有什么区别，要判断一个变量是否属于某种数据类型可以使用 `isinstance()` 方法：

    >>> isinstance(dog,Animal)
    True
    >>> isinstance(dog,Dog)
    True

实例化子类的对象**既属于子类数据类型也属于父类数据类型**！ 但是反过来就不可以，实例化父类的对象不属于子类数据类型。

####多态的好处

比方说在外部编写一个函数，接收Aniaml类型的变量作参数：

    def run_twice(animal):
        animal.run()
        animal.run()

当传入Animal类的实例时就执行animal的run方法，当传入Dog类的实例时也能执行Dog类的run方法，非常方便：

    >>> run_twice(Animal())
    Animal is running...
    Animal is running...
    >>> run_twice(Dog())
    Dog is running...
    Dog is running...

有了多态的特性：

1.**我们不需要为每个子类都在外部重写一个函数**，也不需要再修改函数。

2.只要函数**接收父类类型的变量作参数**，不管我们定义多少**子类都能直接使用这个外部函数**。

3.并且传入的任意类型在实际运行时，调用类的方法时都会**调用实际类型的方法**，这就是多态。

####"开闭"原则
- 对扩展开放：允许新增 `Animal` 子类；

- 对修改封闭：不需要修改依赖 `Animal` 类型的 `run_twice()` 等函数。

**多态真正的威力：调用方只管调用，不管细节**，而当我们新增一种Animal的子类时，只要确保 `run()` 方法编写正确，不用管原来的代码是如何调用的。

对于一个变量，我们**只需要知道它是父类类型的变量，无需确切地知道它的子类类型具体是什么**，就可以放心地调用run()方法，而具体调用的 `run()` 方法是作用在Animal、Dog、Cat还是Tortoise对象上，**由运行时该对象的确切类型决定**。

####静态语言 VS 动态语言
对于静态语言(如：Java)，如果函数需要传入 `Animal` 类型，则传入的参数必须死 `Animal` 类型或者它的子类类型，否则无法调用 `run()` 方法。

对于动态语言而言，则不一定要传入Animal类型。只要保证传入的对象有 `run()` 方法就可以了。 这个特性又称"**鸭子类型**"，即一个对象只需要 "看起来像鸭子，能像鸭子那样走" 就可以了。

    class Timer(object):
        def run(self):
            print('Start...')

比方说这里的Timer类既不属于Animal类型也不继承自Animal类，但它的实例依然可以传入 `run_twice()` 函数并且执行自己的 `run()` 方法。

Python的 "**file-like object**" 就是一种鸭子类型。真正的文件对象有一个read()方法，能返回其内容。 但是，许多对象，只要有read()方法，都被视为 "**file-like object**" 。

许多函数接收的参数就是 "**file-like object**" ，不一定要传入真正的文件对象，可以**传入任何实现了read()方法的对象**。

####小结

- 继承可以把父类的所有功能都直接拿过来，这样就**不必从零做起**，子类只需要**新增自己特有的方法，把父类不合适的方法覆盖重写**。

- 动态语言的鸭子类型特点决定了**继承不像静态语言那样是必须的**。

###获取对象信息
***

这一小节主要介绍给定一个对象，如何了解对象的类型以及有哪些方法。

1.**`type()` 函数：**

    >>> type('str')
    <class 'str'>
    >>> type(None)
    <type(None) 'NoneType'>
    >>> type(abs)
    <class 'builtin_function_or_method'>
    >>> type(a)
    <class '__main__.Animal'>

不仅可以判断基本类型，还可以判断函数和类。 `type()` 函数本身**返回的是type类型的对象**，**值是参数对应的Class**。

    >>> type(type('123'))
    <class 'type'>
    >>> type('123')==str
    True

判断一个对象是否函数可以借助 `types` 模块定义的常量：

    >>> import types
    >>> def fn():
    ...     pass
    ...
    >>> type(fn)==types.FunctionType
    True
    >>> type(abs)==types.BuiltinFunctionType
    True
    >>> type(lambda x: x)==types.LambdaType
    True
    >>> type((x for x in range(10)))==types.GeneratorType
    True

2.**`isinstance()` 函数：**

对于类的继承关系来说，`type()` 函数不合适。 使用 `isinstance()` 函数。 子类的实例也能看作是父类的实例。

前面用 `type()` 函数判断类型，这里用 `isinstance()` 函数也能达到一样的效果：

    >>> isinstance('a', str)
    True
    >>> isinstance(123, int)
    True
    >>> isinstance(b'a', bytes)
    True
    >>> isinstance([1, 2, 3], (list, tuple))
    True

并且参数二还能是一个tuple，此时 `isinstance()` 函数将判断参数一是否属于参数二tuple中**所有类型的其中一种**，是则返回 `True`。

3.**`dir()` 函数：**

`dir()` 函数返回一个对象的所有属性和方法：

    >>> dir('ABC')
    ['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']

形如 `__xxx__` 的属性和方法都是有特殊用途的，比如 `__len__` 方法会返回对象长度，还可以调用 `len()` 函数获取。 实际上，len()函数内部就是在调用该对象的 `__len__()` 方法。

    >>> len('ABC')
    3
    >>> 'ABC'.__len__()
    3

实际上，上面两句代码是等价的。 如果自己写的类想用 `len()` 函数获取长度可以**自己定义一个 `__len__()` 方法。**

    >>> class MyDog(object):
    ...     def __len__(self):
    ...         return 100
    ...
    >>> dog = MyDog()
    >>> len(dog)
    100

4.**`getattr()` 函数、 `setattr()` 函数、 `hasattr()` 函数：**

这几个函数允许我们直接操作一个对象的状态：

    >>> class MyObject(object):
    ...     def __init__(self):
    ...         self.x = 9
    ...     def power(self):
    ...         return self.x * self.x
    ...
    >>> obj = MyObject()

测试属性：

    >>> hasattr(obj, 'x') # 有属性'x'吗？
    True
    >>> obj.x
    9
    >>> hasattr(obj, 'y') # 有属性'y'吗？
    False
    >>> setattr(obj, 'y', 19) # 设置一个属性'y'
    >>> hasattr(obj, 'y') # 有属性'y'吗？
    True
    >>> getattr(obj, 'y') # 获取属性'y'
    19
    >>> obj.y # 获取属性'y'
    19

如果试图获取不存在的属性，会抛出AttributeError的错误：

    >>> getattr(obj, 'z') # 获取属性'z'
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'MyObject' object has no attribute 'z'

可以传入一个default参数，如果属性不存在，就返回默认值,但仅仅是返回，不会把这个没有的属性绑定到对象：

    >>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
    404

除了获取属性还可以获取方法，并且赋值到变量，然后通过变量使用：

    >>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
    >>> fn # fn指向obj.power
    <bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
    >>> fn() # 调用fn()与调用obj.power()是一样的
    81

**Notice**:

只有在不知道对象信息的时候，才要获取对象信息。如果可以直接写： `sum = obj.x + obj.y` 就不要写： `sum = getattr(obj, 'x') + getattr(obj, 'y')`。

比方说读取对象fp，首先判断fp是否有   `read()` 方法，有则fp是流对象，可以读取：

    def readSomething(fp):
        if hasattr(fp, 'read'):
            return readData(fp)
        return None

###实例属性和类属性
***

类属性是属于一个类的，类的所有实例都可以访问到。 注意和在 `__init__` 函数中定义的区别开来:

    >>> class Student(object):
    ...     name = 'Student'
    ...
    >>> s = Student() # 创建实例s
    >>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
    Student
    >>> print(Student.name) # 打印类的name属性
    Student
    >>> s.name = 'Michael' # 给实例绑定name属性
    >>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
    Michael
    >>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
    Student
    >>> del s.name # 如果删除实例的name属性
    >>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
    Student

实例属性的绑定可以通过：

1. 给实例绑定一个属性。
2. 通过 `__init__` 函数，在创建实例时利用 `self` 变量进行绑定。 如: `self.name = name`。

**Notice**：###使用 \_\_slots\_\_
***

####绑定方法
前面提到，基于Python的动态语言特性，可以在定义类之后再给类的实例或者类绑定属性。 事实上，绑定方法(函数)也是可以的，注意**绑定在实例名上**(只有这个实例可用)和**绑定在类名上**(该类的全部实例可用)的区别。

绑定方法有三种方式：

- 直接在类中定义方法
- 像绑定属性一样绑定(右值可以是指向函数的变量名)，并且**只能绑定到类**，绑定到实例则self参数系统无法自动传入，要用 `实例名.方法名(实例名)` 的方式才能正常使用。
- 通过 `MethodType` 函数动态**创建**方法

第三种方法：

    class Student(object):
        pass
    >>> def set_age(self, age): # 定义一个函数作为实例方法
    ...     self.age = age
    ...
    >>> from types import MethodType
    >>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
    >>> s.set_age(25) # 调用实例方法
    >>> s.age # 测试结果
    25

注意第二种方法绑定到实例和第三种方法绑定到实例，作用范围都仅仅是绑定的实例，其他实例无法调用该方法。

如果想给所有实例都绑定方法，可以作用到类名上：

    ` Student.set_score = MethodType(set_score, Student)`

####\_\_slots\_\_
`__slots` 变量是一个特殊的类属性，用于**限制类可以添加的属性**。

    class Student(object):
        __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

    >>> s = Student() # 创建新的实例
    >>> s.name = 'Michael' # 绑定属性'name'
    >>> s.age = 25 # 绑定属性'age'
    >>> s.score = 99 # 绑定属性'score'
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'Student' object has no attribute 'score'

可以看到score属性没有写到 `__slots__` 里面，所以就无法绑定到Student类。

    >>> class GraduateStudent(Student):
    ...     pass
    ...
    >>> g = GraduateStudent()
    >>> g.score = 9999

注意！ `__slots__` 定义的属性限制仅对当前类的实例有效，**对继承的子类无效**。 但是！ 如果集成的子类也定义了 `__slots__`，那么子类实例允许添加的属性范围就等于**自身的 `__slots__` 加上父类的 `__slots__`**。


- **实例属性和类属性不要重名！**否则通过实例查看这个属性时实例属性就会覆盖掉类属性。 当然，如果删除了实例属性，类属性就能正常访问了。

- **修改类属性必须是** `类名.类属性名=新值`，不能通过实例的变量名来改，否则只是给实例绑定了一个实例属性。

##面向对象高级编程
前面的章节介绍了OOP最基础的数据封装、继承和多态3个概念。 在Python中OOP还有很多更高级的特性，这一章会讨论多重继承、定制类、元类等概念。

###使用 \_\_slots\_\_
***

####绑定方法
前面提到，基于Python的动态语言特性，可以在定义类之后再给类的实例或者类绑定属性。 事实上，绑定方法(函数)也是可以的，注意**绑定在实例名上**(只有这个实例可用)和**绑定在类名上**(该类的全部实例可用)的区别。

绑定方法有三种方式：

- 直接在类中定义方法
- 像绑定属性一样绑定(右值可以是指向函数的变量名)，并且**只能绑定到类**，绑定到实例则self参数系统无法自动传入，要用 `实例名.方法名(实例名)` 的方式才能正常使用。
- 通过 `MethodType` 函数动态**创建**方法

第三种方法：

    class Student(object):
        pass
    >>> def set_age(self, age): # 定义一个函数作为实例方法
    ...     self.age = age
    ...
    >>> from types import MethodType
    >>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
    >>> s.set_age(25) # 调用实例方法
    >>> s.age # 测试结果
    25

注意第二种方法绑定到实例和第三种方法绑定到实例，作用范围都仅仅是绑定的实例，其他实例无法调用该方法。

如果想给所有实例都绑定方法，可以作用到类名上：

    ` Student.set_score = MethodType(set_score, Student)`

####\_\_slots\_\_
`__slots` 变量是一个特殊的类属性，用于**限制类可以添加的属性**。

    class Student(object):
        __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

    >>> s = Student() # 创建新的实例
    >>> s.name = 'Michael' # 绑定属性'name'
    >>> s.age = 25 # 绑定属性'age'
    >>> s.score = 99 # 绑定属性'score'
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'Student' object has no attribute 'score'

可以看到score属性没有写到 `__slots__` 里面，所以就无法绑定到Student类。

    >>> class GraduateStudent(Student):
    ...     pass
    ...
    >>> g = GraduateStudent()
    >>> g.score = 9999

注意！ `__slots__` 定义的属性限制仅对当前类的实例有效，**对继承的子类无效**。 但是！ 如果集成的子类也定义了 `__slots__`，那么子类实例允许添加的属性范围就等于**自身的 `__slots__` 加上父类的 `__slots__`**。

###使用@property
***

绑定属性时，属性的值可以随意设置，无法检查参数，这样很不好。 针对这个问题，有两种解决思路：

1.通过方法来设置属性，这样就可以在方法里面检查参数：

    class Student(object):

        def get_score(self):
            return self._score

        def set_score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value

2.使用Python内置的 `@property` **装饰器**，把一个方法**变成属性**来调用。 这样比起通过方法来设置属性更简单直接：

    class Student(object):

        @property
        def score(self):
            return self._score

        @score.setter
        def score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value

`@property` 实现起来稍微复杂，但很好理解，通过类方法来获取/设置属性时我们会使用**getter方法和setter方法**。

这里使用 `@property` 之后*getter方法* 就**变成了属性**。 原本要用 `变量名.getter方法名()` 的方式来获取属性值，现在**只需要像使用属性一样** `变量名.属性名` 就能返回属性值了~

使用了 `@property` 之后，对于*setter方法*就使用 `属性名.setter` 这样的装饰器。 原本要用 `变量名.setter方法名(新属性值)` 的方式来设置属性值，现在**只需要像使用属性一样** `变量名.属性名 = 新属性值` 就能设置属性值了~

    >>> s = Student()
    >>> s.score = 60 # OK，实际转化为s.set_score(60)
    >>> s.score # OK，实际转化为s.get_score()
    60
    >>> s.score = 9999
    Traceback (most recent call last):
      ...
    ValueError: score must between 0 ~ 100!

利用 `@property` ，我们就能够给类绑定**可控的属性**了。 实际上，我们依然是通过**getter方法和setter方法**来完成值的控制，只是**加上了装饰器**。

    class Student(object):

        @property
        def birth(self):
            return self._birth

        @birth.setter
        def birth(self, value):
            self._birth = value

        @property
        def age(self):
            return 2015 - self._birth

还可以不定义setter方法，如上面的age，这样getter方法转换成的属性就是一个**只读属性**，而birth则是一个**可读写属性**。 age是在获取时自动根据birth计算的，所以只用getter方法就可以了。

####小结

`@property` 广泛应用在类的定义中，可以让调用者写出简短的代码，同时**保证对参数进行必要的检查**，这样，程序运行时就**减少了出错的可能性**。

**Notice**:

这种方法本质上其实还是调用方法，所以**getter和setter方法名和实际的属性名必须不同**！ 可以看到上面例子中属性名
都是**前缀下划线**的，当然，取其他名也可以。

###多重继承
***

相比于Java的单一继承，Python允许多重继承，**子类可以继承多个父类并获得所有父类的功能**。(Java其实也能用接口来实现多重继承，也即使用implements，但接口类是有要求的。)

####为什么要用到多重继承？

比方说对动物进行分类。 我们可以分出哺乳类、鸟类等等；也可以分出可飞行不可飞行等等；还可以分出可作宠物不可作宠物等等。

如果只能单一继承，那么类的层次会非常复杂，我们要设置非常多的层次。 因为哺乳类和鸟类都分可飞行不可飞行，这样就分出四个子类，而这四个子类再要根据可作宠物不可作宠物分就分成了八个字类。这样下去，**类的数目会呈指数增长**。

![单一继承](http://www.liaoxuefeng.com/files/attachments/0013946304409926336fd4395ef4ce1809253a1d87dd2fe000/0)

采用多重继承后，一个子类可以继承多个父类就避免了过多无意义的重复。

    class Animal(object):
        pass

    # 大类:
    class Mammal(Animal):
        pass

    class Bird(Animal):
        pass

    class Runnable(object):
        def run(self):
            print('Running...')

    class Flyable(object):
        def fly(self):
            print('Flying...')

    class Dog(Mammal, Runnable):
        pass

    class Bat(Mammal, Flyable):
        pass

####MixIn

上面的代码就运用了 `MixIn` 这种设计模式。 在设计类的继承关系时，一般主线都是单一继承的，比如 `Dog` 继承 `Mammal` 类。 而我们又常常需要**混入额外的功能**，使得 `Dog` 还可以继承 `Runnable` 和 `Pet` 等等的类。 这种设计就称为 `MixIn`，旨在给类添加多个功能。

这样做的好处是**不需要复杂而庞大的继承链**，只要**选择组合不同的类的功能**，就可以快速构建出所需的子类。

**Notice**：

1. 在实际使用中往往采用 `功能名+MixIn`
 来命名添加的功能类，如： `RunnableMixIn` 和 `FlyableMixIn` 这样命名更加清晰。

2. Python的多重继承存在**变量名重复**的缺陷，如果子类和父类有重复变量名会出错。 我们在写代码时必须避免。

3. 继承多个父类时，会有一个优先继承的区别，但继承的多个父类都有一个同名类方法时，子类实例(无定义该类方法)调用该方法会**执行第一个继承的父类的类方法**。

###定制类
***

这一小节介绍一些有**特殊用途的类方法**，我们可以通过自定义的方式实现这些类方法，使得它们更符合我们需求的功能。

- **\_\_str\_\_**

`__str__` 方法定义的是我们**使用print()函数打印该类实例时显示的字符串**。 比方说：

    >>> class Student(object):
    ...     def __init__(self, name):
    ...         self.name = name
    ...
    >>> print(Student('Michael'))
    <__main__.Student object at 0x109afb190>

默认情况下打印的是 `模块名.类名 object at 内存位置`。 我们可以自定义修改打印的内容：

     >>> class Student(object):
    ...     def __init__(self, name):
    ...         self.name = name
    ...     def __str__(self):
    ...         return 'Student object (name: %s)' % self.name
    ...
    >>> print(Student('Michael'))
    Student object (name: Michael)

- **\_\_repr\_\_**

`__repr__` 方法定义的是**输入实例变量名返回的字符串**。 默认情况下和 `__str__` 返回一样的内容，但自定义 `__str__` 不会改变 `__repr__` 返回的内容，所以这里s打印的没变。

    >>> s = Student('Michael')
    >>> s
    <__main__.Student object at 0x109afb310>

自定义时，如果希望 `__str__` 和 `__repr__` 内容一致，可以简单把后者赋值为前者。

    class Student(object):
        def __init__(self, name):
            self.name = name
        def __str__(self):
            return 'Student object (name=%s)' % self.name
        __repr__ = __str__

**Notice**：

两者的区别是 `__str__()` 返回**用户看到的字符串**，而 `__repr__()` 返回**程序开发者看到的字符串**，也就是说，`__repr__()` 是为调试服务的。


- **\_\_iter\_\_**

如果一个类想被用于 `for ... in` 循环，类似list或tuple那样，就必须实现一个 `__iter__()` 方法，该方法返回一个迭代对象，然后，Python的**for循环**就会**不断调用该迭代对象的 `__next__()` 方法拿到循环的下一个值**，直到遇到 `StopIteration` 错误时退出循环。

    class Fib(object):
        def __init__(self):
            self.a, self.b = 0, 1 # 初始化两个计数器a，b

        def __iter__(self):
            return self # 实例本身就是迭代对象，故返回自己

        def __next__(self):
            self.a, self.b = self.b, self.a + self.b # 计算下一个值
            if self.a > 5: # 退出循环的条件
                raise StopIteration();
            return self.a # 返回下一个值

把for循环作用在Fib类的实例上：

    >>> for n in Fib():
    ...     print(n)
    ...
    1
    1
    2
    3
    5

- **\_\_getitem\_\_**

虽然定义了 `__iter__` 和 `__next__` 方法之后可以使用for循环来调用类的实例。 但是**直接使用下标来访问**还是不行：

    >>> Fib()[5]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'Fib' object does not support indexing

`__getitem__` 定义的就是令实例拥有可以像list一样通过下标取出元素的功能：

    class Fib(object):
        def __getitem__(self, n):
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a

    >>> Fib()[5]
    8

####切片
记得前面章节提到list还支持切片操作，但这里不行：

    >>> Fib()[5:8]
    Traceback (most recent call last):
      File "<pyshell#24>", line 1, in <module>
        Fib()[5:8]
      File "<pyshell#20>", line 4, in __getitem__
        for x in range(n):
    TypeError: 'slice' object cannot be interpreted as an integer

原因是 `__getitem__` 传入的参数可能是一个直接的下标(int类型对象)，也可能是一个切片(slice对象)。 要对这两种参数做出判断才行：

    class Fib(object):
        def __getitem__(self, n):
            if isinstance(n, int): # n是索引
                a, b = 1, 1
                for x in range(n):
                    a, b = b, a + b
                return a
            if isinstance(n, slice): # n是切片
                start = n.start
                stop = n.stop
                if start is None:
                    start = 0
                a, b = 1, 1
                L = []
                for x in range(stop):
                    if x >= start:
                        L.append(a)
                    a, b = b, a + b
                return L

这样定义后再使用切片就能正常获取结果了：

    >>> f = Fib()
    >>> f[0:5]
    [1, 1, 2, 3, 5]
    >>> f[:10]
    [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]

**Notice**:

1. 这里还未对step和负数下标进行处理，不妨自己增加。

2. `__getitem__()` 不仅仅能模仿list和tuple，**也能模仿dict**。 也就是说它的参数可以是作为key的对象，然后提取出value。

3. 除了 `__getitem__()` 之外，我们还可以自定义 `__setitem__()` 和 `__delitem__()` 方法用于设置某一项/删除某一项。

- **\_\_getattr\_\_**

正常情况下，当我们调用类的方法或属性时，如果不存在，就会报错。 但是通过自定义 `__getattr__` 方法，我们可以**动态返回一个不存在的属性**。

    class Student(object):

        def __init__(self):
            self.name = 'Michael'

        def __getattr__(self, attr):
            if attr=='score':
                return 99

比方说上面这个例子，Student类没有score这个属性，当**调用不存在的属性时**，比如score，Python解释器**会试图调用`__getattr__(self, 'score')` 来尝试获得属性**，这样，我们就有机会返回score的值。

    >>> s = Student()
    >>> s.name
    'Michael'
    >>> s.score
    99
    >>> print(s.abc)
    None

注意！ 此时调用没有的属性返回的是None，还是不符合，应该改为查找没有的属性时抛出错误。

并且 `__getattr__(self, attr)` 还可以返回一个函数，比如：

    class Student(object):
        def __getattr__(self, attr):
            if attr=='age':
                return lambda: 25
            raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)

自然，调用时也要改为函数的调用方式，实际是返回了一个函数对象，然后执行这个返回的函数:

    >>> s.age()
    25

####动态返回的优势和应用

举个例子，很多网站都有REST API，调用API的URL类似：

    http://api.server/user/friends
    http://api.server/user/timeline/list

写SDK时，如果对每个URL对应的API都写一个类方法，那得累死，而且API一旦改动，就要重写SDK，借助 `__getattr__(self, attr)` 的动态特性就可以：

    class Chain(object):

        def __init__(self, path=''):
            self._path = path

        def __getattr__(self, path):
            return Chain('%s/%s' % (self._path, path))

        def __str__(self):
            return self._path

        __repr__ = __str__

    >>> Chain().status.user.timeline.list
    '/status/user/timeline/list'

可以动态地生成一个path，而不用每个path写一个类方法，这样就非常方便简单了。

- **\_\_call\_\_**

`__call__` 方法允许我们把实例对象作为一个方法来调用。 我们调用类方法时是以 `实例名.方法名()`来调用的，而 `__call__` 方法则使得我们可以以 `实例名()` 的方式调用一个方法。

    class Student(object):
        def __init__(self, name):
            self.name = name

        def __call__(self):
            print('My name is %s.' % self.name)

并且`__call__` 方法还可以定义参数，调用时就好像我们调用其他函数的方式一样。

####判断一个对象是否函数
使用 `callable()` 函数判断是否**可调用对象**：

    >>> callable(Student())
    True
    >>> callable(max)
    True
    >>> callable([1, 2, 3])
    False
    >>> callable(None)
    False
    >>> callable('str')
    False

###使用枚举类
***

在笔记起初的章节提到，在Python中定义常量是通过全大写的变量名定义，但**本质上还是变量**，可能会被误操作影响。 更好的方法是利用枚举类型(Eunm类)来创建一个新类，**每个常量都是类的一个唯一实例**。

    from enum import Enum

    Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

    >>> Month.Jan
    <Month.Jan: 1>
    >>> Month.Jan.name
    'Jan'
    >>> Month.Jan.value
    1
    >>> Month.__members__
    mappingproxy(OrderedDict([('Jan', <Month.Jan: 1>), ('Feb', <Month.Feb: 2>), ('Mar', <Month.Mar: 3>), ('Apr', <Month.Apr: 4>), ('May', <Month.May: 5>), ('Jun', <Month.Jun: 6>), ('Jul', <Month.Jul: 7>), ('Aug', <Month.Aug: 8>), ('Sep', <Month.Sep: 9>), ('Oct', <Month.Oct: 10>), ('Nov', <Month.Nov: 11>), ('Dec', <Month.Dec: 12>)]))
    >>> Month['Jan']
    <Month.Jan: 1>

可以直接通过 `.` 运算符来引用常量，返回的是一个拥有name属性和value属性的枚举类型对象。 其中**value是自动赋给成员的int类型常量，默认从1开始计数**。

也可以使用 `__members__` 来枚举全部成员，返回的是一个**dict**，可以使用dict的方法来遍历和访问。

Enum和Month都是类，Month继承Enum，
各个枚举值是Month的实例，同时也是Enum的实例。

如果我们需要更精确地控制枚举类型，可以自定义类：

    from enum import Enum, unique

    @unique
    class Weekday(Enum):
        Sun = 0 # Sun的value被设定为0
        Mon = 1
        Tue = 2
        Wed = 3
        Thu = 4
        Fri = 5
        Sat = 6

其中 `@unique` 装饰器可以帮助检查枚举的常量的value重复，如有重复，定义时会报错。

####小结

Enum可以把一组相关常量定义在一个class中，**且class不可变**，已经定义，类属性的值也不可再修改，而且成员可以直接比较。

###使用元类
***

首先要理解，Python作为动态语言，对于**函数和类的定义**与静态语言是不同的。 动态语言**不在编译时进行定义**，是在运行时才动态创建的。

比方说把类定义写在 `hello.py` 模块中：

    class Hello(object):
        def hello(self, name='world'):
            print('Hello, %s.' % name)

当Python解释器载入hello模块时，就会**依次执行该模块的所有语句**，执行结果就是**动态创建出一个Hello的class对象**(注意是创建出一个类而非类的实例)。

    >>> from hello import Hello
    >>> h = Hello()
    >>> h.hello()
    Hello, world.
    >>> print(type(Hello))
    <class 'type'>
    >> print(type(h))
    <class 'hello.Hello'>

`type()` 函数可以查看一个类型或变量的类型。 Hello是一个类(class)，它的类型就是 `type`。 h是一个实例，它的类型就是 `Hello`。

####动态创建类

前面说类定义是运行时动态创建的，创建的方法就是使用 `type()` 函数。 `type()` 函数除了前面查看类型的用途，**还可以用来直接创建出新类型**而不需通过 `class Hello(object) ...` 的方式定义。

    >> def fn(self, name='world'): # 先定义函数
    ...     print('Hello, %s.' % name)
    ...
    >>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
    >>> h = Hello()
    >>> h.hello()
    Hello, world.
    >>> print(type(Hello))
    <class 'type'>
    >>> print(type(h))
    <class '__main__.Hello'>

`type()` 函数的三个参数依次是：

1. 类名
2. 继承的父类集合
3. 类属性和类方法

**Notice**：

参数2是一个**tuple**， 注意只有一个父类的写法， **单元素tuple要加上逗号以区分于数学表达式**。

参数3是一个**dict**，这里用 `dict(key1=value1, key2=value2, ...)` 的形式来创建这个类方法名/属性名和对象对应的dict。

这样创造出的类和直接写class完全一样，因为Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用type()函数创建出class。

####元类
元类即metaclass，可以用来**控制类的创建行为**。 可以这样理解元类的概念：

- 要创建类实例，必须先定义类；
- 要创建类，必须先定义元类。

可以把类看成是metaclass的一个“*实例*”。

    # metaclass是类的模板，所以必须从`type`类型派生：
    class ListMetaclass(type):
        def __new__(cls, name, bases, attrs):
            attrs['add'] = lambda self, value: self.append(value)
            return type.__new__(cls, name, bases, attrs)

按照默认习惯，metaclass的类名总是以Metaclass结尾，以便清楚地表示这是一个metaclass。

`__new__()` 方法接收到的参数依次是：

1. 当前准备创建的类的对象；
2. 类的名字；
3. 类继承的父类集合；
4. 类的方法集合。

    class MyList(list, metaclass=ListMetaclass):
        pass

当定义类时传入关键字 `metaclass=元类名` 时，元类就会生效，该例中，`MyList` 类在创建时会通过 `ListMetaclass.__new__()` 来创建。 在这个new方法里，可以修改类定义，加上新方法/属性，然后**返回修改后的类定义**。

    >>> L = MyList()
    >>> L.add(1)
    >> L
    [1]

测试可知add方法确实被加入到创建的MyList类中。 这个在元类中添加的方法称为**魔术方法**。 那么这样**动态修改到底有什么意义**？  显然正常状况下直接在类中写add方法更加简单。

但也有必须通过metaclass修改类定义的情景，比方说**ORM框架**。 ORM全称“**Object Relational Mapping**”，即对象-关系映射。

核心的思想是把关系型数据库的一行映射为一个对象，一个表映射为一个类。 这样写代码时就不需要直接操作SQL语句。 这里不作详细展开，代码实现可看 [ORM的Python实现](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000)。

##错误、调试和测试
错误有可能由代码/用户输入/其他环境因素造成，此时程序会终止运行并退出。 Python内置一套错误处理机制帮助我们处理错误。

调试是对跟踪执行中的程序，查看变量值是否正确。 Python的pdb可以让我们逐步执行代码。

测试是为了保证程序输出符合我们的预期，在程序修改之后依然能反复运行。

###错误处理
***

所有高级语言都内置了一套 `try...except...finally...` 的错误处理机制，Python也不例外，举个例子：

    try:
        print('try...')
        r = 10 / int('a')
        print('result:', r)
    except ValueError as e:
        print('ValueError:', e)
    except ZeroDivisionError as e:
        print('ZeroDivisionError:', e)
    else:
        print('no error!')
    finally:
        print('finally...')
    print('END')

当我们认为某段代码内部可能出错时，使用try来括起，出错时后续代码不再执行而是直接跳转到错误处理代码处，也即except块。

except块捕捉到对应错误类型时会执行。 如果没有错误发生，会执行except块后的esle块。 finally块则是不管有没有错误发生都会在最后执行的一段代码。

使用这种模式而不使用错误码是因为这样写更加方便简洁，试想函数a调用函数b调用函数c，如果c中出错，使用错误码的形式就要一层层return一个自己定义的错误码(比方说用整数-1表示除0错误),然后在能处理错误的函数(比方说这里顶层的a函数)中用if来判断，然后处理。

这样做非常繁琐，而且容易把正常结果和错误码混淆。 而使用 `try...except...finally...` 的机制，就可以实现多层调用，c中出错，只要a中有用try括起调用c的代码就能捕捉到错误，不需要写其他额外的判断。

####错误类型

Python的错误类型其实也是类，所有错误类型都是 `BaseException` 的子类，常见的错误类型和继承关系看这里：[Python错误类型文档](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)

注意如果捕捉一个父类的错误类型，则子类的错误也会被一网打尽，所以except块捕捉的顺序也要注意。

####调用堆栈

前面说道Python的错误处理机制允许多层调用，如果错误没有被捕获，就会一直往上抛，直到被Python解释器捕获并处理，然后程序退出。

    # err.py:
    def foo(s):
        return 10 / int(s)

    def bar(s):
        return foo(s) * 2

    def main():
        bar('0')

    main()

在命令行中运行：

    $ python3 err.py
    Traceback (most recent call last):
      File "err.py", line 11, in <module>
        main()
      File "err.py", line 9, in main
        bar('0')
      File "err.py", line 6, in bar
        return foo(s) * 2
      File "err.py", line 3, in foo
        return 10 / int(s)
    ZeroDivisionError: division by zero

错误信息**由上往下表示整个调用链**，并在最后指出属于何种错误。

####记录错误

上面的例子**没有对错误进行捕获，所以打印错误信息后程序也结束**了。 如果**打印错误信息后程序能继续执行**，可以使用logging模块的功能。

    # err_logging.py
    import logging

    def foo(s):
        return 10 / int(s)

    def bar(s):
        return foo(s) * 2

    def main():
        try:
            bar('0')
        except Exception as e:
            logging.exception(e)

    main()
    print('END')

在命令行中执行：

    $ python3 err_logging.py
    ERROR:root:division by zero
    Traceback (most recent call last):
      File "err_logging.py", line 13, in main
        bar('0')
      File "err_logging.py", line 9, in bar
        return foo(s) * 2
      File "err_logging.py", line 6, in foo
        return 10 / int(s)
    ZeroDivisionError: division by zero
    END

可以看到例子中捕获了错误，并且使用logging进行记录，打印完错误信息之后继续执行程序并输出END。 **实际中，我们往往还会通过一定的配置，使用logging把错误信息记录到日志文件中，方便事后排查**。

####抛出错误

错误其实是一个class，捕获错误捕获到的其实就是该class的一个实例。 所以错误其实是**有意创建并抛出**的，Python的内置函数会抛出很多类型的错误，我们也可以自己编写错误类型并抛出：

    # err_raise.py
    class FooError(ValueError):
    pass

    def foo(s):
        n = int(s)
        if n==0:
            raise FooError('invalid value: %s' % s)
        return 10 / n

    foo('0')

在命令行中运行：

    $ python3 err_raise.py
    Traceback (most recent call last):
      File "err_throw.py", line 11, in <module>
        foo('0')
      File "err_throw.py", line 8, in foo
        raise FooError('invalid value: %s' % s)
    __main__.FooError: invalid value: 0

既然Python本身已有丰富的错误类型，我们就不需要自己额外定义，能使用Python内置的就尽量使用。

这里再提及一种错误处理的方式：

    # err_reraise.py

    def foo(s):
        n = int(s)
        if n==0:
            raise ValueError('invalid value: %s' % s)
        return 10 / n

    def bar():
        try:
            foo('0')
        except ValueError as e:
            print('ValueError!')
            raise

    bar()

这段例子中，在上层的bar函数中捕获了错误并且打印，然后**又再次抛出这个错误**。

这是因为捕获错误仅仅是为了进行记录便于后期追踪。 如果当前函数不知道如何处理该错误就应该继续往上抛，**让顶层调用者去处理**。

如果raise语句不带参数，就把当前错误原样抛出。 带上参数的话完全可以做到**修改错误类型及错误信息**。 但注意要符合逻辑。

自己编写的程序，应当注意**在文档中指出**会抛出哪些错误及错误产生的原因。

###调试
***

调试可以理解为跟踪运行中的程序和变量值。 最笨的方法就是用 `print()` 把变量值打印出来，这样做的坏处很明显，我们在完成调试后还得删除掉这样 `print()` 语句，否则程序里到处都是打印的垃圾信息。 Python提供以下几种机制帮助调试：

    def foo(s):
        n = int(s)
        print('>>> n = %d' % n)
        return 10 / n

####断言

把所有用 `print()` 查看变量的地方改为用assert来代替：

    def foo(s):
        n = int(s)
        assert n != 0, 'n is zero!'
        return 10 / n

语法是 `assert 表达式, '错误信息'`， 运行时会判断是否满足表达式，满足则继续，不满足则抛出 `AssertionError`并打印错误信息。

但是这也不是一个很好的解决方案，到处都是assert效果和 `print()` 差不多。 不过启动Python解释器时可以用 `-O` 参数来关闭assert。 注意是字母O不是数字0。 即 `$ python3 -O err.py`，这时，可以把所有assert语句看作pass。

####记录

使用logging更优，因为logging不会抛出错误，而且**可以把信息输出到文件**。

    import logging
    logging.basicConfig(level=logging.INFO)

    s = '0'
    n = int(s)
    logging.info('n = %d' % n)
    print(10 / n)

这里第二行的语句是对logging进行配置，否则不会输出任何信息。 logging的好处是**允许记录信息的级别**，按程度由低到高有 `debug`, `info`, `waring`, `error` 等等。 如果配置等级为 `error` 则 `debug`, `info`, `waring`这三个等级的信息不会输出。 这就类似于java中的 `Log.e(标识,错误信息)` 这种用法。

####调试器
Python自带调试器pdb，能让程序以以单步方式运行，可以随时查看运行状态。

在命令行中以 `python -m pdb 文件名.py` 的方式执行文件即可进入Pdb模式，该模式下输入l可以查看代码，输入n会执行下一行代码，输入 `p 变量名` 可以查看变量值，输入q会结束调试，退出程序。

比方说写一个最简单的程序：

    a=1
    print(a)
    b='aa'
    print(b)

调试：

    f:\Python35>python -m pdb test.py
    > f:\python35\test.py(1)<module>()
    -> a=1
    (Pdb) l
      1  -> a=1
      2     print(a)
      3     b='aa'
      4     print(b)
    [EOF]
    (Pdb) n
    > f:\python35\test.py(2)<module>()
    -> print(a)
    (Pdb) n
    1
    > f:\python35\test.py(3)<module>()
    -> b='aa'
    (Pdb) q

    f:\Python35>

####设置断点

Pdb的功能很方便，但我们并不是每次都想逐步执行，Python又提供了设置断点的功能，这需要我们在代码中进行标记。

需要载入pdb这个模块，然后用 `pdb.set_trace()` 在需要的地方设置断点，这样运行程序时就会在这个地方**暂停并进入pdb调试环境**。

依然写个简单的例子：

    import pdb
    a=1
    print(a)
    pdb.set_trace()
    b='aa'
    print(b)

执行时：

    f:\Python35>python test.py
    1
    > f:\python35\test.py(5)<module>()
    -> b='aa'
    (Pdb) l
      1     import pdb
      2     a=1
      3     print(a)
      4     pdb.set_trace()
      5  -> b='aa'###单元测试
***

**单元测试是用来对一个模块、一个函数或者一个类进行正确性检验的测试工作**。 是测试驱动开发(TDD)模式的重点，能保证程序模块的行为符合我们的期望。

比如对函数abs()，我们可以编写出以下几个测试用例：

1. 输入正数，比如1、1.2、0.99，期待返回值与输入相同；
2. 输入负数，比如-1、-1.2、-0.99，期待返回值与输入相反；
3. 输入0，期待返回0；
4. 输入非数值类型，比如None、[]、{}，期待抛出TypeError。

**把上面的测试用例放在一个测试模块里，就是一个完整的单元测试**。

如果单元测试通过，说明这个函数能正常工作，否则代码有bug要继续修改。

单元测试通过后，如果再次对单元进行修改，只需再跑一边单元测试就可以了。 通过说明修改后行为一致， 不能通过说明修改后行为有变化，要么继续改代码，要么改测试。

这里通过编写一个Dict类来作例子，允许像dict类一样操作，并且可以通过属性来访问key对应的值，期望如下：

    >>> d = Dict(a=1, b=2)
    >>> d['a']
    1
    >>> d.a
    1

首先把Dict类定义写在 `mydict.py` 文件中：

    class Dict(dict):

        def __init__(self, **kw):
            super().__init__(**kw)

        def __getattr__(self, key):
            try:
                return self[key]
            except KeyError:
                raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

        def __setattr__(self, key, value):
            self[key] = value

这里基本上都是继承了父类dict的功能，`super().方法名` 即调用父类的该方法。 在Dict中只是增加了使用 `.` 符进行属性访问时，把属性作为字典的key，然后返回value，设置操作也同理。

编写单元测试文件要引入Python自带的unittest模块，把单元测试写在 `mydict_test.py` 文件中：

    import unittest

    from mydict import Dict

    class TestDict(unittest.TestCase):

        def test_init(self):
            d = Dict(a=1, b='test')
            self.assertEqual(d.a, 1)
            self.assertEqual(d.b, 'test')
            self.assertTrue(isinstance(d, dict))

        def test_key(self):
            d = Dict()
            d['key'] = 'value'
            self.assertEqual(d.key, 'value')

        def test_attr(self):
            d = Dict()
            d.key = 'value'
            self.assertTrue('key' in d)
            self.assertEqual(d['key'], 'value')

        def test_keyerror(self): #访问不存在的key应抛出KeyError
            d = Dict()
            with self.assertRaises(KeyError):
                value = d['empty']

        def test_attrerror(self):  #访问不存在的属性应抛出AttributeError
            d = Dict()
            with self.assertRaises(AttributeError):
                value = d.empty

编写单元测试实际是实现一个测试类，这个类继承自 `unnittest.Testcase` 类。 `Testcase` 这个父类内置了很多条件判断(断言)方式，我们可以调用它们来进行测试。

测试类中我们针对每一类测试都写一个 `test_xxx(self)` 方法，在方法中测试功能并且用断言判断输出是否符合期望。

最常用的断言有两种：

一是 `assertEqual()`:

这种断言用于测试输出值是确定的情况。

二是 `with assertRaises(错误类型): 代码块`:

这种断言用来测试运行出错时是否能够正确抛出错误。 执行时会执行冒号后代码块内容，如果代码块有错就会抛出错误，错误类型与断言一致就通过测试。


      6     print(b)
    [EOF]
    (Pdb) p a
    1
    (Pdb) n
    > f:\python35\test.py(6)<module>()
    -> print(b)
    (Pdb) c
    aa

    f:\Python35>

可以看到断点前的语句都会执行，然后到达断点时暂停程序变为逐步执行模式。 如果**输入c就会执行后续代码直到遇到下一断点或者程序结束**。

####小结

使用IDE可以更方便地设置断点和执行调试，但最好的调试方式还是使用logging。

###单元测试
***

**单元测试是用来对一个模块、一个函数或者一个类进行正确性检验的测试工作**。 是测试驱动开发(TDD)模式的重点，能保证程序模块的行为符合我们的期望。

比如对函数abs()，我们可以编写出以下几个测试用例：

1. 输入正数，比如1、1.2、0.99，期待返回值与输入相同；
2. 输入负数，比如-1、-1.2、-0.99，期待返回值与输入相反；
3. 输入0，期待返回0；
4. 输入非数值类型，比如None、[]、{}，期待抛出TypeError。

**把上面的测试用例放在一个测试模块里，就是一个完整的单元测试**。

如果单元测试通过，说明这个函数能正常工作，否则代码有bug要继续修改。

单元测试通过后，如果再次对单元进行修改，只需再跑一边单元测试就可以了。 通过说明修改后行为一致， 不能通过说明修改后行为有变化，要么继续改代码，要么改测试。

这里通过编写一个Dict类来作例子，允许像dict类一样操作，并且可以通过属性来访问key对应的值，期望如下：

    >>> d = Dict(a=1, b=2)
    >>> d['a']
    1
    >>> d.a
    1

首先把Dict类定义写在 `mydict.py` 文件中：

    class Dict(dict):

        def __init__(self, **kw):
            super().__init__(**kw)

        def __getattr__(self, key):
            try:
                return self[key]
            except KeyError:
                raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

        def __setattr__(self, key, value):
            self[key] = value

这里基本上都是继承了父类dict的功能，`super().方法名` 即调用父类的该方法。 在Dict中只是增加了使用 `.` 符进行属性访问时，把属性作为字典的key，然后返回value，设置操作也同理。

编写单元测试文件要引入Python自带的unittest模块，把单元测试写在 `mydict_test.py` 文件中：

    import unittest

    from mydict import Dict

    class TestDict(unittest.TestCase):

        def test_init(self):
            d = Dict(a=1, b='test')
            self.assertEqual(d.a, 1)
            self.assertEqual(d.b, 'test')
            self.assertTrue(isinstance(d, dict))

        def test_key(self):
            d = Dict()
            d['key'] = 'value'
            self.assertEqual(d.key, 'value')

        def test_attr(self):
            d = Dict()
            d.key = 'value'
            self.assertTrue('key' in d)
            self.assertEqual(d['key'], 'value')

        def test_keyerror(self): #访问不存在的key应抛出KeyError
            d = Dict()
            with self.assertRaises(KeyError):
                value = d['empty']

        def test_attrerror(self):  #访问不存在的属性应抛出AttributeError
            d = Dict()
            with self.assertRaises(AttributeError):
                value = d.empty

编写单元测试实际是实现一个测试类，这个类继承自 `unnittest.Testcase` 类。 `Testcase` 这个父类内置了很多条件判断(断言)方式，我们可以调用它们来进行测试。

测试类中我们针对每一类测试都写一个 `test_xxx(self)` 方法，在方法中测试功能并且用断言判断输出是否符合期望。

最常用的断言有两种：

一是 `assertEqual()`:

这种断言用于测试输出值是确定的情况。

二是 `with assertRaises(错误类型): 代码块`:

这种断言用来测试运行出错时是否能够正确抛出错误。 执行时会执行冒号后代码块内容，如果代码块有错就会抛出错误，错误类型与断言一致就通过测试。

####运行单元测试

写好单元测试文件后需要运行以检测编写的单元是否能正常运行。 一般来说有两种运行单元测试的方式。

一是在 `mydict_test.py` 最后加上两行代码：

    if __name__ == '__main__':
        unittest.main()

这样就可以直接把单元测试当作Python脚本运行。

    f:\Python35>python mydict_test.py
    .....
    ----------------------------------------------------------------------
    Ran 5 tests in 0.001s

    OK

二是在命令行通过参数 `-m unittest` 直接运行单元测试：

    f:\Python35>python -m unittest mydict_test.py
    .....
    ----------------------------------------------------------------------
    Ran 5 tests in 0.001s

    OK

后者更为推荐，因为这样可以一次批量运行很多单元测试，并且有很多工具可以自动运行这些单元测试。

####特殊方法
可以在测试类中编写两个特殊方法 `setUp()` 和 `tearDown()`。 这两个方法分别在每个测试方法调用前和调用后执行。

假设测试方法要求连接一个数据库，那么测试前就要先连接数据库，而测试结束则要求关闭连接。 在 `setUp()` 中实现连接， 在 `tearDown()` 中实现断开就**避免了在每个测试方法中都要编写重复的代码**，这样我们只需要调用这两个函数就可以了。

    class TestDict(unittest.TestCase):

        ...

        def setUp(self):
            print('setUp...')

        def tearDown(self):
            print('tearDown...')

        ...

**Notice**：

1.这两个方法**不用特别写调用语句**，只要测试类中定义了，那么单元测试运行时每个测试都会调用这两个方法。

    f:\Python35>python -m unittest  mydict_test.py
    setUp...
    tearDown...
    .setUp...
    tearDown...
    .setUp...
    tearDown...
    .setUp...
    tearDown...
    .setUp...
    tearDown...
    .
    ----------------------------------------------------------------------
    Ran 5 tests in 0.010s

    OK

2.单元测试通过并不意味这代码无bug，但不通过的程序就肯定有bug。

3.单元测试用例要覆盖常用的输入组合，边界条件和异常。

4.单元测试代码要非常简单，测试代码太复杂则本身就可能有bug。

###文档测试
***

Python的官方文档往往都会自带一些示例代码，这些代码和其他说明可以写在注视中，然后通过一些工具来自动生成文档。

这一节讲得主要就是，既然这些代码本身可以复制出来运行，那么可不可以**直接自动执行写在注视中的代码**呢？

答案是肯定的。 以上一节写的Dict为例子，只是这次把单元测试改为文档测试：

    # mydict2.py
    class Dict(dict):
        '''
        Simple dict but also support access as x.y style.

        >>> d1 = Dict()
        >>> d1['x'] = 100
        >>> d1.x
        100
        >>> d1.y = 200
        >>> d1['y']
        200
        >>> d2 = Dict(a=1, b=2, c='3')
        >>> d2.c
        '3'
        >>> d2['empty']
        Traceback (most recent call last):
            ...
        KeyError: 'empty'
        >>> d2.empty
        Traceback (most recent call last):
            ...
        AttributeError: 'Dict' object has no attribute 'empty'
        '''

        def __init__(self, **kw):
            super(Dict, self).__init__(**kw)

        def __getattr__(self, key):
            try:
                return self[key]
            except KeyError:
                raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

        def __setattr__(self, key, value):
            self[key] = value

    if __name__=='__main__':
        import doctest
        doctest.testmod()

可以看到中间由 `'''` 括起的是多行注释，中间包括文字描述和测试样例。 测试样例是按输入和输出轮番排列的，doctest模块也会严格按照给出的输入输出判断测试结果。

正常状况下执行该脚本是没有输出的，但如果模块有问题不能通过文档测试就会报错了，比方说注释掉 `__getattr__()` 方法后：

    $ python3 mydict2.py
    **********************************************************************
    File "/Users/michael/Github/learn-python3/samples/debug/mydict2.py", line 10, in __main__.Dict
    Failed example:
        d1.x
    Exception raised:
        Traceback (most recent call last):
          ...
        AttributeError: 'Dict' object has no attribute 'x'
    **********************************************************************
    File "/Users/michael/Github/learn-python3/samples/debug/mydict2.py", line 16, in __main__.Dict
    Failed example:
        d2.c
    Exception raised:
        Traceback (most recent call last):
          ...
        AttributeError: 'Dict' object has no attribute 'c'
    **********************************************************************
    1 items had failures:
      2 of   9 in __main__.Dict
    ***Test Failed*** 2 failures.

**Notice**：

1. 在IDLE中import模块时，doctest不会被执行，只有在命令行(也即测试环境下)直接作为脚本运行时，才会执行doctest。

2. 解读doctest的报错信息，可以看到它是把每对输入输出作为一个example，前面文档有9个输入输出，对应就有9个example。 然后会测试输入是否能得到和文档中同样的输出，否则failed example。

3. 输入输出是严格按照Python交互式命令行的，所以 **`>>>` 和代码句中间还有个空格，千万不能漏**！！

##IO编程
IO在计算机中指 Input/Output，涉及到数据交换的地方，如磁盘、网络等，就需要IO接口。

比方说上网，就需要通过网络IO获取页面，浏览器先把数据发送给服务器，这是Output，然后服务器发回页面，这是Input。 又比如磁盘读写，读就是Input，写就是Output。

Stream(流)是一个重要的概念，可以**把流想作一个水管，数据就是水管里的水，只能单向流动**。 要既能收数据又能发数据就要建立至少两根水管。

由于CPU和内存速度远远高于外设，所以IO编程中存在速度严重不匹配的问题。 这时分为同步IO和异步IO两种处理方式：

    举个麦当劳点餐的例子：
    你点餐后服务员说汉堡现做，要等5分钟，做完通知你。
    同步IO：
        你在收银台等了5分钟，拿到汉堡才去做别的事情，比如逛街。
    异步IO：
        你先去逛街，某个时刻服务员打电话来通知了再去拿汉堡。

显然异步IO编写的程序性能会远远高于同步UI，但模型也相对复杂。 服务员“通知”你的方式也各有不同，服务员直接跑来找到你，这是**回调模式**。 服务员用电话通知，你要一直检查手机，这是**轮询模式**。

**Notice**:

1. 操作IO的能力都是由操作系统提供的，操作系统不允许普通程序直接操作磁盘，所以实际上读写文件是向系统发出打开文件对象的请求。每一种编程语言都会把操作系统提供的低级C接口封装起来方便使用。

2. 本章IO编程都是同步IO模式，异步在以后涉及到服务器端程序开发再讨论。

###文件读写
***

####读文件

    try:
        f = open('/path/to/file', 'r')
        print(f.read())
    finally:
        if f:
            f.close()

一个完整的读文件操作如上，使用 `open()` 函数打开文件对象，传入文件名和标识符，标识符 `r` 表示读文件。  如果文件不存在 `open()` 函数就会抛出 `IOError` 错误。

得到文件对象后，可以使用 `read()` 方法**一次读取文件的全部内容到内存**，**返回的是一个str对象**。

最后一步是用 `close()` 方法关闭文件，**使用完毕后必须关闭，因为文件对象会占用操作系统资源**，同一时间能打开的文件数量是有限的。

因为 `open()` 函数出错后， `close()` 方法就不会被调用，为了保证正确关闭文件，就用 `try ... finally` 来实现。

但是每次都这样写太繁琐，Python又引入了 `with` 语句帮我们自动调用 `close()` 方法，这样我们就不用刻意调用了。

    with open('/path/to/file', 'r') as f:
        print(f.read())

特别地，因为 `read()` 方法会一次读取文件全部内容，如果文件太大就会爆掉内存。 保险起见有两种方式：

1. 使用 `read(size)` 方法每次读取size个字节的内容。

2. 使用 `readline()` 方法每次读取一行内容。

另外 `readline()` 还有个变形，使用 `readlines()` 方法可以**一次读取所有内容**并按行返回一个list对象。

    for line in f.readlines():
        print(line.strip()) # 把末尾的'\n'删掉

####file-like object

像 `open()` 函数返回的有 `read()` 方法的对象，在Python中统称为 `file-like Object`，除了文件以外，还可以是内存中的字节流，网络流，自定义流等等，不需要继承特定的类，有 `read()` 方法就可以了。

####二进制文件

前面**默认读取UTF-8编码的文本文件**，要读取二进制文件，如图片，视频，就要改一下标识符：

    >>> f = open('test.jpg', 'rb')
    >>> f.read()
    b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节

用 `rb` 模式打开文件即可。

####字符编码

要读取非UTF-8编码的文件，需要给 `open()` 函数传入 `encoding` 参数：

    >>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
    >>> f.read()
    '测试'

有时遇到编码不规范的文件，会抛出 `UnicodeDecodeError` 的错误，因为文件中夹杂了非法编码的字符。 这时需要给 `open()` 函数传入 `errors` 参数，表示如何处理编码错误，最简单就是直接忽略：

    >>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')

####写文件

写文件和读文件类似，只是打开文件传入标识符改为 `w` 或者 `wb`，并且换成用 `write()` 方法。

    >>> f = open('/Users/michael/test.txt', 'w')
    >>> f.write('Hello, world!')
    >>> f.close()

可以反复调用 `write()` 方法写文件，但操作系统往往不会立即将数据写入磁盘，而是放在内存中缓存，空闲时再写。 **调用 `close()` 方法时操作系统才会把没写入的数据全部写入磁盘**。

为了保险，同样可以使用 `with` 语句来实现写文件，并且不用调用 `close()` 方法：

    with open('/Users/michael/test.txt', 'w') as f:
        f.write('Hello, world!')

**Notice**：

1. 要写入特定编码文本文件，同样可以给 `open()` 函数传入 `encoding` 参数。

2. 使用 `with` 语句操作文件IO是个好习惯。

###StringIO和BytesIO
***
前面说道数据读写不仅仅是文件，还可以在内存中读写String和Bytes。

####StringIO

要把str写入StringIO，需要先创建一个StringIO，然后像写入文件一样写入：

    >>> from io import StringIO
    >>> f = StringIO()
    >>> f.write('hello')
    5
    >>> f.write(' ')
    1
    >>> f.write('world!')
    6
    >>> print(f.getvalue())
    hello world!

`getvalue()` 方法用于获得StringIO中的字符串。

读取StringIO的方法和读文件类似：

    >>> from io import StringIO
    >>> f = StringIO('Hello!\nHi!\nGoodbye!') #初始化StringIO
    >>> while True:
    ...     s = f.readline()
    ...     if s == '':
    ...         break
    ...     print(s.strip())
    ...
    Hello!
    Hi!
    Goodbye!

####BytesIO

StringIO只能操作str，要操作二进制数据就要用BytesIO读写bytes。创建和写入方法类似：

    >>> from io import BytesIO
    >>> f = BytesIO()
    >>> f.write('中文'.encode('utf-8')) #先编码得到UTF-8编码的bytes再写入
    6
    >>> print(f.getvalue())
    b'\xe4\xb8\xad\xe6\x96\x87'

读取：

    >>> from io import StringIO
    >>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
    >>> f.read()
    b'\xe4\xb8\xad\xe6\x96\x87'

###操作文件和目录
***

Python内置的os模块允许我们直接调用操作系统提供的接口函数以进行目录和文件操作。

####查看信息

    >>> import os
    >>> os.name
    'nt'

`os.environ` 可以查看所有环境变量，要获得某个环境变量可以用 `os.environ.get('key')`：

    >>> os.environ.get('JAVA_HOME')
    'C:\\Program Files\\Java\\jdk1.7.0_67'

`os.uname()` 可以查看用户信息，但Windows操作系统上不提供接口。

####创建/删除目录

操作文件和目录的函数方法os模块和`os.path`模块中。 其中`os.path.abspath()` 函数用来生成一条绝对路径：

    >>> os.path.abspath('.') #点符代表当前路径
    'F:\\Python35'
    >>> os.path.abspath('Tools\\demos') #相对路径
    'F:\\Python35\\Tools\\demos'

要在某个目录下创建一个新目录要先把新目录的绝对路径生成出来，可以**先获得当前路径，然后利用join函数合并生成**。 最后**用mkdir函数创建目录**。

    >>> curpath = os.path.abspath('.')
    >>> newpath = os.path.join(curpath, 'test_new')
    >>> newpath
    'F:\\Python35\\test_new'
    >>> os.mkdir(newpath)
    >>> os.rmdir(newpath)

合成路径时用join函数更优，因为**不同操作系统下路径分隔符不一样**，这样处理可以避免错误。

同理，拆分路径时也不应直接拆，而是通过split函数拆分，split函数会将路径拆分为路径和最后级别的目录/文件名两部分：

    >>> os.path.split('F:\\Python35\\Tools\\demos')
    ('F:\\Python35\\Tools', 'demos')
    >>> os.path.split('F:\\Python35\\hello.py')
    ('F:\\Python35', 'hello.py')
    >>> os.path.splitext('F:\\Python35\\hello.py')
    ('F:\\Python35\\hello', '.py')

利用splitext函数还可以方便地获取文件扩展名，如果是目录路径则扩展名为 `''`。

**Notice**：

合并和拆分函数针对的是字符串，所以路径不需要真实存在。

####复制文件

复制文件的函数在os模块中不存在，但 `shutil` 模块提供了 `copyfile()` 函数。

    >>> import shutil
    >>> shutil.copyfile('F:\\Python35\\hello.py','F:\\Python35\\Tools\\hello.py')
    'F:\\Python35\\Tools\\hello.py'

####过滤文件

下面举列出当前目录下所有目录和列出当前目录下所有 `.py` 脚本文件为例：

    >>> [x for x in os.listdir('.') if os.path.isdir(x)]
    ['DLLs', 'Doc', 'include', 'Lib', 'libs', 'Scripts', 'tcl', 'Tools', '__pycache__']
    >>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
    ['hello.py', 'mydict.py', 'mydict2.py', 'mydict_test.py', 'test.py']

其实就是用了列表生成式和os模块的函数，用 `os.path.isdir()` 函数判断是否目录； 用 `os.path.isfile()` 函数判断是否文件，然后再用 `os.path.splitext[]` 拆分路径，然后去tuple的第二个元素(下标1)就是扩展名了。

####作业

编写一个程序，能在当前目录以及当前目录的所有子目录下查找文件名包含指定字符串的文件，并打印出绝对路径。

    import os

    key = input('Please inter what you want to search:\n')

    for x in os.walk('.'):
        for y in x[2]:
            if key in os.path.split(y)[1]:
                print(os.path.join(os.path.abspath(x[0]), y))

os模块提供非常强大的walk函数可以用于生成制定路径下的目录树，返回的tuple有`dirpath`, `dirname`, `filename`三个元素。 这里用y提取出第三个元素也即文件名，然后判断一下我们输入的字符串是否在文件名中，是就把路径(第一个元素)和文件名拼接输出。

###序列化
***

**把变量从内存中变成可存储或传输的过程**称为序列化，在Python中叫做 pickling，在其他语言中也被称为 serialization，marshalling(信号编集)，flattening（压平）等等。

序列化后的内容可以写入磁盘或者通过网络传输到别的机器上。 反过来，**把变量内容从序列化的对象重新读到内存里**称为反序列化，也即 unpickling。

####PICKLE

Python提供 `pickle` 模块来实现序列化：

    >>> import pickle
    >>> d = dict(name='Bob', age=20, score=88)
    >>> pickle.dumps(d)
    b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'

`pickle.dumps()` 方法能把任意对象序列化，返回一个bytes对象。 这个bytes对象就可以写入文件。

此外 `pickle.dump()` 方法允许我们直接把一个对象序列化并写入文件：

    >>> f = open('dump.txt', 'wb')
    >>> pickle.dump(d, f)
    >>> f.close()

要把变量内容从磁盘读到内存时，可以先用bytes对象读出内容，再用 `pickle.loads()` 方法反序列化出对象。 也可以直接用 `pickle.load()` 方法反序列化出对象：

    >>> f = open('dump.txt', 'rb')
    >>> d = pickle.load(f)
    >>> f.close()
    >>> d
    {'age': 20, 'score': 88, 'name': 'Bob'}

**Notice**：

使用pickle的问题是，它只属于Python特有的序列化，只能用于Python，甚至不同版本Python也互补兼容，所以只用来保存一些不重要的数据。

####JSON

要在不同编程语言之间传递对象就必须把对象爱那个序列化成标准格式，如XML，更好的方法是**序列化为JSON**，因为**JSON表示出来就是一个字符串，可以被所有语言读取**，也可以方便地存储到磁盘或者通过网络传输，比XML更快，而且可以直接在WEB页面读取。

JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：

<table border="1px" cellspacing="2px" style="width:500px;margin:0 auto;text-align:center">
    <tr>
        <td># JSON类型 #</td>
        <td># Python类型 #</td>
    </tr>
    <tr>
        <td>{}</td>
        <td>dict</td>
    </tr>
    <tr>
        <td>[]</td>
        <td>list</td>
    </tr>
    <tr>
        <td>"string"</td>
        <td>str</td>
    </tr>
    <tr>
        <td>1234.56</td>
        <td>int或float</td>
    </tr>
    <tr>
        <td>true/false</td>
        <td>True/False</td>
    </tr>
    <tr>
        <td>null</td>
        <td>None</td>
    </tr>
</table>

Python提供了 `json` 模块用于Python对象和JSON格式的转换：

    >>> import json
    >>> d = dict(name='Bob', age=20, score=88)
    >>> json.dumps(d)
    '{"age": 20, "score": 88, "name": "Bob"}'

`dumps()` 方法返回一个str，内容就是标准的JSON。类似的，`dump()` 方法可以直接把JSON写入一个 `file-like Object`。

反序列化也类似，用 `loads()` 或者对应的 `load()` 方法，前者把JSON的字符串反序列化，后者从 `file-like Object` 中读取字符串并反序列化：

    >>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
    >>> json.loads(json_str)
    {'age': 20, 'score': 88, 'name': 'Bob'}

由于JSON标准规定**JSON编码是UTF-8**，所以我们总是能正确地在Python的str与JSON的字符串之间转换。

####JSON进阶

除了上面表格包含的类型，有时我们会自定义类型，比方说定义一个Student类，如果还是直接用 `dumps()` 方法进行序列化就会抛出 `TypeError` 的错误了。

    import json

    class Student(object):
        def __init__(self, name, age, score):
            self.name = name
            self.age = age
            self.score = score

    s = Student('Bob', 20, 88)

其实 `dumps()` 方法除了必须的obj参数，还有一大堆[可选参数](https://docs.python.org/3/library/json.html#json.dumps)可以用来定制JSON序列化。

其中，可选参数 `default` 就是**把任意一个对象变成一个可序列为JSON的对象**，我们只需要为Student专门写一个**转换函数**，再把函数传进去即可：

    def student2dict(std):
        return {
            'name': std.name,
            'age': std.age,
            'score': std.score
        }

    >>> print(json.dumps(s, default=student2dict))
    {"age": 20, "name": "Bob", "score": 88}

但是下次我们定义了另外一个类就又要再写一个转换函数了，这样很麻烦，其实可以直接用类对象的 `__dict__` 属性，除了定义了 `__slots__` 的类之外，每个类的实例都有 `__dict__` 属性，这是一个用来存储实例变量的dict。

因此我们只要写为 `print(json.dumps(s, default=lambda obj: obj.__dict__))` 就可以了。

但反过来就必须定义转换函数了，思路是用属性值生成一个新的实例然后返回：

    def dict2student(d):
        return Student(d['name'], d['age'], d['score'])

    >>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
    >>> print(json.loads(json_str, object_hook=dict2student))
    <__main__.Student object at 0x10cd3c190>

##进程和线程

对操作系统来说，一个任务就是一个**进程(Process)**，比如打开一个word就是启动一个word进程；一个进程要做的事可能不止一样，比如word既要进行打字，又要做拼写检查等等，这些子任务就成为**线程(Thread)**。线程是最小的执行单元。

前面写的Python程序都是执行单任务的进程，即只有一个线程，同时执行多个任务有3种解决方案：

1. 多进程模式：启动多个进程，每个进程只有一个线程，但多个进程可以同时执行多任务。

2. 多线程模式：启动一个进程，在进程内启动多个线程，多个线程可以执行多任务。

3. 多进程+多线程模式：启动多个进程，每个进程内再启动多个线程，最为复杂。

如何调度进程和线程，完全**由操作系统决定**，程序自己不能决定什么时候执行，执行多长时间。 多进程和多线程的程序涉及到**同步数据共享**的问题，编写起来更复杂。

###多进程
***

Unix/Linux操作系统提供 `fork()` 系统调用，这个函数调用非常特殊，**调用一次，返回两次**。 操作系统会自动把当前进程(称为*父进程*)复制一份(称为*子进程*)，然后**分别在父进程和子进程内返回**。

子进程永远返回0，父进程返回子进程ID。 因为一个父进程可以fork出很多子进程，所以要记下每个子进程的ID，而子进程只需要调用 `getppid()` 就可以拿到父进程的ID。

Python的 `os` 模块封装着常见的系统调用，其中就包括 `fork`，所以可以在Python中很轻松地创建子进程：

    import os

    print('Process (%s) start...' % os.getpid())
    # Only works on Unix/Linux/Mac:
    pid = os.fork()
    if pid == 0:
        print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
    else:
        print('I (%s) just created a child process (%s).' % (os.getpid(), pid))

运行结果：

    Process (876) start...
    I (876) just created a child process (877).
    I am child process (877) and my parent is 876.

可以看到首先在父进程中返回，返回的pid是子进程的，不为0，所以执行else代码块。 然后到子进程中返回，子进程返回0，执行if代码块，此时 `getpid` 得到子进程自己的pid， `getppid` 得到父进程的pid。

**Notice**：

1. Windows没有提供 `fork` 调用，上面这段代码无法执行。
2. 有了 `fork` 调用，进程接到新任务时就可以赋值出一个子进程来处理新任务，常见的Apache服务器就是由父进程监听端口，每当有新的http请求时，就fork出子进程来处理新的http请求。

####multiprocessing

Python是跨平台的，自然也要提供一个跨平台的多进程支持。`multiprocessing` 模块就是跨平台版本的多进程模块。

`multiprocessing` 模块提供一个 `Process` 类表示进程对象：

    from multiprocessing import Process
    import os

    # 子进程要执行的代码
    def run_proc(name):
        print('Run child process %s (%s)...' % (name, os.getpid()))

    if __name__=='__main__':
        print('Parent process %s.' % os.getpid())
        p = Process(target=run_proc, args=('test',))
        print('Child process will start.')
        p.start()
        p.join()
        print('Child process end.')

上面的例子展示的是启动一个子进程，并等待其结束。 **创建子进程实质是创建一个 `Process` 类的实例**，传入参数包括一个子进程要执行的函数以及该函数的参数，注意单个元素tuple的写法。

`start()` 方法用于启动进程，`join()` 方法用于进程间同步，**可以等待子进程结束再往下运行**。 如果不调用 `join()`，父进程和子进程就同时运行。

这个例子中就是创建了一个子进程p执行 `run_proc` 函数，父进程等到p结束了才往下执行，打印 `'Child process end.'` 的信息。

执行结果：

    F:\Python35>python multiprocessing_test1.py
    Parent process 17492.
    Child process will start.
    Run child process test (17516)...
    Child process end.

####Pool

要启动大量子进程时可以用进程池的方式批量创建子进程：

    from multiprocessing import Pool
    import os, time, random

    def long_time_task(name):
        print('Run task %s (%s)...' % (name, os.getpid()))
        start = time.time()
        time.sleep(random.random() * 3)
        end = time.time()
        print('Task %s runs %0.2f seconds.' % (name, (end - start)))

    if __name__=='__main__':
        print('Parent process %s.' % os.getpid())
        p = Pool(4)
        for i in range(5):
            p.apply_async(long_time_task, args=(i,))
        print('Waiting for all subprocesses done...')
        p.close()
        p.join()
        print('All subprocesses done.')

执行结果：

    F:\Python35>python multiprocessing_test2.py
    Parent process 7992.
    Waiting for all subprocesses done...
    Run task 0 (5164)...
    Run task 1 (13200)...
    Run task 2 (8884)...
    Run task 3 (1580)...
    Task 2 runs 0.20 seconds.
    Run task 4 (8884)...
    Task 3 runs 1.56 seconds.
    Task 4 runs 2.08 seconds.
    Task 1 runs 2.58 seconds.
    Task 0 runs 2.86 seconds.
    All subprocesses done.

这个例子首先是创建一个 `Pool` 类的实例，如果不传入参数，**默认Pool的大小就是系统的CPU核数**(比如我的电脑CPU是4核的，Pool大小就是4，最多同时执行4个进程)，这里传入一个4，那么进程池大小就是4。 **即使只有4核，传入大于4的数也可以**，操作系统会自己协调好如何通过执行多个进程。这里可以看到进程4(第5个进程)要等一个进程结束后才能开始。

然后在循环中再调用 `apply_async` 方法，每次循环调用一次就启动一个子进程执行参数里的方法，参数和前面实例Process时类似。

不同的是，对进程池对象调用 `join()` 方法是**等待所有子进程执行完毕再往下执行**。 并且，调用 `join()` 方法之前要先调用 `close()` 方法，**close之后就不能再往进程池加入新的进程了**。

####子进程

上面例子中，子进程都是自身的copy。 但很多时候子进程是一个外部进程，`subprocess` 模块可以让我们方便地启动一个子进程并控制起输入和输出。

    import subprocess

    r = subprocess.call(['java', '-version'])
    print('Exit code:', r)

这段代码的运行效果就像直接在命令行执行 `java -version` 一样。 事实上多个参数也可以的，存储在list里面，用逗号分割。 默认返回码为0即运行通过，为1则出错。

如果要控制子进程的输入，可以通过 `communicate()` 方法，注意输出的处理方式，要进程解码：

    import subprocess

    print('$ nslookup')
    p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    output, err = p.communicate(b'set q=mx\npython.org\nexit\n')
    print(output.decode('gbk')) #这里原文是utf-8，但windows下要用gbk
    print('Exit code:', p.returncode)

这段代码相当于在命令行执行nslookup之后再手动输入：

    set q=mx
    python.org
    exit

注意回车也要表现出来，并且输入要转换成bytes才能传到子进程。

这里无法用前面 `java -version` 的例子来做，因为不是一种类型，nslookup执行后会转换成一种等待用户输入的格式，而java执行后是直接输出帮助信息然后结束的。

####进程间通信

操作系统提供很多机制实现进程间通信，Python的multiprocessing模块包装了底层的机制，提供了 `Queue`，`Pipes` 等方式，虾米那以 `Queue` 为例，在父进程中创建两个子进程，一个往 `Queue` 里面写数据， 一个从 `Queue` 中读数据：

    from multiprocessing import Process, Queue
    import os, time, random

    # 写数据进程执行的代码:
    def write(q):
        print('Process to write: %s' % os.getpid())
        for value in ['A', 'B', 'C']:
            print('Put %s to queue...' % value)
            q.put(value)
            time.sleep(random.random())

    # 读数据进程执行的代码:
    def read(q):
        print('Process to read: %s' % os.getpid())
        while True:
            value = q.get(True)
            print('Get %s from queue.' % value)

    if __name__=='__main__':
        # 父进程创建Queue，并传给各个子进程：
        q = Queue()
        pw = Process(target=write, args=(q,))
        pr = Process(target=read, args=(q,))
        # 启动子进程pw，写入:
        pw.start()
        # 启动子进程pr，读取:
        pr.start()
        # 等待pw结束:
        pw.join()
        # pr进程里是死循环，无法等待其结束，只能强行终止:
        pr.terminate()

####小结

在 `Unix/Linux` 下，`multiprocessing` 模块封装了 `fork()` 调用，使我们不需要关注 `fork()` 的细节。由于 `Windows` 没有 `fork` 调用，因此，`multiprocessing` 需要“模拟”出 `fork` 的效果，**父进程所有Python对象都必须通过 `pickle` 序列化再传到子进程去**，所有，如果 `multiprocessing` 在 `Windows` 下调用失败了，要先考虑是不是 `pickle` 失败了。

###多线程
***

前面提到多任务可以由多进程完成，也可以由一个进程内的多线程完成。 Python的标准库提供了两个模块：`_thread` 和 `threading`，`_thread` 是低级模块，`threading` 是对 `_thread` 进行了封装的高级模块。 绝大多数情况下，我们只需要使用threading这个高级模块。

启动一个线程就是创建一个 `Thread` 实例并传入一个用于执行的函数，然后调用 `start()` 开始执行：

    import time, threading

    # 新线程执行的代码:
    def loop():
        print('thread %s is running...' % threading.current_thread().name)
        n = 0
        while n < 5:
            n = n + 1
            print('thread %s >>> %s' % (threading.current_thread().name, n))
            time.sleep(1)
        print('thread %s ended.' % threading.current_thread().name)

    print('thread %s is running...' % threading.current_thread().name)
    t = threading.Thread(target=loop, name='LoopThread')
    t.start()
    t.join()
    print('thread %s ended.' % threading.current_thread().name)

执行结果：

    thread MainThread is running...
    thread LoopThread is running...
    thread LoopThread >>> 1
    thread LoopThread >>> 2
    thread LoopThread >>> 3
    thread LoopThread >>> 4
    thread LoopThread >>> 5
    thread LoopThread ended.
    thread MainThread ended.

由于任何进程默认就会启动一个线程，我们把该线程称为**主线程，主线程又可以启动新的线程**，Python的threading模块有个 `current_thread()` 函数，它永远返回当前线程的实例。 主线程实例的名字叫 `MainThread`，子线程的名字在创建时指定，我们用 `LoopThread` 命名子线程。**名字仅仅在打印时用来显示，完全没有其他意义**，如果不起名字Python就自动给线程命名为Thread-1，Thread-2……

####Lock

多线程和多进程最大的不同在于，多进程中，同一个变量，**各自有一份拷贝存在于每个进程中**，互不影响，而多线程中，所有变量都由所有线程共享，所以，**任何一个变量都可以被任何一个线程修改**。

举个例子：

    a=a+5
    a=a-5

当多个线程同执行上面两个语句时，理论上a的值是不应该有变化的，但实际不是，`a+5` 在CPU中执行实际是分两步的：

    temp=a+5
    a=temp

当多个线程同时操作a并处于不同步骤时，比如线程1执行 `temp=a+5` 后，执行 `a=temp` 前，该时刻线程2也恰恰执行了  `temp=a+5`，这样a的值就会产生错误。

要避免这一问题，本质就是要**确保同一时刻只有一个线程在修改变量的值**。 因此线程要**持有锁**，然后才能操作。 创建一个锁就是通过 `threading.Lock()` 来实现：

    import time, threading

    # 假定这是你的银行存款:
    balance = 0
    lock = threading.Lock()

    def change_it(n):
        # 先存款后取款，结果应该为0:
        global balance
        balance = balance + n
        balance = balance - n

    def run_thread(n):
        for i in range(100000):
            # 先要获取锁:
            lock.acquire()
            try:
                # 放心地改吧:
                change_it(n)
            finally:
                # 改完了一定要释放锁:
                lock.release()

    t1 = threading.Thread(target=run_thread, args=(5,))
    t2 = threading.Thread(target=run_thread, args=(8,))
    t1.start()
    t2.start()
    t1.join()
    t2.join()
    print(balance)

**获得锁的线程用完后一定要释放锁**，否则那些苦苦等待锁的线程将永远等待下去，成为死线程。所以我们用try...finally来确保锁一定会被释放。

**Notice**：

1. 锁确保了某段关键代码只能由一个线程从头到尾完整地执行。 但也阻止了多线程并发执行，包含锁的某段代码实际上只能以单线程模式执行，效率大大下降。
2. 由于可以存在多个锁，不同的线程持有不同的锁，并试图获取对方持有的锁时，可能会造成**死锁**，导致多个线程全部挂起，既不能执行，也无法结束，只能靠操作系统强制终止。

####多核CPU

拥有多核CPU时可以同时执行多个线程，这个同时指的是完全同一个时刻，而不是操作系统在背后来回切换而近似的同时。

如果有一个线程死循环，则会100%占用CPU，两个线程死循环就会200%占用CPU，也即占用两个CPU，要令N核CPU死掉就要启动N个死循环线程。

用 `multiprocessing.cpu_count()` 可以知道自己电脑CPU是几核的。

写段死循环代码：

    import threading, multiprocessing

    def loop():
        x = 0
        while True:
            x = x ^ 1

    for i in range(multiprocessing.cpu_count()):
        t = threading.Thread(target=loop)
        t.start()

廖神的教程是飙到102%，自己电脑实测是从13%左右飙到50多%，也即**只用了一核**。

事实上，用C、C++、Java做同样的事会把核心全部跑满，4核就跑到400%。 但Python不会，尽管**Python的线程是真正的线程而不是模拟出来的**，但是Python解释器执行代码时会有一个**GIL(Global Interpreter Lock)**锁。

每个Python线程执行前必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个**GIL全局锁把所有线程的执行代码都给上了锁**，所以，**多线程在Python中只能交替执行**，即使100个线程跑在100核CPU上，也只能用到1个核，不能有效利用多核，除非自己重写一个不带GIL的解释器。

如意如果真的要用Python写多核任务就要用前面的多进程的模式，多个Python进程有各自独立的GIL锁，互不影响。

###ThreadLocal

在多线程环境下，每个线程都有自己的数据。 线程使用自己的**局部变量**比使用全局变量好，因为局部变量只有线程自己能看见，不会影响其他线程，而全局变量的修改必须加锁。

但局部变量在函数调用时必须进行传递，多层调用时就会非常麻烦，比如：

    def process_student(name):
        std = Student(name)
        # std是局部变量，但是每个函数都要用它，因此必须传进去：
        do_task_1(std)
        do_task_2(std)

    def do_task_1(std):
        do_subtask_1(std)
        do_subtask_2(std)

    def do_task_2(std):
        do_subtask_2(std)
        do_subtask_2(std)

这个例子中std是根据传入子线程的参数name实例化的Student类对象，因为每个子线程得到的name可能不同，所以每个实例都应是不同的，这里std不能是全局变量。

但是不是全局变量的话，别的函数要使用std就必须调用者传入std作为参数才可以，当调用的函数变更多层数也变更多，这样传递就显得非常繁琐了。

有一个思路就是写一个global的dict，然后子线程本身作key，每个子线程实例出的std作对应的value。这样可以解决问题，但代码太丑~~

于是我们有了 `ThreadLocal`：

    import threading

    # 创建全局ThreadLocal对象:
    local_school = threading.local()

    def process_student():
        # 获取当前线程关联的student:
        std = local_school.student
        print('Hello, %s (in %s)' % (std, threading.current_thread().name))

    def process_thread(stuName):
        # 绑定ThreadLocal的student:
        local_school.student = stuName
        process_student()

    t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
    t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
    t1.start()
    t2.start()
    t1.join()
    t2.join()

运行得到：

    Hello, Alice (in Thread-A)
    Hello, Bob (in Thread-B)

注意 `process_thread(stuName)` 中 `stuName` 实在创建线程实例时放入args这个参数tuple中的，同样注意单元素tuple的写法，name参数则是用于给线程起名字的。

全局变量 `local_school` 就是一个 `ThreadLocal` 对象，每个Thread对它都可以读写student属性，但互不影响。你可以把 `local_school` 看成全局变量，但每个属性如 `local_school.student` 都是线程的局部变量，可以任意读写而互不干扰，也不用管理锁的问题，`ThreadLocal`内部会处理。

`ThreadLocal` 最常用的地方就是为每个线程绑定一个**数据库连接，HTTP请求，用户身份信息等**，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。

###进程 VS 线程

这一节主要是比较一下多进程和多线程这两种实现多任务的方式有什么优缺点。

要实现多任务，通常会采用 Master-Worker 模式，Master负责分配任务，Worker负责执行任务。 通常是一个Master，多个Worker。用多进程实现就主进程Master，其他进程Worker；用多线程实现就主线程Master，其他线程Worker。

多进程最大优点是**稳定性高**，一个子进程崩溃不会影响主进程和其他子进程。(当然主进程挂了就会全挂，但主进程只负责分配任务，挂掉概率很低)。Apache最早就是用多进程的。

多进程缺点是**创建进程代价大**，Unix/Linux系统用fork调用还行，Windows下创建进程开销巨大。 另外一个是，**操作系统同时运行的进程数有限**。

多线程优点是**比多进程快一点**，但也只是一点。 缺点是**任何一个线程挂掉都可能造成整个进程崩溃**。 因为所有线程共享进程的内存，一个线程出了问题，操作系统会强制结束整个进程。

Windows下多线程效率比多进程搞。所以IIS默认用多线程，但由于稳定性问题，后来IIS和Apache都妥协，该用多进程+多线程混合模式，但这样就把问题弄得很复杂。

####线程切换

无论是用多进程还是多线程实现多任务，数量一多，效率都上不去。因为系统忙着切换任务，根本没时间执行任务。

看起来多任务同时执行其实是因为切换速度足够快。 但每次切换都要先**保存现场**（CPU寄存器状态、内存页等），然后切换到新任务时要**准备新环境**（恢复该任务上次的寄存器状态，切换内存页等）。

所以，多任务一旦多到一个限度，就会消耗掉系统所有的资源，结果效率急剧下降，所有任务都做不好。

####计算密集型 VS IO密集型

是否采用多任务还要考虑任务的类型。

计算密集型任务要进行大量的计算，**消耗CPU资源**，比如算圆周率、解码等。 这类任务代码运行效率至关重要，像Python这样的脚本语言运行效率很低，应当用C语言编写。

IO密集型任务达赖那个涉及到网络、磁盘IO，**CPU消耗很少**，大部分事件都在等待IO（IO速度远低于CPU和内存速度），这类任务用脚本语言最好，因为开发效率高，代码量少。

####异步IO

因为IO速度远低于CPU和内存速度，所以用单进程或单线程模型会导致别的任务无法并行执行，因此才需要多进程和多线程模型。

现代操作系统的一大改进就是允许异步IO，即允许进程/线程先做别的任务，IO好了再进行等待IO的任务。

有了异步IO的支持，单进程单线程模型就可以执行多任务，这种全新的模型称为**事件驱动模型**。 Nginx就是支持异步IO的Web服务器，它在单核CPU上采用单进程模型就可以高效地支持多任务。在多核CPU上，可以运行多个进程（数量与CPU核心数相同），充分利用多核CPU。**由于系统总的进程数量十分有限，因此操作系统调度非常高效。用异步IO编程模型来实现多任务是一个主要的趋势。**

对应到Python，单进程的异步编程模型称为**协程*，后面会进行讨论。

###分布式进程

在线程和进程中，应当**优选进程**。因为进程更稳定且可分布到多台机器上，线程则只能分布到同一台机器的多个CPU上。

在Python中可以利用 `multiprocessing` 模块的子模块 `managers` 来实现分布式进程。 一个服务进程（主进程）作为调度者，依靠网络通信把任务分布到其他多个进程中。

前面提到多个进程间可以用 `Queue` 进行通信，分布在不同机器上的进程通信同样可以利用 `Queue`，但是要通过 `managers` 模块把它暴露出去。

先看服务进程(主进程)，它负责启动 `Queue`，并且把 `Queue` 注册到网络上，然后往里面写任务：

    # task_master.py

    import random, time, queue
    from multiprocessing import freeze_support
    from multiprocessing.managers import BaseManager

    # 发送任务的队列:
    task_queue = queue.Queue()
    # 接收结果的队列:
    result_queue = queue.Queue()

    # 从BaseManager继承的QueueManager:
    class QueueManager(BaseManager):
        pass

    def return_task_queue():
        global task_queue
        return task_queue

    def return_result_queue():
        global result_queue
        return result_queue

    def test():
        # 把两个Queue都注册到网络上, callable参数关联了Queue对象:
        # QueueManager.register('get_task_queue', callable=lambda: task_queue)
        # QueueManager.register('get_result_queue', callable=lambda: result_queue)
        QueueManager.register('get_task_queue', callable=return_task_queue)
        QueueManager.register('get_result_queue', callable=return_result_queue)

        # 绑定端口5000, 设置验证码'abc':
        manager = QueueManager(address=('127.0.0.1', 5000), authkey=b'abc')
        # 启动Queue:
        manager.start()
        # 获得通过网络访问的Queue对象:
        task = manager.get_task_queue()
        result = manager.get_result_queue()
        # 放几个任务进去:
        for i in range(10):
            n = random.randint(0, 10000)
            print('Put task %d...' % n)
            task.put(n)
        # 从result队列读取结果:
        print('Try get results...')
        for i in range(10):
            r = result.get(timeout=10)
            print('Result: %s' % r)
        # 关闭:
        manager.shutdown()
        print('master exit.')

    if __name__ == '__main__':
        #freeze_support()
        test()

如果只是在一台机器写多进程，创建的Queue可以直接用，但分布式多进程中，主进程往Queue中添加任务不可以直接操作，否则就绕过了manager的封装，**必须通过 `manager.get_task_queue()` 取得Queue接口然后再添加任务**。

**Notice**：

原教程中代码可以在Linux/Unix下跑，但Windows下要改几个地方，上面的是改好的。

1. register中callable不要用lambda，直接改成定义函数，然后传入函数名。

2. 创建manager时指定IP地址，而不是直接用`''`。

3. 测试代码用 `if __name__ == '__main__':` 括起来，如果有崩溃就先执行 `multiprocessing.freeze_support()` 再test。（我没崩所以没有用~）

然后可以在另一台机器启动任务进程，当然本机上启动也行，不过就不是分布式了~

    # task_worker.py

    import time, sys, queue
    from multiprocessing.managers import BaseManager

    # 创建类似的QueueManager:
    class QueueManager(BaseManager):
        pass

    # 由于这个QueueManager只从网络上获取Queue，所以注册时只提供名字:
    QueueManager.register('get_task_queue')
    QueueManager.register('get_result_queue')

    # 连接到服务器，也就是运行task_master.py的机器:
    server_addr = '127.0.0.1'
    print('Connect to server %s...' % server_addr)
    # 端口和验证码注意保持与task_master.py设置的完全一致:
    m = QueueManager(address=(server_addr, 5000), authkey=b'abc')
    # 从网络连接:
    m.connect()
    # 获取Queue的对象:
    task = m.get_task_queue()
    result = m.get_result_queue()
    # 从task队列取任务,并把结果写入result队列:
    for i in range(10):
        try:
            n = task.get(timeout=1)
            print('run task %d * %d...' % (n, n))
            r = '%d * %d = %d' % (n, n, n*n)
            time.sleep(1)
            result.put(r)
        except Queue.Empty:
            print('task queue is empty.')
    # 处理结束:
    print('worker exit.')

任务进程是通过网络连接到服务进程的，所以**要指定跑服务进程那台机器的IP**。

####小结

结果这里不贴了，可以看到这一节的例子实现了一个简单的 Master/Worker 模型，只要稍加改造，甚至可以把任务分布到几十台机器上，Python的封装很好，实现非常方便。

可以看到任务进程没有创建Queue，所以实际上两个Queue都是存储在服务进程里的，其他机器是通过网络来访问Queue的。

authkey可以用来保证机器正常通信，不受恶意干扰，如果authkey不一致肯定连不上。

**Notice**：

1. Queue的作用是用来传递任务和接收结果，**每个任务的描述数据量要尽量小**。

2. 凡是通过网络传输的对象必须**支持序列化**，建议统一用JSON序列化成str。

3. 这一节其实我有些疑问，用本机IP（127.0.0.1）没问题，但换成IPv4地址就连不上，这一点我希望之后学到TCP/UDP章节时能解答。

##正则表达式

正则表达式是一种用来匹配字符串的方法，用一种描述性语言来给字符串定义一个规则，然后判断字符串是否合法，即是否符合规则。

最基本的有：

- 直接给出字符时精确匹配
- 用 `\d` 可以匹配一个数字，`\w` 可以匹配一个字母或数字。
- 用 `*` 表示任意个字符(包括0个)，`+` 表示至少一个字符，`?` 表示0个或1个字符，`{n}` 表示n个字符，`{n,m}` 表示n~m个字符。

举个例子：`\d{3}\s+\d{3,8}`

- `\d{3}` 表示匹配3个数字，例如'010'；

- `\s` 可以匹配一个空格（也包括Tab等空白符），所以 `\s+` 表示至少有一个空格，例如匹配' '，' '等；

- `\d{3,8}` 表示3-8个数字，例如'1234567'。

整个合起来可以用来匹配带3位区号，以空格隔开的电话号码。

###进阶

进行更精确地匹配，可以用 `[]` 表示范围，如：

- `[0-9a-zA-Z\_]` 可以匹配一个数字、字母或者下划线；

- `[0-9a-zA-Z\_]+` 可以匹配至少由一个数字、字母或者下划线组成的字符串，比如'a100'，'0_Z'，'Py3000'等等；

- `[a-zA-Z\_][0-9a-zA-Z\_]*` 可以匹配由字母或下划线开头，后接任意个数字、字母或者下划线组成的字符串，也就是Python合法的变量；

- `[a-zA-Z\_][0-9a-zA-Z\_]{0, 19}` 更精确地限制了变量的长度是1-20个字符（前面1个字符+后面最多19个字符）。

特别地，有几个值得注意的特殊字符：

- `A|B` 可以匹配A或B，所以 `[P|p]ython` 可以匹配'Python'或者'python'。

- `^` 表示行的开头，`^\d` 表示必须以数字开头。

- `$` 表示行的结束，`\d$` 表示必须以数字结束。

有趣的是，py也可以用来匹配'python'，但是用 `^py$`就变成了整行匹配，就只能匹配'py'了。

###re模块

Python提供 `re` 模块来包含所有正则表达式的功能。 但是，因为Python字符串本身也是用 `\` 左斜杠做转义符，所以使用正则匹配就会有些问题：

    s = 'ABC\\-001' # Python的字符串
    # 对应的正则表达式字符串变成：
    # 'ABC\-001'

建议最好使用 `r` 前缀表示正则表达式本身是不转义的Python字符串：

    s = r'ABC\-001' # Python的字符串
    # 对应的正则表达式字符串不变：
    # 'ABC\-001'

要判断匹配，可以使用 `match` 函数，匹配成功就返回一个 `Match` 对象，否则返回 `None`。

    >>> import re
    >>> re.match(r'^\d{3}\-\d{3,8}$', '010-12345')
    <_sre.SRE_Match object; span=(0, 9), match='010-12345'>
    >>> re.match(r'^\d{3}\-\d{3,8}$', '010 12345')
    >>>

常见的写法是：

    test = '用户输入的字符串'
    if re.match(r'正则表达式', test):
        print('ok')
    else:
        print('failed')

###切分字符串

用正则表达式切分字符串比用固定字符更灵活，比方说：

    >>> 'a b   c'.split(' ')
    ['a', 'b', '', '', 'c']

因为b和c之间有多个空格，这样切分就不正确了，但是用正则的话：

    >>> re.split(r'\s+', 'a b   c')
    ['a', 'b', 'c']

还可以允许有多种划分符：

    >>> re.split(r'[\s\,\;]+', 'a,b;; c  d')
    ['a', 'b', 'c', 'd']

###分组

使用正则还可以很方便地提取子串，在正则表达式中 `()` 表示的就是分组：

    >>> m = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
    >>> m
    <_sre.SRE_Match object; span=(0, 9), match='010-12345'>
    >>> m.group(0)
    '010-12345'
    >>> m.group(1)
    '010'
    >>> m.group(2)
    '12345'

同样使用 `match` 方法来匹配，如过匹配上，我们就可以根据正则表达式中的分组用 `group` 来提取子串了。 `^(\d{3})-(\d{3,8})$` 定义了两个分组，3位数字开头，中间一个横杆，连接3~8位数字结尾。

再比如提取时分秒：

    >>> t = '19:05:30'
    >>> m = re.match(r'^(0[0-9]|1[0-9]|2[0-3]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])$', t)
    >>> m.groups()
    ('19', '05', '30')

可以在正则中规定合法的时间，这样如果匹配不上就是不合法的时间了。

###贪婪匹配

正则表达式默认是贪婪匹配，即没有规定具体匹配多少个字符时会匹配尽可能多的字符，如：

    >>> re.match(r'^(\d+)(0*)$', '102300').groups()
    ('102300', '')

可以看到第一个分组直接匹配了整个字符串，第二个分组就匹配不到了。 可以通过加入 `?` 符表示采用非贪婪匹配(尽可能少匹配)，注意尽可能少的前提是**保证字符串能匹配完整**。 如果第一个分组没有匹配到字符3，那第二个分组就匹配不上后面字符了，所以这里的最少不是说就匹配一个。

    >>> re.match(r'^(\d+?)(0*)$', '102300').groups()
    ('1023', '00')
    >>> re.match(r'^(\d+?)(0*)$', '10000000').groups()
    ('1', '0000000')
    >>> re.match(r'^(\d+?)(0*)$', '100000001').groups()
    ('100000001', '')

###编译

在Python中使用正则表示式时，re模块会**先编译正则表达式**，如果表达式不合法，会报错。 编译通过了就会**用编译过后的正则表达式去匹配字符串**。

因此，如果我们要重复多次使用正则表达式匹配时，出于效率的考虑，应该**先预编译正则表达式**，然后**直接用得到编译好的正则表达式进行匹配**。

    >>> import re
    # 编译:
    >>> re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
    # 使用：
    >>> re_telephone.match('010-12345').groups()
    ('010', '12345')
    >>> re_telephone.match('010-8086').groups()
    ('010', '8086')

###练习

写一个可以匹配Email地址的正则表达式，并且能提取出名字：

    import re
    re_email = re.compile(r'([A-Za-z\.\_]+)@([A-Za-z0-9]+?)\.com')
    while True:
        email = input('plaues enter email address:').strip()
        res = re_email.match(email)
        if res:
            print('Hello, %s' % res.group(1))
        else:
            print('Invalid email address.')

思路很简单，邮箱的格式是 `用户名@邮箱名.com`，注意分组的设置，然后后半部分要用非贪婪匹配。

运行时：

    F:\Python35>python checkEmail.py
    plaues enter email address:family_ld@163.com
    Hello, family_ld
    plaues enter email address:baidu.com
    Invalid email address.
    plaues enter email address:bill.gates@microsoft.com
    Hello, bill.gates

##常用内建模块

###datatime

datatime库用于处理日期和时间，获取当前日期和时间如下：

    >>> from datetime import datetime
    >>> now = datetime.now() # 获取当前datetime
    >>> print(now)
    2015-05-18 16:28:07.198690
    >>> print(type(now))
    <class 'datetime.datetime'>

注意import的方式，在datetime模块下有一个datetime类，我们要用到的是这个类的类方法，所以不能只 `import datetime`，否则就要用 `datetime.datetime.类方法名` 的方式来使用。

生产一个指定的日期和时间可以通过类的构造函数完成：

    >>> from datetime import datetime
    >>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
    >>> print(dt)
    2015-04-19 12:20:00

####datetime转换为timestamp

因为计算机中timestamp实际是用数字表示的，当前时间实质是从1970年1月1日 00:00:00 UTC+00:00时区的时刻(称为：**epoch time**，即新纪元时间，记为数字0)到当前时间所过的**秒数**，这个差我们称为timestamp。

注意UTC后面的时区，比方说我们生活在东八区，那么epoch time就应该是 `1970-1-1 08:00:00 UTC+8:00`，这和 `1970-1-1 00:00:00 UTC+0:00` 是一样的，计算机存的都是0。

要把datetime类型转换为timestamp只需要简单调用 `timestamp()` 方法：

    >>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
    >>> dt.timestamp() # 把timestamp转换为datetime
    1429417200.0

注意timestamp是个浮点数，小数点表示毫秒数。

####timestamp转换为datetime

反过来转换时调用 `fromtimestamp()` 方法：

    >>> t = 1429417200.0
    >>> print(datetime.fromtimestamp(t))
    2015-04-19 12:20:00

注意到timestamp是一个浮点数，它没有时区的概念，而**datetime是有时区的**。上述转换**是在timestamp和本地时间做转换**。所以我们如果操作系统设定的是东八区，那所得的时间实际上就是 `2015-04-19 12:20:00 UTC+8:00`。

如果要转换为标准的格林威治时间，则使用 `utcfromtimestamp()` 函数：

    >>> print(datetime.utcfromtimestamp(t)) # UTC时间
    2015-04-19 04:20:00

####str转换为datetime

有时用户输入的日期时间是字符串，要进行处理就必须转换为datetime，这是通过 `datetime
.strptime()` 函数实现的，如何进行格式化可以参考[Python文档](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior)。

    >>> cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
    >>> print(cday)
    2015-06-01 18:19:59

####datetime转换为str

类似地可以反过来通过 `datetime.strftime()` 函数进行转换。

    >>> now = datetime.now()
    >>> print(now.strftime('%a, %b %d %H:%M'))
    Mon, May 05 16:28

####datetime加减

datetime的加减要导入 `timedelta` 这个类：

    >>> from datetime import datetime, timedelta
    >>> now = datetime.now()
    >>> now
    datetime.datetime(2015, 5, 18, 16, 57, 3, 540997)
    >>> now + timedelta(hours=10)
    datetime.datetime(2015, 5, 19, 2, 57, 3, 540997)
    >>> now - timedelta(days=1)
    datetime.datetime(2015, 5, 17, 16, 57, 3, 540997)
    >>> now + timedelta(days=2, hours=12)
    datetime.datetime(2015, 5, 21, 4, 57, 3, 540997)

可以通过不同的关键字参数设定具体相差的日数，小时数等等。

####本地时间转换为UTC时间

UTC时间特指的是格林威治时间，即**零时区(UTC+0:00)的时间**；本地时间则是**系统设定时区的时间**。

datetime类有一个属性 `tzinfo` 专门用来表示时区，默认是None，也即默认产生的datetime是不带时区信息的。

    >>> from datetime import datetime, timedelta, timezone
    >>> tz_utc_8 = timezone(timedelta(hours=8)) # 创建时区UTC+8:00
    >>> now = datetime.now()
    >>> now
    datetime.datetime(2015, 5, 18, 17, 2, 10, 871012)
    >>> dt = now.replace(tzinfo=tz_utc_8) # 强制设置为UTC+8:00
    >>> dt
    datetime.datetime(2015, 5, 18, 17, 2, 10, 871012, tzinfo=datetime.timezone(datetime.timedelta(0, 28800)))

**注意**：

教程里说如果系统时区不是UTC+8:00，上面这段代码就不会成功，因为**不能强制设置错误的时区**。 但实际上我用Win10+Python3是可以强制设置别的时区的。

####时区转换

通过 `astimezone()` 这个函数可以将任意**带时区信息**的datetime对象进行时区转换。 如果没有设置过时区信息就要先构造一个带时区信息的datetime对象。

    # 拿到UTC时间，并强制设置时区为UTC+0:00:
    >>> utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
    >>> print(utc_dt)
    2015-05-18 09:05:12.377316+00:00
    # astimezone()将转换时区为北京时间:
    >>> bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
    >>> print(bj_dt)
    2015-05-18 17:05:12.377316+08:00
    # astimezone()将转换时区为东京时间:
    >>> tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
    >>> print(tokyo_dt)
    2015-05-18 18:05:12.377316+09:00

要构造带时区信息的datetime对象可以像前一小节那样直接replace掉tzinfo属性，也可以像这里这样构造一个UTC+0:00时区的datetime对象，然后再转换。

####小结

1. datetime表示的时间必须带有时区信息才能确定一个特定的时间，否则只能视为本地时间。
2. 要存储datetime最佳方法是转换为timestamp再存，因为timestamp值跟时区无关。

####作业

假设用户输入字符串格式的时间和时区信息，将它们转换为timestamp输出：

    # -*- coding:utf-8 -*-

    import re
    from datetime import datetime, timezone, timedelta

    def to_timestamp(dt_str, tz_str):
        #先创建时间
        dt = datetime.strptime(dt_str, '%Y-%m-%d %H:%M:%S')

        #然后使用正则表达式匹配时区
        #这里正则的意思是以UTC开头，找出两个分组，分组1匹配正负号，分组2匹配时区号
        #注意正负号和时区号中间可能有0，所以用0?匹配表示0~1个0字符
        #[]中匹配正负号不需要转义，最后结尾是:00
        tz_match=re.match(r'^UTC([+-])0?(\d):00$',tz_str)
        tz_int = int(tz_match.group(1)+tz_match.group(2))
        tz = timezone(timedelta(hours=tz_int))

        #把时区信息设置到时间对象中，最后转换为timestamp返回
        dt = dt.replace(tzinfo=tz)
        return dt.timestamp()


    # 测试:
    if __name__ == '__main__':
        t1 = to_timestamp('2015-6-1 08:10:30', 'UTC+7:00')
        assert t1 == 1433121030.0, t1

        t2 = to_timestamp('2015-5-31 16:10:30', 'UTC-09:00')
        assert t2 == 1433121030.0, t2

        print('Pass')

###collections

collections内建模块提供了许多有用的集合类。

####namedtuple

namedtuple是一个函数，可以用来创建一个自定义的tuple对象，并且规定tuple中元素的个数，可以用属性来访问这个自定义tuple中的元素。

    >>> from collections import namedtuple
    >>> Point = namedtuple('Point', ['x', 'y'])
    >>> p = Point(1, 2)
    >>> p.x
    1
    >>> p.y
    2

比如这个例子中定义了一个Point对象，它继承自tuple，有两个元素x和y，也可以理解为两个属性x和y。 这样我们既能更准确地描述对象，又不需要创建一个类这么麻烦。

    >>> isinstance(p, Point)
    True
    >>> isinstance(p, tuple)
    True

可以看到定义的Point确实是tuple的子类，注意 `Point = namedtuple('Point', ['x', 'y'])` 中等式左边是类名，可以不必和等式右边的名相同，但是一般为了代码可读性还是保证一致~~

####deque

使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为list是线性存储，数据量大的时候，插入和删除效率很低。

deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈：

    >>> from collections import deque
    >>> q = deque(['a', 'b', 'c'])
    >>> q.append('x')
    >>> q.appendleft('y')
    >>> q
    deque(['y', 'a', 'b', 'c', 'x'])

deque除了实现list的 `append()` 和`pop()` 外，还支持 `appendleft()` 和 `popleft()`，这样就可以非常高效地往头部添加或删除元素。
