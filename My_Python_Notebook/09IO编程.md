# IO编程

## 目录

<!-- MarkdownTOC -->

- [什么是IO](#什么是io)
- [文件读写](#文件读写)
    - [读文件](#读文件)
    - [写文件](#写文件)
    - [file-like Object](#file-like-object)
    - [二进制文件](#二进制文件)
    - [字符编码](#字符编码)
    - [小结](#小结)
- [StringIO和BytesIO](#stringio和bytesio)
    - [StringIO](#stringio)
    - [BytesIO](#bytesio)
    - [为什么使用StringIO和BytesIO](#为什么使用stringio和bytesio)
    - [读写IO需要注意的地方](#读写io需要注意的地方)
    - [小结](#小结-1)
- [操作目录和文件](#操作目录和文件)
    - [简述](#简述)
    - [环境变量](#环境变量)
    - [操作目录和文件](#操作目录和文件-1)
    - [小结](#小结-2)
    - [练习](#练习)
- [序列化](#序列化)
    - [序列化和反序列化](#序列化和反序列化)
    - [序列化（pickle）](#序列化（pickle）)
    - [JSON](#json)
    - [JSON进阶](#json进阶)
    - [小结](#小结-3)

<!-- /MarkdownTOC -->


## 什么是IO

IO在计算机中指**输入和输出（Input/Output）**。由于程序运行时，数据是在内存中驻留，并由CPU这个超快的计算核心来进行处理的（处理时会把数据从内存载入到CPU的高速缓存中），而涉及到数据交换的操作，比如磁盘读写、网络传输等的时候，就需要使用IO接口来协调了。

比如你打开浏览器，访问新浪首页，浏览器这个程序就需要通过网络IO获取新浪的网页。浏览器首先会发送数据给新浪服务器，告诉它我想要首页的HTML，这个动作是往外发数据，叫Output，随后新浪服务器把网页发过来，这个动作是从外面接收数据，叫Input。所以，通常，程序完成IO操作会有Input和Output两个数据流。当然也有只用一个的情况，比如，从磁盘读取文件到内存，就只有Input操作，反过来，把数据写到磁盘文件里，就只是一个Output操作。

IO编程中，**流（Stream）是**一个很重要的概念，可以把流想象成一个水管，**数据就是水管里的水，但是只能单向流动**。Input Stream就是数据从外面（磁盘、网络）流进内存，Output Stream就是数据从内存流到外面去。对于浏览网页来说，浏览器程序和新浪服务器之间至少需要建立两根水管，才可以既能发数据，又能收数据。

由于CPU和内存的速度远远高于外设的速度，所以，在IO编程中，就存在**速度严重不匹配**的问题。举个例子来说，比如要把100M的数据写入磁盘，CPU输出100M的数据只需要0.01秒，可是磁盘要接收这100M数据可能需要10秒，怎么办呢？有两种办法：

- 第一种方法是让CPU等待，也就是程序暂停执行后续代码，等100M的数据在10秒后写入磁盘，再接着往下执行，这种模式称为**同步IO**；

- 第二种方法是CPU不等待，只是告诉磁盘，“您老慢慢写，不着急，我接着干别的事去了”，于是，后续代码可以继续执行，这种模式称为**异步IO**。

同步和异步的区别就在于是否等待IO执行的结果。好比你去麦当劳点餐，你说“来个汉堡”，服务员告诉你，对不起，汉堡要现做，需要等5分钟，于是你站在收银台前面等了5分钟，拿到汉堡再去逛商场，这是同步IO。

你说“来个汉堡”，服务员告诉你，汉堡需要等5分钟，你可以先去逛商场，**等做好了，我们再通知你**，这样你可以立刻去干别的事情（逛商场），这是异步IO。

很明显，使用异步IO来编写程序性能会远远高于同步IO，但是异步IO的缺点是编程模型复杂。想想看，你得知道什么时候通知你“汉堡做好了”，而通知你的方法也各不相同。如果是服务员亲自跑过来找到你，这是**回调模式**，如果服务员发短信通知你，你就得不停地检查手机，这是**轮询模式**。总之，异步IO的复杂度远远高于同步IO。

**操作IO的能力都是由操作系统提供的**，**编程语言所做的只是把操作系统提供的低级C接口封装起来方便使用**，Python也不例外。后面的小节中会详细讨论Python的IO编程接口。

注意，本章的IO编程都是同步模式，异步IO由于复杂度太高，后续涉及到服务器端程序开发时会再作讨论。

---

<br>

## 文件读写

读写文件是最常见的IO操作。Python内置了读写文件的函数，用法和C是兼容的。

读写文件前，我们先必须了解一下，**在磁盘上读写文件的功能都是由操作系统提供的**，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

---

### 读文件

要以读文件的模式打开一个文件对象，可以使用Python内置的 `open()` 函数，传入文件名（如果文件和代码文件在相同文件夹下就可以省略路径）和标示符 `'r'`：

```python
>>> f = open('/Users/michael/test.txt', 'r')
```

标示符 `'r'` 表示读，这样，我们就成功地打开了一个文件。

如果文件不存在，`open()` 函数就会抛出一个 `IOError` 的错误，并且给出错误码和详细的信息告诉你文件不存在：

```python
>>> f=open('/Users/michael/notfound.txt', 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: '/Users/michael/notfound.txt'
```

如果文件打开成功，我们就可以调用 `read()` 方法来一次读取文件的全部内容，Python会把为文件的内容读到内存，返回的是一个 `str` 对象：

```python
>>> f.read()
'Hello, world!'
```

读取完毕后，如果不需要继续操作文件对象，我们就应当调用 `close()` 方法来关闭它。因为**文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的**：

```python
>>> f.close()
```

由于读写文件都有可能产生 `IOError`，一旦出错，后面的 `f.close()` 就不会调用。所以，为了**保证无论是否出错都能正确地关闭文件**，我们可以使用 `try ... finally` 来实现：

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

但是每次都这么写实在太繁琐，所以，Python引入了 `with` 语句来自动帮我们调用 `close()` 方法（（在[上一章](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook/08%E9%94%99%E8%AF%AF%E3%80%81%E8%B0%83%E8%AF%95%E4%B8%8E%E6%B5%8B%E8%AF%95.md)中有 `with` 语句使用及原理的介绍））：

```python
with open('/path/to/file', 'r') as f:
    print(f.read())
```

这和前面的 `try ... finally` 实现的效果是一样的，但是代码更简洁，并且我们不必调用 `f.close()` 方法。

调用 `read()` 方法可以一次性读取文件的全部内容。但如果文件有10G，内存就爆了，所以，为了保险起见，我们可以多次调用 `read(size)` 方法，每次最多读取size个**字节**的内容。

但是有时候文件不一定有严格的格式，比方说读取一篇文章，这时按字节读取就不太合适了。但我们可以调用 `readline()` 方法，`readline()` 方法每次读取文件的一行内容。而调用 `readlines()` 方法则会一次读取文件的所有内容并按行返回一个 `list` 对象。我们可以：

```python
for line in f.readlines():
    print(line.strip()) # 把末尾的换行符'\n'删掉再打印
```

---

### 写文件

写文件和读文件是一样的，唯一区别是调用 `open()`函数时，传入标识符 `'w'` 或者 `'wb'` 表示写文本文件或写二进制文件：

```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```

你可以多次调用 `write()` 来写入文件，但是**最后一定要调用 `f.close()` 来关闭文件**。当我们写文件时，操作系统往往不会立刻把数据写入磁盘，而是**放在内存中缓存起来，空闲的时候再慢慢写入**。只有调用 `close()` 方法时，操作系统才会保证把没有写入的数据全部写入磁盘。忘记 `close()` 的后果是**数据可能只有一部分写到了磁盘，剩下的丢失了**。为了避免这样的情况发生，类似上一节所介绍的，我们可以使用 `with` 语句自动管理上下文：

```python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```

如果要写入特定编码的文本文件，还可以给 `open()` 函数传入 `encoding` 参数，将要写入的字符串自动转换成指定编码。

---

### file-like Object

在Python中，除了文件对象之外，内存中的字节流，网络流，自定义流等等，拥有 `read()` 方法的对象统称为 **`file-like Object`**。`file-like Object` 不需要继承自特定的类，只要有 `read()` 方法就行（文件对象的其他方法不一定都需要实现，可以看看[官方说明](https://docs.python.org/2.4/lib/bltin-file-objects.html)），这得益于Python鸭子类型的实现。`StringIO` 就是在内存中创建的 `file-like Object`，常用作临时缓冲。

---

### 二进制文件

前面讲的默认都是读取文本文件，并且是UTF-8编码的文本文件。要读取二进制文件，比如图片、视频等等，用 `'rb'` 模式打开文件即可：

```python
>>> f = open('/Users/michael/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```

---

### 字符编码

要读取非UTF-8编码的文本文件，可以给 `open()` 函数传入 `encoding` 参数，例如，读取GBK编码的文件：

```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
```

遇到有些编码不规范的文件，你可能会遇到 `UnicodeDecodeError`，因为在文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，`open()` 函数还接收一个 `errors` 参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：

```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

---

### 小结

在Python中，文件读写是通过 `open()` 函数打开的文件对象完成的。使用 `with` 语句操作文件IO是个好习惯。

---

<br>

## StringIO和BytesIO

### StringIO

很多时候，数据读写不一定是对文件进行的，我们也可以在内存中进行读写操作。

`StringIO` 顾名思义就是**在内存中读写 `str`**。

要把 `str` 写入 `StringIO`，我们需要先创建一个 `StringIO` 对象，然后，像文件一样写入即可：

```python
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
```

`getvalue()` 方法用于**获得IO流中的全部内容**。

读取 `StringIO` 的方法也和读文件类似：

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!') # 用一个str初始化StringIO
>>> while True:
...     s = f.readline()
...     if s == '': # 读取完毕，跳出循环
...         break
...     print(s.strip()) # 去掉当前行首尾的空格再打印
...
Hello!
Hi!
Goodbye!
```

---

### BytesIO

`StringIO` 操作的只能是 `str`，如果要操作二进制数据，就需要使用 `BytesIO`。

`BytesIO` 实现了在内存中读写 `bytes`，我们创建一个 `BytesIO`，然后写入一些 `bytes`：

```python
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

请注意，写入的不是 `str`，而是经过UTF-8编码的 `bytes`。

和 `StringIO` 类似，读取 `BytesIO`：

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87') # 用一个bytes初始化BytesIO
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```

---

### 为什么使用StringIO和BytesIO

这个问题在Stackoverflow上有[回答](http://stackoverflow.com/questions/4733693/when-is-stringio-used)，为什么我们不直接使用 `str` 和 `bytes`，而要这么别扭地在内存中使用 `StringIO` 和 `BytesIO` 呢？其实呀，主要是因为 **`StringIO` 和 `BytesIO` 都是 `file-like Object`**，可以像文件对象那样使用，当某些库的函数支持文件对象时，我们可以传入 `StringIO` 和 `BytesIO`，也能使用，这一点 `str` 和 `bytes` 是没法做到的。

---

### 读写IO需要注意的地方

以 `StringIO` 为例，前面我们将到读取 `StringIO` 时，是**先使用字符串进行初始化，然后再读取**：

```python
>>> f = StringIO('Hello!\nHi!\nGoodbye!') # 用一个str初始化StringIO
>>> f.readlines()
['Hello!\n', 'Hi!\n', 'Goodbye!']
```

但如果我们**没有进行初始化，而是对一个空的 `StringIO` 进行写入，然后再读取**呢？这时就会像下面这样：

```python
>>> f = StringIO()
>>> f.write('Hello!\n')
7
>>> f.write('Hi!\n')
4
>>> f.write('Goodbye!')
8
>>> f.readlines()
[]
```

我们发现此时读取不到写入的字符串了，这是为什么呢？其实呀，这时因为**当前所处流的位置（Stream Position）在末尾**，所以读取不到东西了。那怎么知道当前处在流的什么位置呢？我们可以使用 `tell()` 方法。对比一下：

使用字符串初始化 `StringIO`：

```python
>>> f = StringIO('Hello!\nHi!\nGoodbye!')
>>> f.tell()
0
```

写入 `StringIO`：

```python
>>> f = StringIO()
>>> f.write('Hello!\n')
7
>>> f.write('Hi!\n')
4
>>> f.write('Goodbye!')
8
>>> f.tell()
19
```

可以发现，使用字符串初始化时，位置会保持在流的开头，而使用 `write()` 方法对流进行写入操作后，位置会移动到**写入结束的地方**。那有没有办法在写入以后进行读取呢？有！可以使用前面提到的 `getvalue()` 方法读取IO流中的全部内容，另外也可以使用 `seek()` 方法回到前面的某一位置，然后读取该位置后的内容：

```python
>>> f.tell()
19
>>> f.seek(0) # 回到流的开头位置
0
>>> f.tell()
0
```

有时候呀，我们可能会在初始化一个 `StringIO` 之后，想要对其进行写入操作，这时会发生一个问题：

```python
>>> f = StringIO('Hello!')
>>> f.getvalue()
'Hello!'
>>> f.write('Hi!')
3
>>> f.getvalue()
'Hi!lo!'
```

可以看到初始化的内容被写入的内容覆盖了，这显然不是我们所希望的。为什么会这样呢？其实呀，跟前面说的问题是一样的，举一反三，都是因为Stream Position引起的。初始化一个 `StringIO` 后，位置在流的开头，此时写入就会从流的开头写入，而不是像我们所希望的那样从流的末尾写入，稍微改动一下就好了：

```python
>>> f = StringIO('Hello!')
>>> f.seek(0, 2) # 移动到流的末尾
6
>>> f.write('Hi!')
3
>>> f.getvalue()
'Hello!Hi!'
```

除了移动到流的末尾，也能移动到某个位置，看看 `seek()` 方法的描述：

```python
Help on built-in function seek:

seek(pos, whence=0, /) method of _io.StringIO instance
    Change stream position.

    Seek to character offset pos relative to position indicated by whence:
        0  Start of stream (the default).  pos should be >= 0;
        1  Current position - pos must be 0;
        2  End of stream - pos must be 0.
    Returns the new absolute position.
```

可以看到 `seek()` 方法有必选参数 `pos` 和 可选参数 `whence`，前者是移动多少，后者是从哪里开始移动。`whence` 默认为0，也即默认从流的开头移动 `pos` 个位置。

---

### 小结

`StringIO` 和 `BytesIO` 是在内存中操作 `str` 和 `bytes` 的方法，和读写文件具有一致的接口。

---

<br>

## 操作目录和文件

### 简述

在命令行下，我们可以通过输入操作系统提供的各种命令，比如dir、cp等，来操作目录和文件。这些命令的本质其实就是简单地调用了**操作系统提供的接口函数**。

那如果想在Python程序中操作目录和文件该怎么办呢？Python内置的 `os` 模块同样给与我们调用操作系统提供的接口函数的能力。

打开Python交互式命令行，首先看看如何使用 `os` 模块的基本功能：

```python
>>> import os
>>> os.name
'posix'
```

Linux、Unix和Mac OS X系统返回的是 `posix`，Windows系统返回的则是 `nt`。

要获取详细的系统信息，可以调用 `uname()` 函数：

```python
>>> os.uname()
posix.uname_result(sysname='Darwin', nodename='MichaelMacPro.local', release='14.3.0', version='Darwin Kernel Version 14.3.0: Mon Mar 23 11:59:05 PDT 2015; root:xnu-2782.20.48~5/RELEASE_X86_64', machine='x86_64')
```

注意，`uname()` 函数在Windows系统上不提供，也就是说，**`os` 模块的能否使用某些函数取决于使用者的操作系统**。

---

### 环境变量

在操作系统中定义的环境变量，全部保存在 `os.environ` 变量中。我们可以直接查看操作系统的所有环境变量：

```python
>>> os.environ
environ({'VERSIONER_PYTHON_PREFER_32_BIT': 'no', 'TERM_PROGRAM_VERSION': '326', 'LOGNAME': 'michael', 'USER': 'michael', 'PATH': '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin', ...})
```

要获取某个环境变量的值，可以调用使用 `os.environ.get('key')` 的方式：

```python
>>> os.environ.get('PATH')
'/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/mysql/bin'
>>> os.environ.get('x', 'default')
'default'
```

传入某个环境变量的名称，得到对应的路径。除此之外还可以传入一个字符串作为默认路径（**没有可返回的路径时**会返回默认路径）。

---

### 操作目录和文件

除了前面的 `os` 模块中，操作目录和文件的函数还有一部分放在 `os.path` 模块中。比方说用于生成绝对路径的 `abspath()` 函数：

```python
>>> os.path.abspath('.') # 点符代表当前工作路径
'F:\\Python35'
>>> os.path.abspath('Tools\\demos')
'F:\\Python35\\Tools\\demos'
```

在[05模块](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook/05%E6%A8%A1%E5%9D%97.md)中归纳过文件搜索路径的一些知识，当我们在程序中需要用到某个文件时，可以使用两种方式来让程序查找到这个文件：

- 一是使用绝对路径，也即完整的路径；
- 二是使用相对路径（相对当前工作路径而言的路径），并且可以使用点符 `.` 来替代当前工作路径。

注意，**使用相对路径时是可以不使用点符的**，所以上面代码中，为 `Tools\\demos` 生成绝对路径也同样可行。

接下来我们试试创建目录和删除目录：

```python
# 在某个目录下创建一个新目录，首先生成新目录的完整路径:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```

**把两个路径合成一个时，不要直接拼字符串**，而要通过 `os.path.join()` 函数，这样可以**正确处理不同操作系统的路径分隔符**。在Linux/Unix/Mac下，`os.path.join('part1','part2')` 返回这样的字符串：

```python
part-1/part-2
```

而Windows下会返回这样的字符串：

```python
part-1\part-2
```

同样的道理，**要拆分路径时，也不要直接去拆字符串**，而要通过 `os.path.split()` 函数，这样可以把一个路径拆分为两部分，后一部分总是**最后级别的目录或文件名**：

```python
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
```

`os.path.splitext()` 函数可以用来**获取文件扩展名**，很多时候非常方便：

```python
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```

这些合并、拆分路径的函数**并不要求目录和文件真实存在**，它们只是对字符串进行操作。

文件操作使用下面的函数。假定当前目录下有一个 `test.txt` 文件：

```python
# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
```

但是 `os` 模块中不存在复制文件的函数！原因是**复制文件并非是由操作系统提供的系统调用**。理论上讲，我们通过上一节的读写文件可以完成文件复制，只不过要多写很多代码。

幸运的是 `shutil` 模块提供了 `copyfile()` 的函数，你还可以在 `shutil` 模块中找到很多实用函数，它们可以看做是对 `os` 模块的补充。

最后看看如何利用Python的特性来过滤文件。比如我们要列出当前目录下的所有目录，只需要一行代码：

```python
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]
```

要列出所有的 `.py` 文件，也只需一行代码：

```python
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']
```

是不是非常简洁？

---

### 小结

Python的 `os` 模块封装了操作系统的目录和文件操作，要注意这些函数有的在 `os` 模块中，有的在 `os.path` 模块中。

### 练习

#### 习题1

> 利用 `os` 模块编写一个能实现 `ls -l` 输出的程序。

先看看 `ls -l` 做的是什么：

```shell
ubuntu@ubuntu:~/HumanFaceRecognitionWithNN$ ls -l
total 344
-rw-rw-r-- 1 ubuntu ubuntu  10301 Dec 14 22:28 face_recognition.py
drwxrwxr-x 3 ubuntu ubuntu   4096 Dec 21 15:50 test
-rw-rw-r-- 1 ubuntu ubuntu 328506 Dec 10 15:39 yaleB_face_dataset.mat
```

注意这是在Linux上执行的，我们想查看当前路径下有什么文件和文件夹可以使用 `ls` 或者 `dir` 命令，而如果我们想了解更详细的信息则可以用 `ls -l` 或者`dir -l` 命令。

这里稍微解析一下返回的信息吧，以下面这一条为例：

| field1 | field2 | field3 | field4 | field5 | field6 | field7 | field8 | field9 | field10 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| - | rw- | rw- | r-- | 1 | ubuntu | ubuntu | 10301 | Dec 14 22:28 | face_recognition.py |

- field1 是 `File type flag`，**标识文件类型**，如果是 `-` 则表明是普通文件，是 `d` 则表明是一个目录；
- field2、field3、field4 依次是拥有者、拥有者所在的用户组以及其他用户对**该文件/文件夹的操作权限**，`r` 表示可读，`w` 表示可写， `x` 表示可执行。
- field5 是**所含链接数**，如果该项是一个文件，则链接数为1；如果该项是一个目录，则一般为**该目录下子文件夹数+2**。为什么呢？因为当前目录（该项的父目录）有一条指向该项的链接，而对文件夹来说，除了父目录的链接之外，它本身还有一条 `.` 链接指向自身，并且它的子目录都有一条 `..` 链接指向它；
- field6 是**拥有者的名字**；
- field7 是**拥有者所在的用户组**；
- field8 是**该项的大小**（多少bytes）；
- field9 是**最后修改该项的日期和时间**；
- field10 是**该项的名字**。

我们注意到，除了每一项的详细信息之外，最前面还有一行输出 `total 344`，这个344是什么呢？它指的是当前目录所有文件和文件夹所使用的块（block）的数目，块是一个操作系统的概念，这里不详细展开。如果我们想知道当前目录下每一项所使用的块的数目，可以使用 `ls -s`命令：

```shell
ubuntu@VM-173-69-ubuntu:~/HumanFaceRecognitionWithNN$ ls -s
total 344
 12 face_recognition.py    4 test  328 yaleB_face_dataset.mat
```

加起来总数正是 `344`。

以上内容参考了以下几个链接：

- [What is that “total” in the very first line after ls -l?](http://stackoverflow.com/questions/7401704/what-is-that-total-in-the-very-first-line-after-ls-l)
- [command ls -l output explained](http://go2linux.garron.me/command-ls-file-permissions/)
- [What do the fields in ls -al output mean?](http://unix.stackexchange.com/questions/103114/what-do-the-fields-in-ls-al-output-mean)
- [What does each part of the `ls -la` output mean?](http://askubuntu.com/questions/710905/what-does-each-part-of-the-ls-la-output-mean)

题目要求实现Python版的 `ls -l`，理论上应该是可行的，但上面的内容只涉及到 `os` 和 `os.path` 模块中很少的函数，其他的还有待发掘。我暂时没有时间去琢磨，所以先略过这一题。

#### 习题2

> 编写一个程序，能在当前目录以及当前目录的所有子目录下查找文件名包含指定字符串的文件，并打印出相对路径。

```python
import os

def search(s, path=os.getcwd()):
    filelst = [x for x in os.listdir(path)]
    for filename in filelst:
        filepath = os.path.join(path, filename)
        # print('Searching: ', path, '\nWith: ', filepath)
        if os.path.isfile(filepath):
            if s in filename:
                print(os.path.relpath(filepath))
        elif os.path.isdir(filepath):
            search(s, filepath)

if __name__ == '__main__':
    s = input('Enter the string: ')
    search(s)
```

这题还是挺有意思的，用户自己定义搜索的字符串，我们不仅要找出当前目录下包含该字符串的文件，还要搜索所有的子目录。我们可以把搜索一个目录的过程封装为 `search` 函数，并采用递归的方式来实现其子目录的搜索。思路如下：

1. 获取当前目录的所有文件&目录名，使用 `os.listdir()` 函数可以实现，把这些名称放在一个列表里保存；

2. 接下来逐个遍历并判断列表中的元素是文件还是目录，可以使用 `os.path.isfile()` 和 `os.path.isdir()` 函数；
    - 如果当前遍历到的元素是文件，则使用 `os.path.relpath()` 函数输出文件的相对路径；
    - 如果当前遍历到的元素是目录，则将该目录的路径传入 `search` 函数。

特别地，我们要注意这些函数应该输入什么和会输出什么。`os.path.relpath()` 函数接收一条完整的绝对路径，并输出**相对于当前工作路径（在命令行中执行该Python文件时所处的路径）的相对路径**，所以我们要先构造出正确的绝对路径，才能获取正确的相对路径。

`os.path.isdir()` 可以接收相对路径也可以接收绝对路径，因为我们使用 `os.listdir()` 只能获得文件或目录的名称，在搜索子目录时，这些名称并不是相对于当前工作路径的相对路径，所以不能直接传入 `os.listdir()` 中，必须先构造绝对路径，然后再判断。

为什么不使用 `os.path.abspath()` 函数来生成绝对路径呢？因为**传入 `os.path.abspath()` 函数的必须是一条正确的相对路径，才会得到正确的相对路径**。举个例子，当前工作路径是 `C:\Users\Administrator\Desktop`，其子目录 `test1` 中有一个文件 `test2.py`，如果我们使用 `os.path.abspath('test2.py')`，那么得到的绝对路径就变成了 `C:\Users\Administrator\Desktop\test2.py`，显然是不对的。

---

<br>

## 序列化

### 序列化和反序列化

在程序运行的过程中，所有的变量都保存在内存中，而一旦程序结束，变量所占用的内存就会被操作系统全部回收。但是，有时候，我们希望通过程序修改了某个变量的值之后，能够让另一个程序能调用这个变量。比方说在程序1中定义了一个 `list`，并且经过某些高开销的操作修改了这个 `list` 的值。如果我们想在程序2中使用程序1中修改后的 `list`，按之前的做法就只能把程序1作为一个模块，在程序2中执行程序1的代码，这样一来，就必须重复执行高开销的操作了。有没有解决这个问题的方法呢？有的，答案就是**序列化**。

我们把**将变量从内存中保存的格式变成可存储或可传输的格式这个过程称之为序列化**，在Python中叫 `pickling`，在其他语言中也被称之为 `serialization`，`marshalling`，`flattening` 等等，都是一个意思。经过序列化之后，内存中的变量就由原来的格式（某种数据结构/类型）转换为特定的格式，从而可以被存储或传输，这样另一个程序需要用到时就可以直接读取，而不必重复计算了。

反过来，把**将变量内容从序列化的对象重新读到内存里还原为原来的格式这一过程称之为反序列化**，即 `unpickling`。

---

### 序列化（pickle）

在[01Python基础](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook/01Python%E5%9F%BA%E7%A1%80.md)中，我们就知道**传输和存储都是以字节（bytes）为单位的**，所以这节首先介绍一种将变量序列化为 `bytes` 对象的方法，Python提供了 `pickle` 模块来实现这一功能。

以 `dict` 为例，将一个 `dict` 类型的对象序列化并写入文件：

```python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```

`pickle.dumps()` 函数可以把任意变量序列化为一个 `bytes` 对象。我们可以把这个 `bytes` 对象写入文件。此外，我们也可以用 `pickle.dump()` 函数直接把对象序列化后**写入一个 `file-like Object`**：

```python
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```

打开 `dump.txt` 文件，我们会看到一堆乱七八糟无法阅读的内容，这些都是Python保存的对象信息。

当我们需要还原变量，也即把对象从磁盘读到内存时，可以先把内容读入到一个 `bytes` 对象中，然后用 `pickle.loads()` 方法反序列化出对象，也可以直接用 `pickle.load()` 方法从一个 `file-like Object` 中直接反序列化出对象。打开另一个Python命令行，试试反序列化刚才保存的对象：

```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

可以看到我们成功地还原了这个 `dict` 类型的对象。

有了 `Pickle` 之后，我们可以方便地在Python中进行序列化和反序列化。但是，和所有其他编程语言的序列化问题一样，`Pickle` 是一种Python特有的序列化解决方案，它只能用于Python，甚至不同版本的Python彼此都可能不兼容。如果我们使用Python写程序，而别人使用其他语言，比如Java，C++等，它们没有 `Pickle` 模块也就没办法进行反序列化了。

---

### JSON

如果我们要**在不同的编程语言之间传递对象**，就必须把对象序列化为通用的**标准格式**，比如序列化**XML（Extensible Markup Language，可扩展标记语言）**。但更好的方法是序列化为**JSON（JavaScript Object Notation，JavaScript对象表示法）**。因为JSON表示出来就是一个字符串，可以被所有语言读取，也可以方便地存储到磁盘或者通过网络传输。JSON不仅是标准格式，并且比XML更快，还可以直接在Web页面中读取，非常方便。

比较一下Python内置的数据类型和JSON中的表示方式：

| Python类型 | JSON表示 |
|:-:|:-:|
| dict | {} |
| list | [] |
| str | "string" |
| int, float | 10, 1234.56 |
| True/False | true/false |
| None | null |

Python内置的 `json` 模块提供了非常完善的Python对象到JSON格式的转换方法。同样对一个 `dict` 进行序列化，方法如下：

```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

`dumps()` 方法**返回一个 `str`**，内容就是标准的JSON。类似的，`dump()` 方法可以直接把JSON写入一个 `file-like Object`。

要把JSON反序列化为Python对象，用 `loads()` 或者对应的 `load()`方法，前者把JSON的字符串反序列化，后者从 `file-like Object` 中读取字符串并反序列化：

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

由于JSON标准规定**JSON编码是UTF-8**，所以我们总是能正确地在Python的 `str` 与JSON之间的转换。

---

### JSON进阶

Python的 `dict` 对象可以直接序列化为JSON的 `{}`，不过很多时候，Python自带的数据结构并不足以实现我们的需求，此时我们会**使用自定义的类来表示对象**。比如定义一个Student类，并尝试序列化该类的实例：

```python
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)
print(json.dumps(s))
```

运行代码，毫不留情地得到一个 `TypeError`：

```python
Traceback (most recent call last):
  ...
TypeError: <__main__.Student object at 0x10603cc50> is not JSON serializable
```

错误的原因是Student对象**不是一个可序列化为JSON的对象**。

这样看来还是不够实用呀，别急，我们再仔细看看 `dumps()` 方法的参数列表，可以发现，除了第一个必须的 `obj` 参数外，`dumps()` 方法还提供了一大堆的可选参数：

```python
>>> help(json.dumps)
Help on function dumps in module json:

dumps(obj, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None,
 indent=None, separators=None, default=None, sort_keys=False, **kw)

...
```

这些可选参数就是允许我们对JSON序列化进行定制的。前面的代码之所以无法把Student类实例序列化为JSON，是因为默认情况下，`dumps()` 方法不知道如何将Student实例变为一个JSON的 `{}` 对象。

可选参数 `default` 允许我们传入一个可以把传入对象变得可序列化的函数，我们只需要为Student专门写一个转换函数，再把函数传进去即可，例如定义：

```python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```

这样，Student实例首先被 `student2dict()` 函数转换成 `dict`，然后再被序列化为JSON：

```python
>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```

不过，下次如果遇到一个Teacher类的实例，我们还是无法把Teacher类的实例序列化为JSON。有没有更方便的做法呢？有的，我们可以偷个懒，利用 `__dict__` 属性即可：

```python
print(json.dumps(s, default=lambda obj: obj.__dict__))
```

通常类的实例都有一个 `__dict__` 属性，它就是一个 `dict`。但也有少数例外，比如定义了 `__slots__` 的类（这样的类没有 `__dict__` 属性）。

同样的道理，如果我们要把JSON反序列化为一个Student对象实例，`loads()`方法会首先转换出一个 `dict` 对象，然后，参数 `object_hook` 则允许我们传入一个函数，负责把 `dict` 转换为Student实例：

```python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])
```

运行结果如下：

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```

打印出的是反序列化后的Student实例对象。

---

### 小结

Python语言特定的序列化模块是 `pickle`，但如果想把序列化做得更通用、更符合Web标准，可以使用 `json` 模块。

`json` 模块的 `dumps()` 和 `loads()` 函数是定义得非常好的接口的典范。当我们使用时，只需要传入一个必须的参数。但是，当默认的序列化或反序列机制不满足我们的要求时，我们又可以传入更多的参数来定制序列化或反序列化的规则，既做到了接口简单易用，又做到了充分的扩展性和灵活性。
