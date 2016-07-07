# Python编程规范-PEP8

良好的代码风格是非常有必要的，不仅能让自己在review代码时思路更加清晰，免去重复造轮子的麻烦，在多人合作开发项目的情况下，也能避免歧义，使得大家可以更好地专注于算法本身。

在使用Python语言编写代码也需要遵循一定的规范，在这篇笔记中，我对Python官方给出的编程规范-PEP8进行了翻译，并加入一些自己的见解。原文来自[PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)，作者是Guido van Rossum、Barry Warsaw和Nick Coghlan。在翻译的过程中，我参考了Github上Damnever的中译文版本[PEP8-Style-Guide-for-Python-Code](https://github.com/Damnever/Note/blob/master/note/PEP8-Style-Guide-for-Python-Code.md)，但该译本存在一些翻译错误，并且缺失了PEP8最新版本中的部分内容。

欢迎转载并进一步完善这份译稿，请保留[作者信息](https://github.com/familyld)以及文中所有的引用。

## 目录
<!-- MarkdownTOC -->

- [介绍](#介绍)
- [一昧地保持一致是愚蠢的](#一昧地保持一致是愚蠢的)
- [代码排版](#代码排版)
    - [缩进](#缩进)
    - [使用Tab还是空格？](#使用tab还是空格？)
    - [每行最多字符数](#每行最多字符数)
    - [换行应在二元运算符前还是后？](#换行应在二元运算符前还是后？)
    - [空行](#空行)
    - [源文件编码](#源文件编码)
    - [导入](#导入)
    - [模块级的名称](#模块级的名称)
- [字符串的引号](#字符串的引号)
- [表达式和语句中的空格](#表达式和语句中的空格)
    - [不可容忍的写法](#不可容忍的写法)
    - [其他建议](#其他建议)
- [注释](#注释)
    - [块注释](#块注释)
    - [行内注释](#行内注释)
    - [文档字符串](#文档字符串)
- [命名约定](#命名约定)
    - [覆盖原则](#覆盖原则)
    - [描述：命名风格](#描述：命名风格)
    - [规范：命名约定](#规范：命名约定)
        - [应当避免的命名](#应当避免的命名)
        - [包名和模块名](#包名和模块名)
        - [类名](#类名)
        - [异常名](#异常名)
        - [全局变量名](#全局变量名)
        - [函数名](#函数名)
        - [函数和方法的参数](#函数和方法的参数)
        - [方法名和实例变量](#方法名和实例变量)
        - [常量](#常量)
        - [继承的设计](#继承的设计)
    - [公共接口和内部接口](#公共接口和内部接口)
- [编程建议](#编程建议)
    - [函数注解](#函数注解)
- [参考文献](#参考文献)
- [版权](#版权)

<!-- /MarkdownTOC -->


## 介绍
**Introduction**

This document gives coding conventions for the Python code comprising the standard library in the main Python distribution. Please see the companion informational PEP describing style guidelines for the C code in the C implementation of Python [[1]](https://www.python.org/dev/peps/pep-0008/#id8).<br>
> 这份文档给出的代码规范适用于Python主要发行版中所有标准库的Python代码。 你可以参阅相关的PEP（全称：Python Enhancement Proposal，Python改进方案）信息，它用于描述Python的C代码规范。

This document and [PEP 257](https://www.python.org/dev/peps/pep-0257) (Docstring Conventions) were adapted from Guido's original Python Style Guide essay, with some additions from Barry's style guide [[2]](https://www.python.org/dev/peps/pep-0008/#id9).<br>
> 这篇文档和 PEP 257（文档字符串约定）都改编自Guido的Python风格指南一文，并从Barry的风格指南中引入了部分内容。

This style guide evolves over time as additional conventions are
identified and past conventions are rendered obsolete by changes in
the language itself.<br>
> 随着时间推移，这份风格指南不断发展。并且，作为一份额外的约定，已被认可。旧的约定则随着语言本身的发展被淘汰了。

Many projects have their own coding style guidelines. In the event of any
conflicts, such project-specific guides take precedence for that project.<br>
> 很多项目都有自己的代码风格指南，在这种情况下，如果（与PEP8）存在冲突，应优先考虑项目指定的代码风格。


## 一昧地保持一致是愚蠢的
**A Foolish Consistency is the Hobgoblin of Little Minds**

One of Guido's key insights is that code is read much more often than it is written.  The guidelines provided here are intended to improve the readability of code and make it consistent across the wide spectrum of Python code. As [PEP 20](https://www.python.org/dev/peps/pep-0020) says, "Readability counts".<br>
> Guido的一个重要观点就是，我们需要读代码的次数远多于需要写代码的次数。 因此，给出这份指南旨在提高代码的可读性并使大多数Python代码能保持一致（的风格）。正如PEP20中说到的那样，可读性是非常重要的。

A style guide is about consistency.  Consistency with this style guide is important.  Consistency within a project is more important. Consistency within one module or function is the most important.<br>
> 一致性对于一份风格指南来说是很重要的，对于一个项目来说就更重要了，对于一个模块或函数来说更甚。

However, know when to be inconsistent -- sometimes style guide recommendations just aren't applicable. When in doubt, use your best judgment. Look at other examples and decide what looks best. And don't hesitate to ask!<br>
> 然而，我们必须知道什么时候需要“不一致” -- 有些情况下，风格指南给出的写法并不适用。 遇到这些情况时，就需要运用你的判断力来解决。对比其他例子来判断怎样写最好。千万不要介意（向同事/负责人）提问题。

In particular: do not break backwards compatibility just to comply with
this PEP!<br>
> 特别地，编写代码时不应该为了遵循这份指南而破环代码的[向后兼容性](https://zh.wikipedia.org/wiki/%E5%90%91%E4%B8%8B%E5%85%BC%E5%AE%B9)（也称“向下兼容性，这里可以理解为较高版本的程序依然能跑使用较低版本的程序编写的代码”）。

Some other good reasons to ignore a particular guideline:<br>
> 这里给出一些忽略某份风格指南的典型理由：

1. When applying the guideline would make the code less readable, even for someone who is used to reading code that follows this PEP.<br>
> 应用该指南使得代码的可读性变差，就连习惯按照该指南阅读代码的人都觉得难以阅读。

2. To be consistent with surrounding code that also breaks it (maybe for historic reasons) -- although this is also an opportunity to clean up someone else's mess (in true XP style).<br>
> 为了和其他不遵循该指南的代码（可能是历史遗留的原因）保持一致 -- 尽管这会是一个整理代码的好机会。事实上，Windows XP，或者说微软的OS，就特别强调维持软件的向下兼容性。即后续的系统更新不会使得在旧版本中开发的软件变得无法使用。为了实现这个目标，有时微软甚至不惜支持使用非官方乃至误用的 API 的软件[[Windows API#历史]](https://zh.wikipedia.org/wiki/Windows_API#.E6.AD.B7.E5.8F.B2)。

3. Because the code in question predates the introduction of the guideline and there is no other reason to be modifying that code.<br>
> 问题代码的历史比较久远（早于引入该指南的时间），而且没有其他理由要修改该代码。

4. When the code needs to remain compatible with older versions of Python that don't support the feature recommended by the style guide.<br>
> 当代码需要与旧版本的 Python 保持兼容，而旧版 Python 又不支持该指南中推荐的特性的时候。


## 代码排版
**Code lay-out**


### 缩进
**Indentation**

Use 4 spaces per indentation level.<br>
> 每层缩进采用4个空格。

Continuation lines should align wrapped elements either vertically using Python's implicit line joining inside parentheses, brackets and braces, or using a *hanging indent* [[7]](https://www.python.org/dev/peps/pep-0008/#fn-hi).  When using a hanging indent the following should be considered; there should be no arguments on the first line and further indentation should be used to clearly distinguish itself as a continuation line.<br>
> 续行时，对于包裹在括号中的元素有两种对齐的方案。要么利用Python的隐式行连接（可以参考[Python风格规范](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)和[Python语言参考手册](http://manual.linuxnote.org/python/implicit-joining.html)中的说明）特性进行垂直对齐（注：这一特性使得编写Python代码时，括号内的续行不需使用反斜线标明，并且缩进不影响代码逻辑。这样我们可以采用任意数量的缩进来实现续行之中各行的对齐）；要么就用悬挂缩进。注意使用悬挂缩进时，第一行不能有参数，并且要使用一个额外的缩进来表明这一行属于续行（从而和其他代码块区分开来）。

正确示例：

```python
# 同开始分界符(左括号)对齐
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 续行多缩进一级以跟其他代码区别开来
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# 悬挂缩进需要多缩进一级
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

错误示例：

```python
# 采用垂直对齐时第一行不应该有参数
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# 续行没有和（其他代码块）区分开，应再缩进一级
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

The 4-space rule is optional for continuation lines.<br>
> 续行中的缩进可以不必是4个空格

可选的：

```python
# 悬挂缩进可以不采用4空格缩进
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

When the conditional part of an ``if``-statement is long enough to require that it be written across multiple lines, it's worth noting that the combination of a two character keyword (i.e. ``if``), plus a single space, plus an opening parenthesis creates a natural 4-space indent for the subsequent lines of the multiline conditional. This can produce a visual conflict with the indented suite of code nested inside the ``if``-statement, which would also naturally be indented to 4 spaces.  This PEP takes no explicit position on how (or whether) to further visually distinguish such
conditional lines from the nested suite inside the ``if``-statement. Acceptable options in this situation include, but are not limited to:<br>
> 当if语句的条件部分足够长，需要分开多个行来写的时候，有一点值得注意的是，对于两字符的关键词（比方说if），可以加入一个空格和左括号来创造自然的4空格缩进，使得if语句条件部分中的各行可以按照这个缩进进行对齐。 但是，这样会和嵌套在if语句中的代码块构成视觉冲突，因为这个代码块同样采用了4空格缩进。 对于如何把if语句的条件部分和嵌套在if语句中的代码块在视觉上区分开来，这份指南并没有给出明确规定。可接受但不限于以下的写法：

```python
# 不采用额外缩进
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# 增加一行注释，这样在编辑器中显示时能有所区分
# supporting syntax highlighting.
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# 在条件语句的续行增加一级缩进
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

(Also see the discussion of whether to break before or after binary operators below.)<br>
> （这里还涉及到了应该在二元运算符前进行换行还是在其后进行换行的问题，可以参看本文的后续内容。）

The closing brace/bracket/parenthesis on multi-line constructs may
either line up under the first non-whitespace character of the last
line of list, as in:<br>
> 多行结构中的右括号应放在最后一行第一个非空白字符的下方，如下所示：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```

or it may be lined up under the first character of the line that
starts the multi-line construct, as in:
<br>
> 也可以放在多行结构中第一行第一个字符的下方，如下所示：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

### 使用Tab还是空格？
**Tabs or Spaces?**

Spaces are the preferred indentation method.<br>
> 采用空格是缩进的首选。

Tabs should be used solely to remain consistent with code that is already indented with tabs.<br>
> 仅当我们需要和以往采用Tab进行缩进的代码保持一致性时使用Tab。

Python 3 disallows mixing the use of tabs and spaces for indentation.<br>
> Python 3 不允许同时采用Tab和空格进行缩进。

Python 2 code indented with a mixture of tabs and spaces should be converted to using spaces exclusively.<br>
> Python 2 中混合使用Tab和空格进行缩进的代码应该都转换为只使用空格。

When invoking the Python 2 command line interpreter with the ``-t`` option, it issues warnings about code that illegally mixes tabs and spaces.  When using ``-tt`` these warnings become errors. These options are highly recommended!<br>
> 当使用``-t``选项启动Python 2命令行解释器时，混合使用Tab和空格进行缩进的代码会被警告。当使用``-tt``选项启动Python 2命令行解释器时，这些警告会变成错误。强烈推荐使用这些选项！


### 每行最多字符数
**Maximum Line Length**

Limit all lines to a maximum of 79 characters.<br>
> 每行最多包含79个字符。

For flowing long blocks of text with fewer structural restrictions (docstrings or comments), the line length should be limited to 72 characters.<br>
> 对于连续的大段文本块（比方说文档字符串或注释），结构上的限制较少，这些文本块每行应限制在72个字符长度内。

Limiting the required editor window width makes it possible to have several files open side-by-side, and works well when using code review tools that present the two versions in adjacent columns.<br>
> 限制编辑器窗口宽度使得同时打开几个文件并在屏幕上并排显示成为了可能（这里可以理解为并排显示时依然能够完整地看到每一行，而无需借助横向滚动条）。当使用代码评审工具把两个版本的代码在（软件面板的）相邻列显示时，效果也很好。

The default wrapping in most tools disrupts the visual structure of the code, making it more difficult to understand. The limits are chosen to avoid wrapping in editors with the window width set to 80, even if the tool places a marker glyph in the final column when wrapping lines. Some web based tools may not offer dynamic line wrapping at all.<br>
> 很多工具提供的自动换行功能会破坏代码的视觉结构，令代码变得更难以理解。即使在一个窗口宽度设置为80个字符的编辑器中，换行时编辑器会在每行末尾放置记号，为了避免自动换行，我们还是需要限制每行最多允许的字符数。一些基于web的工具可能根本就没有提供自动换行的功能了（这种情况下，人为地限制每行的字符数就更加关键了）。

Some teams strongly prefer a longer line length. For code maintained exclusively or primarily by a team that can reach agreement on this issue, it is okay to increase the nominal line length from 80 to 100 characters (effectively increasing the maximum length to 99 characters), provided that comments and docstrings are still wrapped at 72 characters.<br>
> 一些团队可能对更长的行长度（每行显示更多字符）有强烈的需求。 对于仅由或主要由一个团队进行维护的代码，把行长度增加至80到100个字符（实际上最大行长是99字符）是允许的，但文档字符串和注释依然要求按72字符的行长度进行换行。

The Python standard library is conservative and requires limiting lines to 79 characters (and docstrings/comments to 72).<br>
> Python标准库的代码遵循常规，要求把行长度限制在79个字符内（文档字符串/注释为72个字符）。

The preferred way of wrapping long lines is by using Python's implied line continuation inside parentheses, brackets and braces. Long lines can be broken over multiple lines by wrapping expressions in parentheses. These should be used in preference to using a backslash for line continuation.<br>
> 一种推荐的换行方式是利用Python的隐式行连接特性（前文已经提及到），括号中的表达式可以被分为多行。这种方式应当优先考虑而非采用反斜杠进行换行。

Backslashes may still be appropriate at times. For example, long, multiple with -statements cannot use implicit continuation, so backslashes are acceptable:<br>
> 但反斜杠也有适用的情景。比方说，较长的多个with语句就无法采用隐式行连接的方式换行，但是可以采用反斜杠：

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

(See the previous discussion on [multiline if-statements](https://www.python.org/dev/peps/pep-0008/#multiline-if-statements) for further
thoughts on the indentation of such multiline ``with``-statements.)<br>
> 可以参照（参照前面关于多行if语句的讨论来进一步考虑这里with语句的缩进。）

Another such case is with ``assert`` statements.<br>
> 另一个典型的例子是assert语句。

Make sure to indent the continued line appropriately.<br>
> 要确保续行有适当的缩进。

### 换行应在二元运算符前还是后？
**Should a line break before or after a binary operator?**

For decades the recommended style was to break after binary operators. But this can hurt readability in two ways: the operators tend to get scattered across different columns on the screen, and each operator is moved away from its operand and onto the previous line. Here, the eye has to do extra work to tell which items are added and which are subtracted:<br>
> 数十年来，推荐的代码风格都是在二元运算符之后进行换行，但这样做会影响代码的可读性。理由有两个：一是，运算符被分散到了屏幕上的不同列（这里的意思是，因为每行代码长度不同，所以处于不同行行末的运算符会处在不同列）；二是，运算符和运算对象被分隔开，运算符位于运算对象的上一行。 这二者都会对阅读代码构成视觉障碍，使得我们的眼睛需要做一些额外的工作。

```python
# 错误示例： operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)
```

To solve this readability problem, mathematicians and their publishers follow the opposite convention. Donald Knuth explains the traditional rule in his Computers and Typesetting series: "Although formulas within a paragraph always break after binary operations and relations, displayed formulas always break before binary operations" [[3]](https://www.python.org/dev/peps/pep-0008/#id10) .<br>
> 为了解决可读性问题，数学家和他们的出版商遵循相反的约定。Donald Knuth曾解释过：“尽管段内部的公式总是在运算符后换行，表达式总是在运算符前换行的。”（这两者的区别也可以理解为是处于文本段内还是独占一行）

Following the tradition from mathematics usually results in more readable code:<br>
> 遵循数学传统来编写往往能令代码的可读性更加：

```python
# 正确示例： easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

In Python code, it is permissible to break before or after a binary operator, as long as the convention is consistent locally. For new code Knuth's style is suggested.<br>
> 在Python代码中，在二院运算符前或者后换行都是被允许的，只要在这个局部中保持一致即可。但对于新编写的代码来说，更推荐采用Knuth的风格（在运算符前进行断行）。

### 空行
**Blank Lines**

Surround top-level function and class definitions with two blank lines.<br>
> 使用2个空行来分隔最高级的函数(可以理解为处于类外部，且不处于任意函数内部的函数)和类定义。

Method definitions inside a class are surrounded by a single blank line.<br>
> 使用1个空行来分隔类中的方法定义。

Extra blank lines may be used (sparingly) to separate groups of related functions.  Blank lines may be omitted between a bunch of related one-liners (e.g. a set of dummy implementations).<br>
> 可以使用额外的空行来分隔一组相关的函数，但不要滥用。在一系列相关的单行代码之间，空行是可以被省略的，比如一组哑元实现（或者说是虚拟实现，一般指没有实际意义，只是为了版本兼容而存在的代码）。

Use blank lines in functions, sparingly, to indicate logical sections.<br>
> 可以在函数内使用空行使代码逻辑更清晰，但不要滥用。

Python accepts the control-L (i.e. ^L) form feed character as whitespace; Many tools treat these characters as page separators, so you may use them to separate pages of related sections of your file. Note, some editors and web-based code viewers may not recognize control-L as a form feed and will show another glyph in its place.<br>
> Python支持control-L（如:^L）换页符作为空格；许多工具将这些符号作为分页符，因此你可以使用这些符号来进行分页或者划分文件中的相关区域。注意，一些编辑器和基于web的代码预览器可能不会将control-L识别为分页符，而是把它显示为其他符号。


### 源文件编码
**Source File Encoding**

Code in the core Python distribution should always use UTF-8 (or ASCII
in Python 2).<br>
> Python核心发行版中的代码应该一直使用UTF-8（Python 2中则使用ASCII）。

Files using ASCII (in Python 2) or UTF-8 (in Python 3) should not have
an encoding declaration.<br>
> 使用ASCII（在Python 2中）或者UTF-8（在Python 3中）的文件不应该添加编码声明。

In the standard library, non-default encodings should be used only for test purposes or when a comment or docstring needs to mention an author name that contains non-ASCII characters; otherwise, using ``\x``, ``\u``, ``\U``, or ``\N`` escapes is the preferred way to include non-ASCII data in string literals.<br>
> 在标准库中，只有用作测试目的，或者注释或文档字符串需要提及作者名字而不得不使用非ASCII字符时，才能使用非默认的编码。否则，在字符串文字中包括非ASCII数据时，推荐使用\x, \u, \U或\N等转义符来表示。

For Python 3.0 and beyond, the following policy is prescribed for the standard library (see [PEP 3131](https://www.python.org/dev/peps/pep-3131)): All identifiers in the Python standard library MUST use ASCII-only identifiers, and SHOULD use English words wherever feasible (in many cases, abbreviations and technical terms are used which aren't English). In addition, string literals and comments must also be in ASCII. The only exceptions are：<br>
> 对于Python 3.0及其以后的版本，标准库需遵循以下原则（参见PEP 3131）：Python标准库中的所有标识符都必须只采用ASCII编码的标识符，在可行的条件下也应当使用英文词（很多情况下，使用的缩写和技术术语词都不是英文）。此外，字符串文字和注释也应该只包括ASCII编码。只有两种情况例外：

- (a) test cases testing the non-ASCII features<br>
> 测试情况下为了测试非ASCII编码的特性

- (b) names of authors. Authors whose names are not based on the latin alphabet MUST provide a latin transliteration of their names.<br>
> 作者名字。但即使作者名字不是由拉丁字母组成，也必须为其提供一个拉丁音译名。

Open source projects with a global audience are encouraged to adopt a similar policy.<br>
> 鼓励面向全世界的开源项目都采用类似的原则。


### 导入
**Imports**

- Imports should usually be on separate lines, e.g.:<br>
> 导入应该分开多行写，而不是都写在一行，例如：

```python
# 正确示例：
import os
import sys

# 错误示例：
import sys, os
```

It's okay to say this though:<br>
> 这样写也是可以的：

```python
from subprocess import Popen, PIPE
```

- Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.<br>
> 导入应该写在代码文件的开头，位于模块注释和文档字符串之后，模块全局变量和常量声明之前。

Imports should be grouped in the following order:<br>
> 导入应该按照下面的顺序分组来写：

1. standard library imports<br>
> 标准库导入
2. related third party imports<br>
> 相关第三方导入
3. local application/library specific imports<br>
> 本地应用/库的特定导入

You should put a blank line between each group of imports.<br>
> 不同组的导入之间用空行隔开。

Put any relevant ``__all__`` specification after the imports.<br>
> 将任何相关的__all__说明放在导入之后。

- Absolute imports are recommended, as they are usually more readable and tend to be better behaved (or at least give better error messages) if the import system is incorrectly configured (such as when a directory inside a package ends up on ``sys.path``):<br>
> 推荐使用绝对导入，因为这样通常更易读，而且在导入系统配置不当（比如包中的路径以sys.path结束）的情况下，也会有更好的表现（或者至少会给出更准确的错误信息）：

```python
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```

However, explicit relative imports are an acceptable alternative to absolute imports, especially when dealing with complex package layouts where using absolute imports would be unnecessarily verbose:<br>
> 然而，除了绝对导入，显式的相对导入也是一种可以接受的替代方式。特别是在处理复杂的包布局时，采用绝对导入会显得啰嗦。

```python
from . import sibling
from .sibling import example
```

Standard library code should avoid complex package layouts and always use absolute imports.<br>
> 标准库代码应当避免复杂的包布局并一致地采用绝对导入。

Implicit relative imports should *never* be used and have been removed in Python 3.<br>
> 隐式的相对导入应该永不使用，并且在Python 3中这种方式已经被去掉了。

- When importing a class from a class-containing module, it's usually okay to spell this:<br>
> 当从一个包括类的模块中导入一个类时，通常可以这样写：

```python
from myclass import MyClass
from foo.bar.yourclass import YourClass
```

If this spelling causes local name clashes, then spell them :<br>
> 如果和本地命名的拼写产生了冲突，应当直接导入模块：

```python
import myclass
import foo.bar.yourclass
```

and use "myclass.MyClass" and "foo.bar.yourclass.YourClass".<br>
> 然后使用”myclass.MyClass”和”foo.bar.yourclass.YourClass”.


- Wildcard imports (`from < module > import *`) should be avoided, as they make it unclear which names are present in the namespace, confusing both readers and many automated tools. There is one defensible use case for a wildcard import, which is to republish an internal interface as part of a public API (for example, overwriting a pure Python implementation of an interface with the definitions from an optional accelerator module and exactly which definitions will be overwritten isn't known in advance).<br>
> 避免使用通配符进行导入，因为这样会造成在当前命名空间出现的命名含义不清晰，给读者和许多自动化工具造成困扰。有一个适合使用通配符进行导入的情形，即将一个内部接口重新发布成公共API的一部分（比如，使用可选的加速模块中的定义去覆盖纯Python实现的接口，这种情况下，哪些定义会被覆盖是不被提前知晓的）。

When republishing names this way, the guidelines below regarding public and internal interfaces still apply.<br>
> 当使用这种方式重新发布命名时，指南后面关于公共和内部接口的部分仍然适用。

### 模块级的名称
**Module level dunder names**

Module level "dunders" (i.e. names with two leading and two trailing underscores) such as `__all__` , `__author__` , `__version__` , etc. should be placed after the module docstring but before any import statements except `from __future__ imports`. Python mandates that future-imports must appear in the module before any other code except docstrings.<br>
> 模块级的"dunders"(指double underline，在名字前后都有两个下划线)的命名应该放在模块的文档字符串之后，导入语句（除`from __future__ imports`之外）之前。Python要求future-imports必须出现在模块除文档字符串外的任何代码之前。

For example:<br>
> 比方说：

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

## 字符串的引号
**String Quotes**

In Python, single-quoted strings and double-quoted strings are the same. This PEP does not make a recommendation for this. Pick a rule and stick to it.  When a string contains single or double quote characters, however, use the other one to avoid backslashes in the string. It improves readability.<br>
> 在 Python 里面，单引号字符串和双引号字符串是等同的。这份指南没有给出（关于使用哪种引号的）建议。选择一种方式并一致地使用就可以了。当一个字符串包含单引号或者双引号字符时，用（两者中的）另外一种来包裹字符串，而不是使用反斜杠对字符串中的引号进行转义，这样可以提高代码的可读性。

For triple-quoted strings, always use double quote characters to be consistent with the docstring convention in [PEP 257](https://www.python.org/dev/peps/pep-0257).<br>
> 对于三引号字符串，总是使用双引号字符，以跟PEP 257中对文档字符串的约定保持一致（这段话的意思是，三引号字符串应一致地使用`"""something"""`的方式，而不是`'''something'''`）。


## 表达式和语句中的空格
**Whitespace in Expressions and Statements**

### 不可容忍的写法
**Pet Peeves**

Avoid extraneous whitespace in the following situations:<br>
> 下列情况中应避免使用多余的空格：

- Immediately inside parentheses, brackets or braces. :<br>
> 紧靠着括号的内部：

```python
# 正确示例
spam(ham[1], {eggs: 2})
# 错误示例
spam( ham[ 1 ], { eggs: 2 } )
```

- Immediately before a comma, semicolon, or colon:<br>
> 在逗号，分号，引号前面：

```python
# 正确示例
if x == 4: print x, y; x, y = y, x
# 错误示例
if x == 4 : print x , y ; x , y = y , x
```

- However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority).  In an extended slice, both colons must have the same amount of spacing applied.  Exception: when a slice parameter is omitted, the space is omitted.<br>
> 但是，在切片中，冒号扮演的角色类似于二元运算符，因此两侧（的语句）应被同等对待（这里可以把冒号看作一种最低优先级的运算）。在切片中，冒号的左右两侧空格数应该相等。特别地，当切片参数省略时，该侧的空格也被省略。

正确示例：
```python
ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset : upper+offset]
ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
ham[lower + offset : upper + offset]
```

错误示例：
```python
ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

- Immediately before the open parenthesis that starts the argument list of a function call:<br>
> 调用函数时参数列表的左括号前：

```python
# 正确示例：
spam(1)
# 错误示例：
spam (1)
```

- Immediately before the open parenthesis that starts an indexing or slicing:<br>
> 索引或切片的左括号前：

```python
# 正确示例：
dct['key'] = lst[index]
# 错误示例：
dct ['key'] = lst [index]
```
- More than one space around an assignment (or other) operator to align it with another.<br>
> 运算符（比方说赋值运算符等）的两侧不应多于一个空格。

正确示例：

```python
x = 1
y = 2
long_variable = 3
```

错误示例：

```python
x             = 1
y             = 2
long_variable = 3
```

### 其他建议
**Other Recommendations**

- Avoid trailing whitespace anywhere.  Because it's usually invisible, it can be confusing: e.g. a backslash followed by a space and a newline does not count as a line continuation marker.  Some editors don't preserve it and many projects (like CPython itself) have pre-commit hooks that reject it.<br>
> 要避免在结尾处使用空格。因为这种情况下，空格通常是不可见的，而且容易产生歧义：比方说在反斜杠后使用了空格，这样新的一行将不被视为续行。 有些编辑器会把结尾处的空格去除掉，而且很多项目（比方说CPython）的预提交钩子不接受这种写法。

- Always surround these binary operators with a single space on either side: assignment (``=``), augmented assignment (``+=``, ``-=`` etc.), comparisons (``==``, ``<``, ``>``, ``!=``, ``<>``, ``<=``, ``>=``, ``in``, ``not in``, ``is``, ``is not``), Booleans (``and``, ``or``, ``not``).<br>
> 总是在这些二元操作符的两侧加入一个空格：赋值(=)，增量赋值(+=, -= etc.)，比较(==, <, >, !=, <>, <=, >=, in, not in, is, is not)，布尔运算(and, or, not)。

- If operators with different priorities are used, consider adding whitespace around the operators with the lowest priority(ies). Use your own judgment; however, never use more than one space, and always have the same amount of whitespace on both sides of a binary operator.<br>
> 在不同优先级之间，考虑在更低优先级的操作符两侧插入空格。用你自己的判断力；但不要使用超过一个空格，并且在二元操作符的两侧有相同的空格数。

正确示例：

```python
i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

错误示例：

```python
i=i+1
submitted +=1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)
```

- Don't use spaces around the ``=`` sign when used to indicate a keyword argument or a default parameter value.<br>
> 在指示关键字参数或默认参数的值时，等号两侧不要使用空格。

正确示例：

```python
def complex(real, imag=0.0):
    return magic(r=real, i=imag)
```

错误示例：

```python
def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
```

- Function annotations should use the normal rules for colons and always have spaces around the ``->`` arrow if present. (See [Function Annotations](https://www.python.org/dev/peps/pep-0008/#function-annotations) below for more about function annotations.)<br>
> 函数注解（可参照cnblog上[python开发_function annotations](http://www.cnblogs.com/hongten/p/hongten_python_function_annotation.html)一文，是Python3中一种为函数添加注释的特殊写法，这种写法中参数注释用冒号指示，返回值注释用右箭头指示）的冒号仅右侧使用一个空格，而右箭头则两侧都应有一个空格。

正确示例：

```python
def munge(input: 'AnyStr'):
    # TODO:
def munge() -> 'PosInt':
    # TODO:
```

错误示例：

```python
def munge(input:'AnyStr'):
    # TODO:
def munge()->'PosInt':
    # TODO:
```
> 解析一下，上面实例中第一个def的input这个参数的注释是AnyStr，而第二个def中右箭头所指的是返回值的注释，这里PosInt指该函数的返回值是一个正整数。特别地，除了用字符串作注解，也可以用类型作为注解，比方说`def munge(input: str)`。如果想查看一个函数的注解，可以使用`print('Annotations:', munge.__annotations__)`语句。效果：

```
>>> def munge(in1: 'AnyStr', in2: int) -> 'None':
...     print("Annotations:", munge.__annotations__)
...     print("Arguments:", in1, in2)
...
>>> munge('Hello',123)
Annotations: {'in1': 'AnyStr', 'return': 'None', 'in2': <class 'int'>}
Arguments: Hello 123
```
- When combining an argument annotation with a default value, use spaces around the ``=`` sign (but only for those arguments that have both an annotation and a default).<br>
> 当且仅当需要为默认参数添加函数注解时，赋值的等号两侧各添加一个空格。

正确示例：

```python
def munge(sep: AnyStr = None): ...
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
```

错误示例：

```python
def munge(input: AnyStr=None): ...
def munge(input: AnyStr, limit = 1000): ...
```

- Compound statements (multiple statements on the same line) are generally discouraged.<br>
> 不推荐使用符合语句（在一行中编写多个表达式）。

正确示例：

```python
if foo == 'blah':
    do_blah_thing()
do_one()
do_two()
do_three()
```

尽量避免:

```python
if foo == 'blah': do_blah_thing()
do_one(); do_two(); do_three()
```

- While sometimes it's okay to put an if/for/while with a small body on the same line, never do this for multi-clause statements.  Also avoid folding such long lines!<br>
> 有时候如果if/for/while的主体很短（且只有一句），可以选择放在同一行。但是在主体是多条语句的情况下就不应该这样写。同时也要注意，多条短表达式不要折叠起来（放在同一行）写。

尽量避免:

```python
if foo == 'blah': do_blah_thing()
for x in lst: total += x
while t < 10: t = delay()
```

坚决不要:

```python
if foo == 'blah': do_blah_thing()
else: do_non_blah_thing()

try: something()
finally: cleanup()

do_one(); do_two(); do_three(long, argument,
                             list, like, this)

if foo == 'blah': one(); two(); three()
```

## 注释
**Comments**

Comments that contradict the code are worse than no comments.  Always
make a priority of keeping the comments up-to-date when the code
changes!<br>
> 写与代码冲突的注释还不如不写。谨记！在更新代码时也要注意对注释做出相应的更新。

Comments should be complete sentences.  If a comment is a phrase or sentence, its first word should be capitalized, unless it is an identifier that begins with a lower case letter (never alter the case of identifiers!).<br>
> 注释应当是个完整的句子。如果注释是短语或者句子，则第一个单词的首字母应当大写。有一种情况例外，就是开头是小写的标识符（比方说变量名），永远不要改变标识符的大小写（否则注释和代码就对不上了）。

If a comment is short, the period at the end can be omitted.  Block comments generally consist of one or more paragraphs built out of complete sentences, and each sentence should end in a period.<br>
> 如果注释很短，末尾的句号可以省略。而块注释通常由一个或多个包含完整句子的段落组成，这里面的每个句子都应该使用句号作为结束。

You should use two spaces after a sentence-ending period.<br>
> 在句子结束的句号后面应该使用两个空格。

When writing English, follow Strunk and White.<br>
> 在写英文注释时，应当遵循《Strunk and White》（注：这是一本英文写作小册子，也叫作《英文写作指南》，详细介绍可参考[维基百科](https://zh.wikipedia.org/wiki/%E8%8B%B1%E6%96%87%E5%86%99%E4%BD%9C%E6%8C%87%E5%8D%97)）。

Python coders from non-English speaking countries: please write your
comments in English, unless you are 120% sure that the code will never
be read by people who don't speak your language.<br>
> 来自非英语国家的程序员也请使用英文来编写注释，除非你能120%地保证你的代码不会被读不懂你的语言的人阅读。

### 块注释
**Block Comments**

Block comments generally apply to some (or all) code that follows
them, and are indented to the same level as that code.  Each line of a
block comment starts with a ``#`` and a single space (unless it is
indented text inside the comment).<br>
> 块注释通常用于说明后续的代码，并且与它们拥有相同的缩进级别。块注释的每一行以\#号开始，并且跟随这一个空格（除非这一行是注释中本来就有着缩进的文本，此时会多于一个空格）。

Paragraphs inside a block comment are separated by a line containing a
single ``#``.<br>
> 块注释中的段落之间用一个空行隔开，这个空行只包含开头的一个\#号。

### 行内注释
**Inline Comments**

Use inline comments sparingly.<br>
> 尽量少使用行内注释。

An inline comment is a comment on the same line as a statement. Inline comments should be separated by at least two spaces from the statement. They should start with a # and a single space.<br>
> 行内注释和语句处于同一行中，与语句之间最少应使用两个空格进行分割，并且以\#号和一个空格作为开始。

Inline comments are unnecessary and in fact distracting if they state the obvious. Don't do this:<br>
> 如果一个行内注释的描述是显而易见的，则该注释毫无意义并且容易转移读者的注意力。 千万不要写这样的注释：

```python
x = x + 1                 # Increment x
```

But sometimes, this is useful:<br>
> 但有时，这样写是有效的：

```python
x = x + 1                 # Compensate for border
```

### 文档字符串
**Documentation Strings**

Conventions for writing good documentation strings (a.k.a. "docstrings") are immortalized in [PEP 257](https://www.python.org/dev/peps/pep-0257).<br>
> PEP 257给出了关于编写良好的文档字符串的约定。

- Write docstrings for all public modules, functions, classes, and methods. Docstrings are not necessary for non-public methods, but you should have a comment that describes what the method does.  This comment should appear after the ``def`` line.<br>
> 我们需要为所有公共的模块，函数，类，以及方法编写文档字符串。对于非公共的方法来说，没有编写文档字符串的必要，但依然要为这些方法编写注释，已表明它们的用途。 这些主食应放在方法声明（def所处的行）的下面。

- [PEP 257](https://www.python.org/dev/peps/pep-0257) describes good docstring conventions.  Note that most importantly, the ``"""`` that ends a multiline docstring should be on a line by itself, e.g.:<br>
> PEP 257给出了编写良好的文档字符串的约定。 注意最重要的一点是， 多行文档字符串结尾的三引号`"""`应单独成行。

```python
"""Return a foobang

Optional plotz says to frobnicate the bizbaz first.
"""
```

- For one liner docstrings, please keep the closing ``"""`` on the same line.<br>
> 对于只有一行的文档字符串来说，结尾的三引号`"""`应当放在同一行中。

## 命名约定
**Naming Conventions**

The naming conventions of Python's library are a bit of a mess, so we'll never get this completely consistent -- nevertheless, here are the currently recommended naming standards.  New modules and packages (including third party frameworks) should be written to these standards, but where an existing library has a different style, internal consistency is preferred.<br>
> Python库的命名约定比较混乱，因此我们很难保持完全一致的 -- 但是，这里给出一些推荐的命名标准。新的模块和包（包括第三方框架）应当按照这些标准来命名。而对于已存在的风格不同的库，保持内部一致性才是最优先考虑的。

### 覆盖原则
**Overriding Principle**

Names that are visible to the user as public parts of the API should follow conventions that reflect usage rather than implementation.<br>
> API里对用户可见的公共的命名应当注重反映用途而非实现方式。

### 描述：命名风格
**Descriptive: Naming Styles**

There are a lot of different naming styles.  It helps to be able to recognize what naming style is being used, independently from what they are used for.<br>
> 有很多种不同的命名风格。这将有助于我们独立于用途来辨认不同的命名风格。

The following naming styles are commonly distinguished:<br>
> 下面这些命名风格都是有区别的：

- ``b`` (single lowercase letter)<br>
> 单个小写字母

- ``B`` (single uppercase letter)<br>
> 单个大写字母

- ``lowercase``<br>
> 全小写的单词

- ``lower_case_with_underscores``<br>
> 全小写并用下划线连接的短语

- ``UPPERCASE``<br>
> 全大写的单词

- ``UPPER_CASE_WITH_UNDERSCORES``<br>
> 全大写并用下划线连接的短语

- ``CapitalizedWords`` (or CapWords, or CamelCase -- so named because of the bumpy look of its letters [[3]](https://www.python.org/dev/peps/pep-0008/#id11)).  This is also sometimes known as StudlyCaps. Note: When using abbreviations in CapWords, capitalize all the letters of the abbreviation.  Thus HTTPServerError is better than HttpServerError.<br>
> 单词间紧密连接，采用单词首字母大小的方式进行区分的写法又称CapWords，或CamelCase（驼峰命名法），因为这样看上去字母是凹凸不平的。有时也称作StudlyCaps。当CapWords中包含缩写词时，该单词的所有字母都应该用大写表示。所以HTTPServerError这个命名要比HttpServerError更好。

- ``mixedCase`` (differs from CapitalizedWords by initial lowercase character!)<br>
> 混合大小写，这种风格和CapWords是不同的，它的首字母是小写字母！

- ``Capitalized_Words_With_Underscores`` (ugly!)<br>
> 用下划线连接的CapWords（这种写法非常丑陋！不推荐使用！）

There's also the style of using a short unique prefix to group related names together.  This is not used much in Python, but it is mentioned for completeness.  For example, the ``os.stat()`` function returns a tuple whose items traditionally have names like ``st_mode``, ``st_size``, ``st_mtime`` and so on.  (This is done to emphasize the correspondence with the fields of the POSIX system call struct, which helps programmers familiar with that.)<br>
> 也有一种命名风格是采用一个短的唯一的前缀对相关的名字进行分组。 这种风格在Python中不常用，这里仅为了内容的完整性而提及。举个例子，os模块的stat函数返回的元组中就包含这种风格命名的名字（这样做是为了强调该命名与POSIX系统字段的一致性，有助于帮助程序员熟悉这一点）。如下：

```python
>>> import os
>>> os.stat('F:\\')
os.stat_result(st_mode=16895, st_ino=1407374883553285, st_dev=1791446800, st_nlink=1, st_uid=0,
               st_gid=0, st_size=49152, st_atime=1467099074, st_mtime=1467099074, st_ctime=1189206937
)
```

The X11 library uses a leading X for all its public functions.  In Python, this style is generally deemed unnecessary because attribute and method names are prefixed with an object, and function names are prefixed with a module name.<br>
> X11库的所有公共函数都用X打头。 但在Python中这种风格被认为是不重要的，因为属性和方法名都有一个对象名作为前缀，而函数名则有一个模块名作为前缀（这些信息已经足够表明所属）。

In addition, the following special forms using leading or trailing underscores are recognized (these can generally be combined with any case convention):<br>
> 此外，下面这些采用前导或尾随下划线的特殊格式可以和其他风格组合使用。

- ``_single_leading_underscore``: weak "internal use" indicator. E.g. ``from M import *`` does not import objects whose name starts with an underscore.<br>
> 单个前导下划线：表示仅供（模块）内部使用。比方说，使用通配符导入一个模块时就不会导入这种对象。

- ``single_trailing_underscore_``: used by convention to avoid conflicts with Python keyword, e.g. :<br>
> 单个尾随下划线：通常用于避免和Python关键字冲突，比如：

```python
Tkinter.Toplevel(master, class_='ClassName')
```

- ``__double_leading_underscore``: when naming a class attribute, invokes name mangling (inside class FooBar, ``__boo`` becomes ``_FooBar__boo``; see below).<br>
> 双前导下划线：用于命名类属性，调用时这个命名会被改变（在类FooBar中，类属性``__boo``会变成``_FooBar__boo``，详见下文）。

- ``__double_leading_and_trailing_underscore__``: "magic" objects or attributes that live in user-controlled namespaces. E.g. ``__init__``, ``__import__`` or ``__file__``.  Never invent such names; only use them as documented.<br>
> 双前导及双尾随下划线：魔术对象或属性，仅用于用户控制的命名空间内。比方说``__init__``，``__import__``，和``__file__``等等。 永远不要自己创造这样的命名。这些命名应仅用于文档化。

### 规范：命名约定
**Prescriptive: Naming Conventions**

#### 应当避免的命名
**Names to Avoid**

Never use the characters 'l' (lowercase letter el), 'O' (uppercase letter oh), or 'I' (uppercase letter eye) as single character variable names.<br>
> 永远不要单独用小写字母l，大写字母O，大写字母I作为变量名。

In some fonts, these characters are indistinguishable from the numerals one and zero.  When tempted to use 'l', use 'L' instead.<br>
>  在一些字体中，它们很难和数字的1和0区分开。但需要（单独）使用小写字母l时，可以尝试用大写的L代替。

#### 包名和模块名
**Package and Module Names**

Modules should have short, all-lowercase names.  Underscores can be used in the module name if it improves readability.  Python packages should also have short, all-lowercase names, although the use of
underscores is discouraged.<br>
> 模块名（也即文件名）应该使用短且全小写的名字。如果使用下划线可以提高可读性，则下划线也可以用。Python的包名同样应当使用短且全小写的名字，但包名中不推荐使用下划线。

When an extension module written in C or C++ has an accompanying Python module that provides a higher level (e.g. more object oriented) interface, the C/C++ module has a leading underscore (e.g. ``_socket``).<br>
> 当使用C或C++编写的扩展模块拥有一个伴随的用于提供更高层次（比方说更加面向对象）的接口时，C或C++编写的模块名应该有一个先导下划线（比方说``_socket``，单个先导下划线前面也解释过了，用于指示仅内部使用，这里由于采用了Python模块作为高层的公共接口，所以C或C++编写的扩展模块就变为对内部使用了）。

#### 类名
**Class Names**

Class names should normally use the CapWords convention.<br>
> 类名通常使用CapWords风格。

The naming convention for functions may be used instead in cases where the interface is documented and used primarily as a callable.<br>
> 如果接口需要文档化并且可以被调用，则有可能采用函数的命名约定进行命名。

Note that there is a separate convention for builtin names: most builtin names are single words (or two words run together), with the CapWords convention used only for exception names and builtin constants.<br>
> 注意，对于内置命名有单独的一个约定：绝大部分内置的命名都是单个单词（或者两个单词拼接在一起）。CapWords风格只用于异常名和内置常量。

#### 异常名
**Exception Names**

Because exceptions should be classes, the class naming convention applies here.  However, you should use the suffix "Error" on your exception names (if the exception actually is an error).<br>
> 由于异常一般都是通过类来定义的，所以异常名应用的是类名的规则。 注意，当异常确实属于错误时，需要在类名中添加后缀“Error”。

#### 全局变量名
**Global Variable Names**

(Let's hope that these variables are meant for use inside one module only.)  The conventions are about the same as those for functions.<br>
> （这里讨论的全局变量都仅用于模块内部），全局变量命名的约定和函数命名的约定是相同的。

Modules that are designed for use via ``from M import *`` should use the ``__all__`` mechanism to prevent exporting globals, or use the older convention of prefixing such globals with an underscore (which you might want to do to indicate these globals are "module
non-public").<br>
> 以``from M import *``的方式进行导入的模块应当使用``__all__``机制来防止导入全局变量。或者采用旧版的约定，给模块中的全局变量增加一个先导下划线（表示该变量是仅限于模块内部全局使用，是非公共的）。

#### 函数名
**Function Names**

Function names should be lowercase, with words separated by underscores as necessary to improve readability.<br>
> 函数名应该全小写，并且用下划线作为单词之间的分割以提高可读性。

mixedCase is allowed only in contexts where that's already the prevailing style (e.g. threading.py), to retain backwards compatibility.<br>
> 仅在混合大小写风格已经占主导（比方说threading.py）的环境中，使用混合大小写风格为函数命名，以保持向后兼容性。

#### 函数和方法的参数
**Function and method arguments**

Always use ``self`` for the first argument to instance methods.<br>
> 实例方法的第一个参数应当是self。

Always use ``cls`` for the first argument to class methods.<br>
> 类方法的第一个参数应当是cls。

If a function argument's name clashes with a reserved keyword, it is generally better to append a single trailing underscore rather than use an abbreviation or spelling corruption.  Thus ``class_`` is better than ``clss``.  (Perhaps better is to avoid such clashes by using a synonym.)<br>
> 如果函数的参数名和保留关键字冲突，则一般会在参数名后加一个下划线。而不是使用缩写或者移除单词的某些字母进行表示，比方说把class写成clss。采用同义词进行替换的效果可能更好。

> 注：关于实例方法，类方法，静态方法的区别不妨看看[python中类方法、类实例方法、静态方法的使用与区别](http://computer.c.blog.163.com/blog/static/10252448201167112751800/)一文。这里贴出代码简单解析一下：

```python
class A(object):
    def foo(self,x):
    #类实例方法
        print "executing foo(%s,%s)"%(self,x)

    @classmethod
    def class_foo(cls,x):
    #类方法
        print "executing class_foo(%s,%s)"%(cls,x)

    @staticmethod
    def static_foo(x):
    #静态方法
        print "executing static_foo(%s)"%x
```

> 调用情况：

```
a = A()
a.foo(1)             //print: executing foo(<__main__.A object at 0xb77d67ec>,1)

a.class_foo(1)       //print: executing class_foo(<class '__main__.A'>,1)
A.class_foo(1)       //print: executing class_foo(<class '__main__.A'>,1)

a.static_foo(1)      //print: executing static_foo(1)
A.static_foo(1)      //print: executing static_foo(1)
```

> 类方法使用@classmethod进行标记，静态方法使用@staticmethod进行标记，两种标记都没有的就是类实例方法。类实例方法仅能被实例对象调用，并且包含一个隐含调用参数self，指代调用该方法的实例本身；类方法和静态方法既能被实例对象调用，又能被类对象对用（Python中一切皆对象，所以类本身也是一个对象），它们的区别是：类方法包含一个隐含调用参数cls，指代类对象本身；静态方法无隐含调用参数。

#### 方法名和实例变量
**Method Names and Instance Variables**

Use the function naming rules: lowercase with words separated by underscores as necessary to improve readability.<br>
> 和函数命名约定一样，方法名和实例变量名都应该全小写，并且用下划线作为单词之间的分割以提高可读性。

Use one leading underscore only for non-public methods and instance variables.<br>
> 对于非公共的方法和实例变量，使用一个先导下划线标示。

To avoid name clashes with subclasses, use two leading underscores to invoke Python's name mangling rules.<br>
> 为了避免和子类（中的方法和变量名）冲突，可以使用两个先导下划线，这样在被调用时会触发Python的命名重整。

Python mangles these names with the class name: if class Foo has an attribute named ``__a``, it cannot be accessed by ``Foo.__a``.  (An insistent user could still gain access by calling ``Foo._Foo__a``.) Generally, double leading underscores should be used only to avoid name conflicts with attributes in classes designed to be subclassed.<br>
> Python中使用类名进行命名重整：如果Foo类有一个属性``__a``，则调用时不能采用``Foo.__a``的形式（当然，如果硬要访问该属性，可以采用``Foo._Foo__a``的方式获取访问权）。一般来说，双前导下划线仅用于防止基类和子类的属性产生命名冲突。

Note: there is some controversy about the use of __names (see below).<br>
> 注意：关于__names的使用仍然存在争议，详见下文。

#### 常量
**Constants**

Constants are usually defined on a module level and written in all capital letters with underscores separating words.  Examples include ``MAX_OVERFLOW`` and ``TOTAL``.<br>
> 常量通常是模块级定义的，采用同全大写字母表示，并且使用下划线来划分各个单词。

#### 继承的设计
**Designing for inheritance**

Always decide whether a class's methods and instance variables (collectively: "attributes") should be public or non-public.  If in doubt, choose non-public; it's easier to make it public later than to make a public attribute non-public.<br>
> 设计时应考虑类的方法和实例变量（这二者统称为类的属性）是否公开。如果不能确定，那就先按非公共的方式进行编写，因为从非公共改为公共要更简单一些。

Public attributes are those that you expect unrelated clients of your class to use, with your commitment to avoid backward incompatible changes.  Non-public attributes are those that are not intended to be used by third parties; you make no guarantees that non-public attributes won't change or even be removed.<br>
> 公共属性指的是你希望所有使用你的类的人都能使用的属性，并且你应当保证这些属性的向后兼容。非公共属性则是哪些你不希望第三方使用的属性，这些属性没有任何保证，（在新版本中）可能会被更改甚至移除。

We don't use the term "private" here, since no attribute is really private in Python (without a generally unnecessary amount of work).<br>
> 这里我们不使用“私有”这个术语，因为在Python中没有属性是真正私有的。

Another category of attributes are those that are part of the "subclass API" (often called "protected" in other languages).  Some classes are designed to be inherited from, either to extend or modify aspects of the class's behavior.  When designing such a class, take care to make explicit decisions about which attributes are public, which are part of the subclass API, and which are truly only to be used by your base class.<br>
> 除了公共和非公共属性之外，还有一类属性称为子类API，这类属性在基类中定义，供子类调用（在其它编程语言中一般称为“受保护的（属性）”）。有一些类设计出来是用于被继承的，（子类）可以在基类的基础上进行功能上的扩展和修改。 当设计这样的类时，注意要明确好哪些属性是公共的，哪些属性是子类API，以及哪些属性是仅限于在这个基类中使用的。

With this in mind, here are the Pythonic guidelines:<br>
> 理解上面的内容后，这里给出一些Python编程指南：

- Public attributes should have no leading underscores.<br>
> 公共属性不应使用前导下划线。

- If your public attribute name collides with a reserved keyword, append a single trailing underscore to your attribute name.  This is preferable to an abbreviation or corrupted spelling.  (However, notwithstanding this rule, 'cls' is the preferred spelling for any variable or argument which is known to be a class, especially the first argument to a class method.)<br>
> 如果公有属性名和保留关键字冲突，可以使用添加一个尾随下划线。这比缩写和简写的做法更优。（特别地，当变量或参数是一个类时，使用简写的cls代替class也是很常见的，类方法的第一个参数就是cls。）

    Note 1: See the argument name recommendation above for class methods.<br>
> 关于这点可以参见上文中函数和方法的参数这一小节中对参数命名的约定。

- For simple public data attributes, it is best to expose just the attribute name, without complicated accessor/mutator methods.  Keep in mind that Python provides an easy path to future enhancement, should you find that a simple data attribute needs to grow functional behavior.  In that case, use properties to hide functional implementation behind simple data attribute access syntax.<br>
> 对于简单的公共属性，最好是直接公开属性名，不要使用复杂的访问/修改方法。值得注意，Python提供了一种非常方便的进一步增强功能的方法（这里指的应该是装饰器）。这种情况下，可以使用@property来把函数实现隐藏在简单的公共属性后面（这个装饰器允许把方法作为实例变量调用，利用对应的setter和getter方法来编写调用这个实例变量的增强功能，这样外部访问的时候，看起来就像是在访问一个普通的公共实例变量，真正复杂的实现被隐藏起来了）。

    Note 1: Properties only work on new-style classes.<br>
> Note 1：property只对采用新型风格的类有效（是否指Pythohn2.x以后的版本？这点待验证）

    Note 2: Try to keep the functional behavior side-effect free, although side-effects such as caching are generally fine.<br>
> Note 2：实现功能时应尽量避免带来的副作用，尽管像缓存之类的副作用通常是无伤大雅的。

    Note 3: Avoid using properties for computationally expensive operations; the attribute notation makes the caller believe that access is (relatively) cheap.<br>
> 避免对计算开销大的操作使用property；属性标记会让调用者认为这是一个开销（相对）较低的操作。

- If your class is intended to be subclassed, and you have attributes that you do not want subclasses to use, consider naming them with double leading underscores and no trailing underscores.  This invokes Python's name mangling algorithm, where the name of the class is mangled into the attribute name.  This helps avoid attribute name collisions should subclasses inadvertently contain attributes with the same name.<br>
> 如果你设计的类用于被继承，并且你希望一些属性不会被子类使用，可以在命名时采取双前导下划线的方式。当该属性调用时会自动触发Python的命名重整，把类型放在属性名前面。这样即使我们无意中在子类使用了相同的名称，也可以避免属性名的冲突。

    Note 1: Note that only the simple class name is used in the mangled name, so if a subclass chooses both the same class name and attribute name, you can still get name collisions.<br>
> 注意，命名重整只是简单地利用了类名，所以如果子类类名和基类类名也相同时，还是会构成命名冲突。

    Note 2: Name mangling can make certain uses, such as debugging and ``__getattr__()``, less convenient.  However the name mangling algorithm is well documented and easy to perform manually.<br>
> 命名重整是有特定的用法，比方说调试和使用``__getattr__()``时就不太方便。但对文档化和手动执行有帮助。

    Note 3: Not everyone likes name mangling. Try to balance the  need to avoid accidental name clashes with potential use by advanced callers.<br>
> 不是所有人都喜欢命名重整，所以要平衡好避免命名冲突和被高级调用者调用这二者的需求。


### 公共接口和内部接口
**Public and internal interfaces**

Any backwards compatibility guarantees apply only to public interfaces. Accordingly, it is important that users be able to clearly distinguish between public and internal interfaces.<br>
> 只保证公共接口的向后兼容性。因此，对于用户来说，能清晰地分辨出公共接口和内部接口是非常重要的。

Documented interfaces are considered public, unless the documentation explicitly declares them to be provisional or internal interfaces exempt from the usual backwards compatibility guarantees. All undocumented interfaces should be assumed to be internal.<br>
> 文档化的接口一般都是公共的，除非文档中指出这是不受向后兼容性保障的临时接口或内部接口。所有没有文档化的接口都应当认为是仅供模块内部使用的。

To better support introspection, modules should explicitly declare the names in their public API using the ``__all__`` attribute. Setting ``__all__`` to an empty list indicates that the module has no public API.<br>
> 为了便于回顾，模块中应当使用``__all__``属性来显式地指示所有公共API。如果``__all__``被设置为空列表，则表明该模块没有任何公共API。

Even with ``__all__`` set appropriately, internal interfaces (packages, modules, classes, functions, attributes or other names) should still be prefixed with a single leading underscore.<br>
> 即使已经设置好了``__all__``属性。内部接口（包，模块，类，函数，属性或者其他名字）还是要用一个先导下划线标示。

An interface is also considered internal if any containing namespace (package, module or class) is considered internal.<br>
> 当一个接口所属的命名空间（包，模块，类）是内部的，则该接口也被认为是内部的。

Imported names should always be considered an implementation detail. Other modules must not rely on indirect access to such imported names unless they are an explicitly documented part of the containing module's API, such as ``os.path`` or a package's ``__init__`` module that exposes functionality from submodules.<br>
> 导入的名字应当始终被视为一个实现细节。其他模块不能使用这些导入名（对导入的模块/函数/etc）进行间接的访问，除非这些导入名在其模块API的文档中有明确的说明。比方说``os.path``或是包中用于暴露子模块功能的``__init__``模块。


## 编程建议
**Programming Recommendations**

- Code should be written in a way that does not disadvantage other implementations of Python (PyPy, Jython, IronPython, Cython, Psyco, and such).<br>
> 编写的代码要利于其他Python实现（PyPy，Jython，IronPython，Cython，Psyco，诸如此类）。

    For example, do not rely on CPython's efficient implementation of in-place string concatenation for statements in the form ``a += b`` or ``a = a + b``.  This optimization is fragile even in CPython (it only works for some types) and isn't present at all in implementations that don't use refcounting.  In performance sensitive parts of the library, the ``''.join()`` form should be used instead.  This will ensure that concatenation occurs in linear time across various implementations.<br>
> 举个例子，不要依赖于CPython对``a += b`` or ``a = a + b``这种方便的字符串拼接实现，即便是用CPython进行解释，优化的效果也极其有限（只对某些类型有效），而且不是所有的Python实现都支持引用计数的。对于性能敏感的代码段，采用``''.join()``形式能保证在不同的Python实现中都可以在线性时间内完成字符串拼接。

- Comparisons to singletons like None should always be done with ``is`` or ``is not``, never the equality operators.<br>
> 和单元素集合（比如None）进行比较时，使用``is``或者``is not``，不要用等号。

    Also, beware of writing ``if x`` when you really mean ``if x is not None`` -- e.g. when testing whether a variable or argument that defaults to None was set to some other value.  The other value might have a type (such as a container) that could be false in a boolean context!<br>
> 同时，还要注意``if x``和``if x is not None``的区别。比方说需要测试一个变量或者默认值为None的参数是否被设置为其他值时，这个其他值有可能属于在布尔环境下会返回false的类型（比如容器），所以这种情况下``if x``返回false并不意味着x是None。

- Use ``is not`` operator rather than ``not ... is``.  While both expressions are functionally identical, the former is more readable and preferred.<br>
> 使用``is not``要比``not ... is``更好。虽然两种表达方式实现的功能是一致的，但是前者拥有更好的可读性。

正确示例：

```python
if foo is not None:
```

错误示例：

```python
if not foo is None:
```

- When implementing ordering operations with rich comparisons, it is best to implement all six operations (``__eq__``, ``__ne__``, ``__lt__``, ``__le__``, ``__gt__``, ``__ge__``) rather than relying on other code to only exercise a particular comparison.<br>
> 在实现富比较排序操作是，最好把六种比较操作全部实现（等于、不等于、小于，小于等于，大于，大于等于），而不是只依赖于某一种特定的比较操作。

To minimize the effort involved, the ``functools.total_ordering()`` decorator provides a tool to generate missing comparison methods.<br>
> 为了最小化需要完成的工作，Python提供``functools.total_ordering()``装饰器，用于自动生成缺少的比较方法。

[PEP 207](https://www.python.org/dev/peps/pep-0207) indicates that reflexivity rules *are* assumed by Python. Thus, the interpreter may swap ``y > x`` with ``x < y``, ``y >= x`` with ``x <= y``, and may swap the arguments of ``x == y`` and ``x != y``.  The ``sort()`` and ``min()`` operations are guaranteed to use the ``<`` operator and the ``max()`` function uses the ``>`` operator.  However, it is best to implement all six operations so that confusion doesn't arise in other contexts.<br>
> PEP 207中提到映射规则是有Python完成的，所以解释器有可能会调换参数的位置。比方说把``y > x`` 替换为 ``x < y``,，把``y >= x``替换为``x <= y``， 甚至可能对``x == y``和``x != y``的参数进行调换。 排序和求最小操作保证使用``<``运算符，求最大操作则使用``>``运算符。但是最好还是把六中操作全部实现以避免在其他环境中引起混淆。

- Always use a def statement instead of an assignment statement that binds a lambda expression directly to an identifier.<br>
> 总是采用显示的函数定义def，而不是采用隐式的lambda把函数定义直接赋值给标识符。

正确示例：

```python
def f(x): return 2*x
```
错误示例：

```python
f = lambda x: 2*x
```

The first form means that the name of the resulting function object is specifically 'f' instead of the generic '<lambda>'. This is more useful for tracebacks and string representations in general. The use of the assignment statement eliminates the sole benefit a lambda expression can offer over an explicit def statement (i.e. that it can be embedded inside a larger expression)<br>
> 第一种形式表示所得函数对象的名字是f，而不是'<lambda>'泛型。通常来说，这对于追踪和字符串表述是很有帮助的。使用lambda唯一的好处就是它可以嵌入到一个很长的表达式当中，这是def语句无法做到的。

- Derive exceptions from ``Exception`` rather than ``BaseException``. Direct inheritance from ``BaseException`` is reserved for exceptions where catching them is almost always the wrong thing to do.<br>
> 异常类应继承自``Exception``类而不是``BaseException``类。直接继承``BaseException``类是一种被保留的用法，仅用于一些特定的异常类型，捕获这些异常基本上都是不应该的（结合下文可以更好地理解，比方说捕获一个由Control-C引起的程序中断异常，一般不会这样做）。

Design exception hierarchies based on the distinctions that code *catching* the exceptions is likely to need, rather than the locations where the exceptions are raised. Aim to answer the question "What went wrong?" programmatically, rather than only stating that "A problem occurred" (see [PEP 3151](https://www.python.org/dev/peps/pep-3151) for an example of this lesson being learned for the builtin exception hierarchy)<br>
> 要区分开出现异常的代码段和出现异常的位置，设计异常的层级结构时应该基于前者。设计异常的目的是要回答“什么导致了错误”，而不是仅仅指出“程序出错了”。（详细例子参见PEP 3151）

Class naming conventions apply here, although you should add the suffix "Error" to your exception classes if the exception is an error.  Non-error exceptions that are used for non-local flow control or other forms of signaling need no special suffix.<br>
> 类的命名约定适用于异常的命名。如果异常类是一个错误，应当为其加上后缀“Error”。但对于哪些不是错误的用于非本地流程或其他形式通知的异常来说，不需要添加特定的后缀。

- Use exception chaining appropriately. In Python 3, "raise X from Y" should be used to indicate explicit replacement without losing the original traceback.<br>
> 适当地使用异常链。在Python3中，"raise X from Y"应当用于明确表示替换并且保留着原有的回溯。

When deliberately replacing an inner exception (using "raise X" in Python 2 or "raise X from None" in Python 3.3+), ensure that relevant details are transferred to the new exception (such as preserving the attribute name when converting KeyError to AttributeError, or embedding the text of the original exception in the new exception message).<br>
> 在替换内部异常(比方说，Python 2中的``raise X``或Python 3.3版本以上的``raise X from None``)时，确保相关的细节被转移到新的异常中（比方说把KeyError转换为AttributeError时保留属性名，或这把原来异常的文本嵌入到新异常的消息里面)。

- When raising an exception in Python 2, use ``raise ValueError('message')`` instead of the older form ``raise ValueError, 'message'``.<br>
> 在Python2中，使用``raise ValueError('message')``来报告异常而不是旧版的``raise ValueError, 'message'``。

    The latter form is not legal Python 3 syntax.<br>
> 后者在Python 3中已经废除了。

    The paren-using form also means that when the exception arguments are long or include string formatting, you don't need to use line continuation characters thanks to the containing parentheses.<br>
> 使用括号意味着，当异常的参数很长或者包含字符串格式化时，我们可以借助Python隐式行连接的特性（把语句分散在多行编写），而无须使用续行符进行续行。

- When catching exceptions, mention specific exceptions whenever possible instead of using a bare ``except:`` clause. For example, use:<br>
> 在捕获异常时应明确地提及到异常名，而不是空的``except:``子句。例如：

```python
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```

A bare ``except:`` clause will catch SystemExit and KeyboardInterrupt exceptions, making it harder to interrupt a program with Control-C, and can disguise other problems.  If you want to catch all exceptions that signal program errors, use ``except Exception:`` (bare except is equivalent to ``except BaseException:``).<br>
> 空的``except:``子句由于没有指定要捕获的异常类型，所以SystemExit和KeyboardInterrupt这两种异常也会被捕获，从而使得用户难以使用Control-C中断程序（因为无法判断异常是有中断引起还是由其他问题引起）。如果想要捕获所有可能的程序错误，可以使用``except Exception:``语句（空的``except:``语句等同于``except BaseException:``）。

A good rule of thumb is to limit use of bare 'except' clauses to two cases:<br>
> 经验告诉我们，仅在两种情形下需要使用空的``except:``子句：

1. If the exception handler will be printing out or logging the traceback; at least the user will be aware that an error has occurred.<br>
> 异常的处理程序会被打印出来或者会记录回溯信息；至少能让用户意识到错误发生了。

2. If the code needs to do some cleanup work, but then lets the exception propagate upwards with ``raise``.  ``try...finally`` can be a better way to handle this case.<br>
> 代码需要进行一些清理工作，但接下来会用``raise``把异常向上传播。这种情况下，使用``try...finally``语句更好。

<span> </span>

- When binding caught exceptions to a name, prefer the explicit name binding syntax added in Python 2.6:<br>
> 要把异常绑定到一个名字上时，最好使用Python 2.6中加入的显示命名绑定语法：
```python
try:
    process_data()
except Exception as exc:
    raise DataProcessingFailedError(str(exc))
```

    This is the only syntax supported in Python 3, and avoids the ambiguity problems associated with the older comma-based syntax.<br>
> 这也是Python 3中唯一支持的语法，避免由基于逗号的旧版语法引起的混淆问题。

- When catching operating system errors, prefer the explicit exception hierarchy introduced in Python 3.3 over introspection of ``errno`` values.<br>
> 捕获操作系统错误时，最好采用Python 3.3中引入的显式异常层级结构，而不是回溯的``errno``值。

- Additionally, for all try/except clauses, limit the ``try`` clause to the absolute minimum amount of code necessary.  Again, this avoids masking bugs.<br>
> 此外，对于所有的try/except子句，应尽量限制try子句中代码的数量，这样可以避免错误被掩盖。例如：

正确示例：

```python
try:
    value = collection[key]
except KeyError:
    return key_not_found(key)
else:
    return handle_value(value)
```

错误示例：

```python
try:
    # Too broad!
    return handle_value(collection[key])
except KeyError:
    # Will also catch KeyError raised by handle_value()
    return key_not_found(key)
```

- When a resource is local to a particular section of code, use a ``with`` statement to ensure it is cleaned up promptly and reliably after use. A try/finally statement is also acceptable.<br>
> 访问本地资源应当使用``with``语句，这能够保证在使用完毕后程序会立即进行清理。try/finally语句也是可以接受的。

- Context managers should be invoked through separate functions or methods whenever they do something other than acquire and release resources. For example:<br>
> 使用上下文管理器做除获取/释放资源以外的事情时，应通过独立的函数或方法进行调用。例如：

正确示例：
```python
with conn.begin_transaction():
    do_stuff_in_transaction(conn)
```
错误示例：
```python
with conn:
    do_stuff_in_transaction(conn)
```

The latter example doesn't provide any information to indicate that the __enter__ and __exit__ methods are doing something other than closing the connection after a transaction.  Being explicit is important in this case.<br>
> 后面的例子除了说明在事务结束后关闭连接之外，没有提供任何信息来指示\_\_enter\_\_和\_\_exit\_\_正在处理某些别的任务。 显式地指明在这种情况下是很重要的。

- Be consistent in return statements.  Either all return statements in a function should return an expression, or none of them should.  If any return statement returns an expression, any return statements where no value is returned should explicitly state this as ``return None``, and an explicit return statement should be present at the end of the function (if reachable).<br>
> return语句的使用要一致。函数中的所有return语句要么都返回表达式，要么都采取别的形式。如果return语句是返回表达式的，则无返回值的return语句应该写成显式的``return None``，并且在函数底部（如过可以到达）应使用一个明确的return语句。

正确示例：

```python
def foo(x):
    if x >= 0:
        return math.sqrt(x)
    else:
        return None

def bar(x):
    if x < 0:
        return None
    return math.sqrt(x)
```

错误示例：

```python
def foo(x):
    if x >= 0:
        return math.sqrt(x)

def bar(x):
    if x < 0:
        return
    return math.sqrt(x)
```

- Use string methods instead of the string module.<br>
> 使用字符串方法来替代字符串模块。

    String methods are always much faster and share the same API with unicode strings.  Override this rule if backward compatibility with Pythons older than 2.0 is required.<br>
> 字符串方法（相比模块）总是更快，并且和unicode字符串享有相同的API。如果需要保证和Python 2.0版本以前的向后兼容性，则覆盖掉这条规则。

- Use ``''.startswith()`` and ``''.endswith()`` instead of string slicing to check for prefixes or suffixes.<br>
> 使用``''.startswith()``和``''.endswith()``代替字符串切片来检查字符串的前缀和后缀。

    startswith() and endswith() are cleaner and less error prone.  For example:<br>
> startswith()和endswith()更加简介并且不容易出错，例如：

```python
# 正确示例
if foo.startswith('bar'):
# 错误示例
if foo[:3] == 'bar':
```

- Object type comparisons should always use isinstance() instead of comparing types directly. :<br>
> 比较对象类型时是总是使用isinstance()函数，而不是直接比较类型：

```python
# 正确示例
if isinstance(obj, int):
# 错误示例
if type(obj) is type(1):
```

When checking if an object is a string, keep in mind that it might be a unicode string too!  In Python 2, str and unicode have a common base class, basestring, so you can do:<br>
> 在检查一个对象是否字符串时，注意它也可能是unicode字符串！在Python 2中，str和unicode有一个共同的基类 -- basestring，所以你可以这样做：

```python
if isinstance(obj, basestring):
```

Note that in Python 3, ``unicode`` and ``basestring`` no longer exist (there is only ``str``) and a bytes object is no longer a kind of string (it is a sequence of integers instead)<br>
> 注意，在Python 3中，``unicode``和``basestring``已经不存在了（只有``str``），并且bytes对象不再是一种字符串（变为整数序列）。

- For sequences, (strings, lists, tuples), use the fact that empty sequences are false. :<br>
> 空序列（字符串，列表，元组等等）在布尔环境下返回false，我们可以利用这一点：

```python
# 正确示例
if not seq:
if seq:

# 错误示例
if len(seq):
if not len(seq):
```

- Don't write string literals that rely on significant trailing whitespace.  Such trailing whitespace is visually indistinguishable and some editors (or more recently, reindent.py) will trim them.<br>
> 不要让字符串依赖于尾随的空格。这些尾随的空格在视觉上无法作出区分，一些编辑器（更近期的，reindent.py）会把这些空格移除掉。

- Don't compare boolean values to True or False using ``==``. :<br>
> 不要用==比较True或者False：

```python
# 正确示例
if greeting:
# 错误示例
if greeting == True:
# 更糟的是
if greeting is True:
```

### 函数注解
**Function Annotations**

With the acceptance of [PEP 484](https://www.python.org/dev/peps/pep-0484), the style rules for function annotations are changing.<br>
> 随着PEP 484逐渐被接受，函数注解的风格规范产生了变化。

- In order to be forward compatible, function annotations in Python 3 code should preferably use [PEP 484](https://www.python.org/dev/peps/pep-0484) syntax.  (There are some formatting recommendations for annotations in the previous section.)<br>
> 为了保证向前兼容，Python 3代码中的函数注解应采用PEP 484中推荐的语法（在先前的章节中已经提及到了一些推荐的注解格式）。

- The experimentation with annotation styles that was recommended previously in this PEP is no longer encouraged.<br>
> 在这份PEP早期版本中推荐的函数注解风格试验现在已经不再推荐了。

- However, outside the stdlib, experiments within the rules of [PEP 484](https://www.python.org/dev/peps/pep-0484) are now encouraged.  For example, marking up a large third party library or application with [PEP 484](https://www.python.org/dev/peps/pep-0484) style type annotations, reviewing how easy it was to add those annotations, and observing whether their presence increases code understandabilty.<br>
> 然而，除了标准库之外，在PEP 484规则之内的试验都是被推荐使用的。 比方说，使用PEP 484风格的类型注解来标记大的第三方库或应用，审视加入这些注解的方式是否足够简单，以及它们是否能提高代码的可读性。

- The Python standard library should be conservative in adopting such annotations, but their use is allowed for new code and for big refactorings.<br>
> Python标准库中对是否使用这样的注解是持保守态度的，但是在编写新代码和大的重构时可以采用。

- For code that wants to make a different use of function annotations it is recommended to put a comment of the form: ``# type: ignore``
near the top of the file; this tells type checker to ignore all annotations.  (More fine-grained ways of disabling complaints from type checkers can be found in [PEP 484](https://www.python.org/dev/peps/pep-0484).)<br>
> 当你想把函数注解用作别的用途时，推荐把``# type: ignore``放到文件开头；这样类型检查器或忽略所有注释（更细致的一种方式是禁用类型检查器的complaint，可以在PEP 484中找到详细的用法）。

- Like linters, type checkers are optional, separate tools.  Python interpreters by default should not issue any messages due to type checking and should not alter their behavior based on annotations.<br>
> 类似于linter（用于检查代码风格/错误的小工具，可以让使用者更方便地发现一些排版错误），类型检查器是一种可选的，独立的工具。Python解释器在默认模式下不应该发出任何类型检查的信息，也不应该修改它们对注释作出的行为。

- Users who don't want to use type checkers are free to ignore them. However, it is expected that users of third party library packages may want to run type checkers over those packages.  For this purpose [PEP 484](https://www.python.org/dev/peps/pep-0484) recommends the use of stub files: .pyi files that are read by the type checker in preference of the corresponding .py files. Stub files can be distributed with a library, or separately (with the library author's permission) through the typeshed repo [[5]](https://www.python.org/dev/peps/pep-0008/#id12).<br>
> 用户可以不使用类型检查器并直接忽略它们。但是，有可能用户使用的第三方库需要用到类型检查器。为了实现这一点，PEP 484中推荐使用存根（stub）文件，也即.pyi文件，这些文件会在对应的.py文件之前被类型检查器读入。 存根文件可以和库一起发布，也可以单独（在得到库的作者允许的情况下）地通过typeshed（Python的librarystub的集合，用于为Python标准库和Python内建，以及第三方软件包。此数据可用于静态分析,类型检查和类型推断）发布。

- For code that needs to be backwards compatible, function annotations can be added in the form of comments. See the relevant section of [PEP 484](https://www.python.org/dev/peps/pep-0484) [[6]](https://www.python.org/dev/peps/pep-0008/#id13) .<br>
> 对于需要保持向后兼容性的代码，函数注解可以用注释的方式添加。可以在PEP 484的相关章节找到具体的说明。

Footnotes<br>
> 脚注

| [[7]](https://www.python.org/dev/peps/pep-0008/#id3) | Hanging indentation is a type-setting style where all the lines in a paragraph are indented except the first line.<br> In the context of Python, the term is used to describe a style where the opening parenthesis of a parenthesized<br> statement is the last non-whitespace character of the line, with subsequent lines being indented until the<br> closing parenthesis. |
|:---:|:---|
| | 悬挂缩进是一种段中除首行以外都被缩进的排版风格。在Python中，这个术语用于描述括号语句中，左括号是首行最后<br>一个非空白字符，后续行被统一缩进直到出现右括号（不需另起一行）的风格。 |



## 参考文献
**References**

1. [PEP 7](https://www.python.org/dev/peps/pep-0007), Style Guide for C Code, van Rossum
2. Barry's GNU Mailman style guide
    * <http://barry.warsaw.us/software/STYLEGUIDE.txt>
3. <http://www.wikipedia.com/wiki/CamelCase>
4. Typeshed repo
    * <https://github.com/python/typeshed>
5. mypy type checker
    * <http://mypy-lang.org>
    * <https://github.com/JukkaL/mypy>
6. Suggested syntax for Python 2.7 and straddling code
    * <https://www.python.org/dev/peps/pep-0484/#suggested-syntax-for-python-2-7-and-straddling-code>


## 版权
**Copyright**

This document has been placed in the public domain.<br>
> 这份文档是公开的。

Source: <https://hg.python.org/peps/file/tip/pep-0008.txt>
> 源文件：<https://hg.python.org/peps/file/tip/pep-0008.txt>。

