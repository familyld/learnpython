# Pandas 学习

该笔记摘录自微信公众号“每天进步一点点2015”的文章[《Python数据分析之pandas学习（一）》](http://mp.weixin.qq.com/s?__biz=MzIxNjA2ODUzNg==&mid=2651435170&idx=1&sn=1a112bead78327a672ebb1c2d2167b4f)和[《Python数据分析之pandas学习（二）》](http://mp.weixin.qq.com/s?__biz=MzIxNjA2ODUzNg==&mid=2651435212&idx=1&sn=86ab8a363c590e841c6e9e5615bf8c45)。我对代码和讲解中不够清晰的地方进行了一些改动和补充。

有关pandas模块的学习与应用主要介绍以下8个部分：

1. 数据结构简介：DataFrame和Series
2. 数据索引index
3. 利用pandas查询数据
4. 利用pandas的DataFrames进行统计分析
5. 利用pandas实现SQL操作
6. 利用pandas进行缺失值的处理
7. 利用pandas实现Excel的数据透视表功能
8. 多层索引的使用

## 数据结构介绍
在pandas中有两类非常重要的数据结构，即序列Series和数据框DataFrame。Series类似于numpy中的一维数组，除了**通吃一维数组可用的函数或方法**，而且其可通过索引标签的方式获取数据，还**具有索引的自动对齐功能**；DataFrame类似于numpy中的二维数组，同样可以通用numpy数组的函数和方法，而且还具有其他灵活应用，后续会介绍到。

### 1、Series的创建

序列的创建主要有三种方式：

#### 1）通过一维数组创建序列

```python
In [1]: import numpy as np, pandas as pd
In [2]: arr1 = np.arange(10)
In [3]: arr1
Out[3]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
In [4]: type(arr1)
Out[4]: numpy.ndarray
```

返回的是数组类型。

```python
In [5]: s1 = pd.Series(arr1)
In [6]: s1
Out[6]:
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int32
In [7]: type(s1)
Out[7]: pandas.core.series.Series
```

返回的是序列类型。

#### 2）通过字典的方式创建序列

这种情况下字典的key会被作为Series的索引。

```python
In [8]: dic1 = {'a':10,'b':20,'c':30,'d':40,'e':50}
In [9]: dic1
Out[9]: {'a': 10, 'b': 20, 'c': 30, 'd': 40, 'e': 50}
In [10]: type(dic1)
Out[10]: dict
```

返回的是字典类型。

```python
In [11]: s2 = pd.Series(dic1)
In [12]: s2
Out[12]:
a    10
b    20
c    30
d    40
e    50
dtype: int64
In [13]: type(s2)
Out[13]: pandas.core.series.Series
```

返回的是序列类型。

补充一点，**使用数组创建序列也是可以自定义索引的**，通过index关键字传入同值数组一样大小的数组即可：

```python
>>> a = np.arange(1,6)
>>> b = np.arange(2,7)
>>> pd.Series(a, index=b)
2    1
3    2
4    3
5    4
6    5
dtype: int32
```

#### 3）通过DataFrame中的某一行或某一列创建序列

这部分内容我们放在后面讲，接下来就开始讲一讲如何构造一个DataFrame。

### 2、DataFrame的创建

数据框的创建主要有三种方式：

#### 1）通过二维数组创建数据框

```python
In [14]: arr2 = np.arange(12).reshape(4,3)
In [15]: arr2
Out[15]:
array([[ 0,  1,  2],
[ 3,  4,  5],
[ 6,  7,  8],
[ 9, 10, 11]])
In [16]: type(arr2)
Out[16]: numpy.ndarray
```

返回的是数组类型。

```python
In [17]: df1 = pd.DataFrame(arr2)
In [18]: df1
Out[18]:
   0   1   2
0  0   1   2
1  3   4   5
2  6   7   8
3  9  10  11
In [19]: type(df1)
Out[19]: pandas.core.frame.DataFrame
```

返回的数据框类型。

#### 2）通过字典的方式创建数据框

以下以两种字典来创建数据框，一个是字典列表，一个是嵌套字典。

```python
In [20]: dic2 = {'a':[1,2,3,4],'b':[5,6,7,8],
    ...:         'c':[9,10,11,12],'d':[13,14,15,16]}
In [21]: dic2
Out[21]:
{'a': [1, 2, 3, 4],
'b': [5, 6, 7, 8],
'c': [9, 10, 11, 12],
'd': [13, 14, 15, 16]}
In [22]: type(dic2)
Out[22]: dict
```

返回的是字典类型。

```python
In [23]: df2 = pd.DataFrame(dic2)
In [24]: df2
Out[24]:
   a  b   c   d
0  1  5   9  13
1  2  6  10  14
2  3  7  11  15
3  4  8  12  16
In [25]: type(df2)
Out[25]: pandas.core.frame.DataFrame
```

返回的是数据框类型。

```python
In [26]: dic3 = {'one':{'a':1,'b':2,'c':3,'d':4},
    ...:         'two':{'a':5,'b':6,'c':7,'d':8},
    ...:         'three':{'a':9,'b':10,'c':11,'d':12}}
In [27]: dic3
Out[27]:
{'one': {'a': 1, 'b': 2, 'c': 3, 'd': 4},
'three': {'a': 9, 'b': 10, 'c': 11, 'd': 12},
'two': {'a': 5, 'b': 6, 'c': 7, 'd': 8}}
In [28]: type(dic3)
Out[28]: dict
```

返回的是字典类型。

```python
In [29]: df3 = pd.DataFrame(dic3)
In [30]: df3
Out[30]:
    one  three  two
a    1      9    5
b    2     10    6
c    3     11    7
d    4     12    8
In [31]: type(df3)
Out[31]: pandas.core.frame.DataFrame
```

返回的是数据框类型。这里需要说明的是，如果使用嵌套字典创建数据框的话，**嵌套字典的最外层键会形成数据框的列变量(columns)，而内层键则会形成数据框的行索引(index)**。

同样补充一下，用数组来创建数据框的话也是可以通过columns关键字和index关键字分别指定列名和行索引的：

```python
>>> a = np.arange(1,7).reshape(3,2)
>>> a
array([[1, 2],
       [3, 4],
       [5, 6]])
>>> pd.DataFrame(a, columns=['c1','c2'], index=['r1','r2','r3'])
    c1  c2
r1   1   2
r2   3   4
r3   5   6
```

#### 3）通过数据框的方式创建数据框

可以取数据框的一部分构成新的数据框，索引的用法和numpy是一致的。

```python
In [32]: df4 = df3[['one','three']]
In [33]: df4
Out[33]:
    one  three
a    1      9
b    2     10
c    3     11
d    4     12
In [34]: type(df4)
Out[34]: pandas.core.frame.DataFrame
```

返回的是数据框类型。

```python
In [35]: s3 = df3['one']
In [36]: s3
Out[36]:
a    1
b    2
c    3
d    4
Name: one, dtype: int64
In [37]: type(s3)
Out[37]: pandas.core.series.Series
```

如果只选择数据框中的某一列/某一行，返回的就会是一个序列对象。

