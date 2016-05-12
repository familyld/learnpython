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

