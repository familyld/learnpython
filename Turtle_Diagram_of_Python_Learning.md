# Python学习入门神图

这是一张国外程序员画出Python学习神图，国内有人翻译成了中文，非常适合入门者初步了解Python的一些特性和使用方式。

![Turtle_Diagram_of_Python_Learning](https://github.com/familyld/learnpython/tree/master/graph/Turtle_Diagram_of_Python_Learning.png)

从上往下过一遍：

- Python代码文件结尾.py，和c语言的.c，c++的.cpp，java的.java类似，这个没什么好说的。

- 在代码中如果使用到中文，最好在头部声明`#coding=utf-8`或者`#coding=gbk`，这样存储代码文件时就不会以ASCII码保存而是使用我们指定的编码方式，避免了再次打开时变成乱码。这点在我们使用中文注释时特别有用。在[My_Python_Notebook](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook.md)的`中文注释`这个小节我也有提到。

- 除了可以用UliPad编辑器之外用Sublime也很不错，我就是使用Sublime文本编辑器进行编写的，可以使用ConvertToUTF8插件来实现，安装好保存加载都使用UTF8格式，支持多种语言的使用。

- 类似C/C++语言和Java语言，我们可以通过引入头文件来在Python中调用其他代码文件中定义的函数或者结构，通过`import`语句可以实现这一点，还有`from libName import structName`这样的用法，只加载库的某个部分或某个函数。这里说的模块指的就是一个.py文件，为了避免模块名重复，Python还引入了包名的机制，详见[My_Python_Notebook](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook.md)的`模块`章节。

- Python有一个很友好但也饱受诟病的特点就是代码块用4个空格的缩进表示，缩进相同表示在同一级代码块中，不需要像其他语言那样使用括号，编写时清晰简单，但是有时复制粘贴代码容易因为这点出漏子。

- 我们可以定义一个main函数，但不像java和c/c++，python脚本中的main函数和其他函数一样是不会自动被调用的，它的命名随我们定，甚至不需要叫做main函数。当我们运行脚本时实际上首先执行的是第一行没有缩进的非def的语句，我们在语句中调用函数才会执行函数。

- 看到图的底部`if __name__ == '__main__':`这个代码块，其中 `__name__` 变量是个特殊变量，当我们**在命令行执行**这个.py文件时，Python解释器就会把`__name__` 变量赋值为 `__main__`。如果在别的地方导入这个.py文件时，不会有对 `__name__` 赋值的操作，if判断就会失败。 借助这个特性，我们可以**在if判断中编写一些额外的代码**，用于在命令行运行时执行。**运行测试**就需要这样做。

- 在Python中并没有说单引号是字符双引号是字符串这个说法，都可以用，都视为字符串变量。注意引号内转义符的处理。

- 在Python中函数声明的顺序是没有要求的，只要能找到就可以了，所以图中可以在main函数中定义调用foo函数，并且foo函数不需要在main函数前面声明。

- 我们可以使用`模块名.函数名`来调用模块中的函数，注意之前一定要import。

- Python中的变量是动态弱类型，按引用传递的，所以同一个变量可以显示int型然后变成str型。注意变量调用前先要赋值(实例化)。

- Python的列表类型很强大，是我们在C/C++中使用的数组的加强版，里面可以放多种不同类型的对象，并且可以利用Python的内嵌函数对列表进行各种操作。

- 循环写法没有太多好说的，简单好用，i不用提前定义，可以根据in后面的变量来自动对i进行赋值。要产生一个范围可以使用range函数，注意它是左闭右开的。

- 函数声明，循环声明，条件判断声明都用冒号作为结束。

- 格式化输出类似C语言，注意有多个变量时使用圆括号()括起，这样就当为一个元组。单变量格式化输出无须括号。在格式化描述和变量间用百分号%隔开。

- 逻辑运算符直接使用小写英文的and，or，not即可，`&&`,`||`,`~`这些就不使用了。

- 条件判断时我们常用else if，但是在Python中这个关键字是elif。

- 注释有多重写法，可以单行也可以多行，具体参照[My_Python_Notebook](https://github.com/familyld/learnpython/blob/master/My_Python_Notebook.md)的`注释`章节。

- 注意图中使用的是Python2.x版本，所以print是作为一个语句而不是函数，不需要使用括号括起后面的字符串。在Python3.x版本中print是作为一个函数被调用的，注意区别。详细的差别可以看这个项目下的[Difference_between_Py2_and_Py3](https://github.com/familyld/learnpython/blob/master/Difference_between_Py2_and_Py3.md)一文。
