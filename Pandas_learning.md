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

## 数据索引index

细致的朋友可能会发现一个现象，不论是序列也好，还是数据框也好，对象的最左边总有一个非原始数据对象，这个是什么呢？不错，就是我们接下来要介绍的索引。
在我看来，序列或数据框的索引有两大用处，一个是**通过索引值或索引标签获取目标数据**，另一个是**通过索引，可以使序列或数据框的计算、操作实现自动化对齐**，下面我们就来看看这两个功能的应用。

### 1、通过索引值或索引标签获取数据

```python
In [38]: s4 = pd.Series(np.array([1,1,2,3,5,8]))
In [39]: s4
Out[39]:
0    1
1    1
2    2
3    3
4    5
5    8
dtype: int32
```

**如果不给序列一个指定的索引值，则序列自动生成一个从0开始的自增索引**。可以通过index查看序列的索引：

```python
In [40]: s4.index
Out[40]: RangeIndex(start=0, stop=6, step=1)
```

现在我们为序列设定一个自定义的索引值：

```python
In [41]: s4.index = ['a','b','c','d','e','f']
In [42]: s4
Out[42]:
a    1
b    1
c    2
d    3
e    5
f    8
dtype: int32
```

序列有了索引，就**可以通过索引值或索引标签**进行数据的获取：

```python
In [43]: s4[3] # 获取序列的第4个元素
Out[43]: 3
In [44]: s4['e'] # 获取序列中索引为'e'的元素
Out[44]: 5
In [45]: s4[[1,3,5]] # 获取序列的第2，4，6个元素
Out[45]:
b    1
d    3
f    8
dtype: int32
In [46]: s4[['a','b','d','f']]
Out[46]:
a    1
b    1
d    3
f    8
dtype: int32
In [47]: s4[:4]
Out[47]:
a    1
b    1
c    2
d    3
dtype: int32
In [48]: s4['c':]
Out[48]:
c    2
d    3
e    5
f    8
dtype: int32
In [49]: s4['b':'e']
Out[49]:
b    1
c    2
d    3
e    5
dtype: int32
```

千万注意：**如果通过索引标签获取数据的话，末端标签所对应的值是可以返回的**（区间左右都是闭合的）！在一维数组中，就无法通过索引标签获取数据，这也是序列不同于一维数组的一个方面。

### 2、自动化对齐

如果有两个序列，需要对这两个序列进行算术运算，这时索引的存在就体现的它的价值了--自动化对齐。

```python
In [50]: s5 = pd.Series(np.array([10,15,20,30,55,80]),
    ...:                index = ['a','b','c','d','e','f'])
In [51]: s5
Out[51]:
a    10
b    15
c    20
d    30
e    55
f    80
dtype: int32

In [52]: s6 = pd.Series(np.array([12,11,13,15,14,16]),
    ...:                index = ['a','c','g','b','d','f'])
In [53]: s6
Out[53]:
a    12
c    11
g    13
b    15
d    14
f    16
dtype: int32

In [54]: s5 + s6 # 这两个序列在创建时索引顺序是不同的，但计算时按照索引进行了对齐
Out[54]:
a    22.0
b    30.0
c    31.0
d    44.0
e     NaN
f    96.0
g     NaN
dtype: float64

In [55]: s5/s6
Out[55]:
a    0.833333
b    1.000000
c    1.818182
d    2.142857
e         NaN
f    5.000000
g         NaN
dtype: float64
```

**由于s5中没有对应的g索引，s6中没有对应的e索引，所以数据的运算会产生两个缺失值NaN**。注意，这里的算术结果就实现了两个序列索引的自动对齐，而非简单的将两个序列加总或相除。对于**数据框的对齐，不仅仅是行索引的自动对齐，同时也会自动对齐列索引（变量名）**。

数据框中同样有索引，而且数据框是二维数组的推广，所以数据框不仅有行索引，而且还存在列索引，关于数据框中的索引相比于序列的应用要强大的多，这部分内容将放在下面的数据查询中讲解。

## 利用pandas查询数据

这里的查询数据相当于R语言里的subset功能，可以通过布尔索引有针对的选取原数据的子集、指定行、指定列等。我们先导入一个student数据集：

```python
In [56]: student = pd.io.parsers.read_csv('C:\\Users\\admin\\Desktop\\student.csv')
```

查询数据的前5行或末尾5行：

```python
In [57]: student.head()
Out[57]:
      Name Sex  Age  Height  Weight
0   Alfred   M   14    69.0   112.5
1    Alice   F   13    56.5    84.0
2  Barbara   F   13    65.3    98.0
3    Carol   F   14    62.8   102.5
4    Henry   M   14    63.5   102.5

In [58]: student.tail()
Out[58]:
       Name Sex  Age  Height  Weight
14   Philip   M   16    72.0   150.0
15   Robert   M   12    64.8   128.0
16   Ronald   M   15    67.0   133.0
17   Thomas   M   11    57.5    85.0
18  William   M   15    66.5   112.0
```

查询指定的行：

```python
In [59]: student.ix[[0,2,4,5,7]] #这里的ix索引标签函数必须是中括号[]
Out[59]:
      Name Sex  Age  Height  Weight
0   Alfred   M   14    69.0   112.5
2  Barbara   F   13    65.3    98.0
4    Henry   M   14    63.5   102.5
5    James   M   12    57.3    83.0
7    Janet   F   15    62.5   112.5
```

查询指定的列：

```python
In [60]: student[['Name','Height','Weight']].head()  #如果多个列的话，必须使用双重中括号
Out[60]:
      Name  Height  Weight
0   Alfred    69.0   112.5
1    Alice    56.5    84.0
2  Barbara    65.3    98.0
3    Carol    62.8   102.5
4    Henry    63.5   102.5
```

也可以通过ix索引标签查询指定的列：

```python
In [61]: student.ix[:,['Name','Height','Weight']].head()
Out[61]:
      Name  Height  Weight
0   Alfred    69.0   112.5
1    Alice    56.5    84.0
2  Barbara    65.3    98.0
3    Carol    62.8   102.5
4    Henry    63.5   102.5
```

查询指定的行和列：

```python
In [62]: student.ix[[0,2,4,5,7],['Name','Height','Weight']].head()
Out[62]:
      Name  Height  Weight
0   Alfred    69.0   112.5
2  Barbara    65.3    98.0
4    Henry    63.5   102.5
5    James    57.3    83.0
7    Janet    62.5   112.5
```

这里简单说明一下**ix的用法**：`df.ix[行索引,列索引]`

1. ix后面必须是中括号
2. 多个行索引或列索引必须用中括号括起来
3. 如果选择所有行索引或列索引，则用英文状态下的冒号:表示

以上是从行或列的角度查询数据的子集，现在我们来看看如何**通过布尔索引实现数据的子集查询**。

查询所有女生的信息：

```python
In [63]: student[student['Sex']=='F']
Out[63]:
       Name Sex  Age  Height  Weight
1     Alice   F   13    56.5    84.0
2   Barbara   F   13    65.3    98.0
3     Carol   F   14    62.8   102.5
6      Jane   F   12    59.8    84.5
7     Janet   F   15    62.5   112.5
10    Joyce   F   11    51.3    50.5
11     Judy   F   14    64.3    90.0
12   Louise   F   12    56.3    77.0
13     Mary   F   15    66.5   112.0
```

查询出所有12岁以上的女生信息：

```python
In [64]: student[(student['Sex']=='F') & (student['Age']>12)]
Out[64]:
       Name Sex  Age  Height  Weight
1     Alice   F   13    56.5    84.0
2   Barbara   F   13    65.3    98.0
3     Carol   F   14    62.8   102.5
7     Janet   F   15    62.5   112.5
11     Judy   F   14    64.3    90.0
13     Mary   F   15    66.5   112.0
```

查询出所有12岁以上的女生姓名、身高和体重：

```python
In [66]: student[(student['Sex']=='F') & (student['Age']>12)][['Name','Height','Weight']]
Out[66]:
       Name  Height  Weight
1     Alice    56.5    84.0
2   Barbara    65.3    98.0
3     Carol    62.8   102.5
7     Janet    62.5   112.5
11     Judy    64.3    90.0
13     Mary    66.5   112.0
```

上面的查询逻辑其实非常的简单，需要注意的是，如果是多个条件的查询，必须在&（且）或者|（或）的两端条件用括号括起来。

补充一下，除了使用在方括号索引内，布尔索引同样也能用于ix中作为行索引的部分：

```python
>>> df
   0  1
a  1  2
b  3  4
c  0  0
>>> df[(df[0]==1) | (df[0]==3)]
   0  1
a  1  2
b  3  4
>>> df.ix[(df[0]==1) | (df[0]==3)]
   0  1
a  1  2
b  3  4
>>> df.ix[(df[0]==1) | (df[0]==3), 1::-1]
   1  0
a  2  1
b  4  3
```

## 统计分析

pandas模块为我们提供了非常多的描述性统计分析的指标函数，如总和、均值、最小值、最大值等，我们来具体看看这些函数：

首先随机生成三组数据

```python
In [67]: np.random.seed(1234)
In [68]: d1 = pd.Series(2*np.random.normal(size = 100)+3)
In [69]: d2 = np.random.f(2,4,size = 100)
In [70]: d3 = np.random.randint(1,100,size = 100)

In [71]: d1.count()  #非空元素计算
Out[71]: 100
In [72]: d1.min()    #最小值
Out[72]: -4.1270333212494705
In [73]: d1.max()    #最大值
Out[73]: 7.7819210309260658
In [74]: d1.idxmin() #最小值的位置，类似于R中的which.min函数
Out[74]: 81
In [75]: d1.idxmax() #最大值的位置，类似于R中的which.max函数
Out[75]: 39
In [76]: d1.quantile(0.1)    #10%分位数
Out[76]: 0.68701846440699277
In [77]: d1.sum()    #求和
Out[77]: 307.0224566250874
In [78]: d1.mean()   #均值
Out[78]: 3.070224566250874
In [79]: d1.median() #中位数
Out[79]: 3.204555266776845
In [80]: d1.mode()   #众数
Out[80]: Series([], dtype: float64)
In [81]: d1.var()    #方差
Out[81]: 4.005609378535085
In [82]: d1.std()    #标准差
Out[82]: 2.0014018533355777
In [83]: d1.mad()    #平均绝对偏差
Out[83]: 1.5112880411556109
In [84]: d1.skew()   #偏度
Out[84]: -0.64947807604842933
In [85]: d1.kurt()   #峰度
Out[85]: 1.2201094052398012
In [86]: d1.describe()   #一次性输出多个描述性统计指标
Out[86]:
count    100.000000
mean       3.070225
std        2.001402
min       -4.127033
25%        2.040101
50%        3.204555
75%        4.434788
max        7.781921
dtype: float64
```

必须注意的是，**describe方法只能针对序列或数据框，一维数组（numpy.ndarray）是没有这个方法的**。

这里自定义一个函数，将这些统计描述指标全部汇总到一起：

```python
In [87]: def stats(x):
    ...:     return pd.Series([x.count(),x.min(),x.idxmin(),
    ...:                x.quantile(.25),x.median(),
    ...:                x.quantile(.75),x.mean(),
    ...:                x.max(),x.idxmax(),
    ...:                x.mad(),x.var(),
    ...:                x.std(),x.skew(),x.kurt()],
    ...:               index = ['Count','Min','Whicn_Min',
    ...:                        'Q1','Median','Q3','Mean',
    ...:                        'Max','Which_Max','Mad',
    ...:                        'Var','Std','Skew','Kurt'])

In [88]: stats(d1)
Out[88]:
Count        100.000000
Min           -4.127033
Whicn_Min     81.000000
Q1             2.040101
Median         3.204555
Q3             4.434788
Mean           3.070225
Max            7.781921
Which_Max     39.000000
Mad            1.511288
Var            4.005609
Std            2.001402
Skew          -0.649478
Kurt           1.220109
dtype: float64
```

在实际的工作中，我们可能需要处理的是一系列的数值型数据框，**如何将这个函数应用到数据框中的每一列呢**？可以使用apply函数，参数axis默认为0，将函数应用到数据框的每一列；设置为1时，则将函数应用到每一行。

将之前创建的d1,d2,d3数据构建数据框：

```python
In [89]: df = pd.DataFrame(np.array([d1,d2,d3]).T,columns=['x1','x2','x3'])
In [90]: df.head()
Out[90]:
         x1        x2    x3
0  3.942870  1.369531  55.0
1  0.618049  0.943264  68.0
2  5.865414  0.590663  73.0
3  2.374696  0.206548  59.0
4  1.558823  0.223204  60.0

In [91]: df.apply(stats)
Out[91]:
                   x1          x2          x3
Count      100.000000  100.000000  100.000000
Min         -4.127033    0.014330    3.000000
Whicn_Min   81.000000   72.000000   76.000000
Q1           2.040101    0.249580   25.000000
Median       3.204555    1.000613   54.500000
Q3           4.434788    2.101581   73.000000
Mean         3.070225    2.028608   51.490000
Max          7.781921   18.791565   98.000000
Which_Max   39.000000   53.000000   96.000000
Mad          1.511288    1.922669   24.010800
Var          4.005609   10.206447  780.090808
Std          2.001402    3.194753   27.930106
Skew        -0.649478    3.326246   -0.118917
Kurt         1.220109   12.636286   -1.211579
```

非常完美，就这样很简单的创建了数值型数据的统计性描述。**如果是离散型数据呢**？就不能用这个统计口径了，**我们需要统计离散变量的观测数、唯一值个数、众数水平及个数**。你只需要使用describe方法就可以实现这样的统计了。

```python
In [92]: student['Sex'].describe()
Out[92]:
count     19 # 有多少个样本
unique     2 # 唯一值个数
top        M # 众数
freq      10 # 众数出现的次数
Name: Sex, dtype: object
```

除以上的简单描述性统计之外，还提供了**连续变量的相关系数（corr）和协方差矩阵（cov）的求解**，这个跟R语言是一致的用法。

```python
In [93]: df.corr() # 返回pair-wise相关系数，所以是一个对称矩阵
Out[93]:
          x1        x2        x3
x1  1.000000  0.136085  0.037185
x2  0.136085  1.000000 -0.005688
x3  0.037185 -0.005688  1.000000
```

关于**相关系数的计算可以调用pearson方法或kendell方法或spearman方法**，默认使用pearson方法。

```python
In [94]: df.corr('spearman')
Out[94]:
         x1        x2        x3
x1  1.00000  0.178950  0.006590
x2  0.17895  1.000000 -0.033874
x3  0.00659 -0.033874  1.000000
```

如果**只想关注某一个变量与其余变量的相关系数**的话，可以使用corrwith,如下方只关心x1与其余变量的相关系数：

```python
In [95]: df.corrwith(df['x1'])
Out[95]:
x1    1.000000
x2    0.136085
x3    0.037185
dtype: float64
```

数值型数据的协方差矩阵：

```python
In [96]: df.cov()
Out[96]:
          x1         x2          x3
x1  4.005609   0.870124    2.078596
x2  0.870124  10.206447   -0.507512
x3  2.078596  -0.507512  780.090808
```

## 类似于SQL的操作

在SQL中常见的操作主要是增、删、改、查几个动作，那么pandas能否实现对数据的这几项操作呢？答案是Of Course!

### 增：添加新行或增加新列

```python
In [99]: dic = {'Name':['LiuShunxiang','Zhangshan'],
    ...:        'Sex':['M','F'],'Age':[27,23],
    ...:        'Height':[165.7,167.2],'Weight':[61,63]}

In [100]: student2 = pd.DataFrame(dic)

In [101]: student2
Out[101]:
   Age  Height          Name Sex  Weight
0   27   165.7  LiuShunxiang   M      61
1   23   167.2     Zhangshan   F      63
```

现在将student2中的数据新增到student中，可以通过**concat函数**实现：

![g1](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwTIu4ibolcJbCGXJnwicUOgsLwS0zpiazsDrZ6mRLJHCRbiawQAaFxtg2MQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

注意到了吗？在数据库中union必须要求两张表的列顺序一致，而这里**concat函数可以自动对齐两个数据框的变量**（前面student数据框列的顺序是'Name Sex  Age  Height  Weight'，和student2定义的列顺序不同，但是合并时按列名进行了对齐）！

新增列的话，其实在pandas中就更简单了，例如在student2中新增一列学生成绩：

![g2](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwQGicPicpQqU30D8XPtT5qibFB5OnxcYQ6K31yCrbrz8icpSSGYlbGKV6Kg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对于新增的列没有赋值，就会出现空NaN的形式。

补充一下，也可以这样初始化并新增一列：

```python
>>> df = pd.DataFrame({'a':[1,2],'b':[3,4]})
>>> df
   a  b
0  1  3
1  2  4
>>> df['c'] = 0
>>> df
   a  b  c
0  1  3  0
1  2  4  0
```

### 删：删除表、观测行或变量列

删除数据框student2,通过del命令实现，该命令可以删除Python的所有对象。

![g3](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwPgAX4xN3piaeM5KIotujfYSfogWYLuvKVRdqvUpr11oBNttYlScHoJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

#### 删除指定的行

![g4](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwbA70IZKNtKrEBkbvlFr4Xcz8U48HticgSCR7VB0jSRJ8ribQyQs7L17A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

原数据中的第1,2,4,7行的数据已经被删除了。

根据**布尔索引**删除行数据，其实这个删除就是保留删除条件的反面数据，例如删除所有14岁以下的学生：

![g5](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwnvpevZlxcWUf9234l9e3ia5t4utIibKbOUXqw2ibG2maV9uqlF7ziaAkjw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

#### 删除指定的列

![g6](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwECAW5ibeKFaicFZ9NdKdS8tExC0jSz92Wib6Lad5OVdbofnMIByUCUP3g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

我们发现，不论是删除行还是删除列，都可以通过drop方法实现，只需要设定好删除的轴即可，即**调整drop方法中的axis参数。默认该参数为0，表示删除行观测，如果需要删除列变量，则需设置为1**。

### 改：修改原始记录的值

如果发现表中的某些数据错误了，如何更改原来的值呢？我们试试**结合布尔索引和赋值**的方法：

例如发现student3中姓名为Liushunxiang的学生身高错了，应该是173，如何改呢？

![g7](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwRRGbEkt2xNIs4BVMrhVgzqiaDwQqxlOJKR1PW3aYTc1p7zDibvkbUICg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这样就可以把原来的身高修改为现在的170了。

看，关于索引的操作非常灵活、方便吧，就这样轻松搞定数据的更改。

### 查

有关数据查询部分，上面已经介绍过，下面重点讲讲聚合、排序和多表连接操作。

### 聚合

pandas模块中可以通过groupby()函数实现数据的聚合操作

根据性别分组，计算各组别中学生身高和体重的平均值：

![g8](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwWAliaoe6sXiboLVSbicpe2LPvAkzcWQDqmiaB3Srbz9QwagLyTtHNNzNHg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

如果不对原始数据作限制的话，聚合函数会**自动选择数值型数据进行聚合计算**。如果不想对年龄计算平均值的话，就需要剔除改变量：

![g9](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwLXkwoicFG8oEeHbzbkaVibDWeBDUOLVutnFM7SgRtlq3jYGSKT7G00pw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

groupby还可以使用多个分组变量，例如根本年龄和性别分组，计算身高与体重的平均值：

![g10](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwmicaE9Hm0eTIFIqkvNeRvhElSRRsl4FSPP3ibeDDU4y6Bm6WGkLSvvrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

当然，还可以**对每个分组计算多个统计量**：

![g11](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwcgcz9bicCWxRnfu7BibNvUsKamVWOntdrkBKPOrC6rdMyMR2nfKeHRAw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

是不是很简单，只需一句就能完成SQL中的SELECT...FROM...GROUP BY...功能，何乐而不为呢？

### 排序

排序在日常的统计分析中还是比较常见的操作，我们可以使用order、sort_index和sort_values实现序列和数据框的排序工作：

![g12](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQw6YeuvzQqt7FamAryWEprAltcysLCjPBUgh3epdJRicK2c2tmTBaFOFw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

我们再试试降序排序的设置：

![g13](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwOnc7KsnraR3qAMs2dicuFBZRnhqIZRRyia48AjiaKXMs2TUCK4ZZibeLbQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

上面两个结果其实都是按值排序，并且结果中都给出了警告信息，即**建议使用sort_values()函数进行按值排序**。

在数据框中一般都是按值排序，例如：

![g14](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwSOfubia7UEGGPWt1lerH1NzicrAibnrCEHoK8xAicz914AlytFAL7kOOibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 多表连接

多表之间的连接也是非常常见的数据库操作，连接分**内连接**和**外连接**，在数据库语言中通过join关键字实现，pandas我比较建议使用merge函数实现数据的各种连接操作。

如下是构造一张学生的成绩表：

![g15](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwehjHYusfagiaZu5CVMcj1rmXOWrGPiaqGJWkase0OnqyKiaypicQjbEFIQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

现在想把学生表student与学生成绩表score做一个关联，该如何操作呢？

![g16](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQw8qCMoiaMtBYUBbPjscNTGeu9BxgyTdBGeQ0ic9kFKnKp88YEelWulTtw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

注意，**默认情况下，merge函数实现的是两个表之间的内连接，即返回两张表中共同部分的数据**。可以通过how参数设置连接的方式，left为左连接；right为右连接；outer为外连接。

![g17](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwq8S6j6UfSmtd4nmiclU04UFF06ic2oD2Jjicw40loyLAdk6U9nGdF4yvw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

左连接实现的是保留student表中的所有信息，同时将score表的信息与之配对，能配多少配多少，对于**没有配对上的Name，将会显示成绩为NaN**。

## 缺失值处理

现实生活中的数据是非常杂乱的，其中缺失值也是非常常见的，对于缺失值的存在可能会影响到后期的数据分析或挖掘工作，那么我们该如何处理这些缺失值呢？**常用的有三大类方法，即删除法、填补法和插值法**。

#### 删除法

当数据中的**某个变量大部分值都是缺失值**，可以考虑删除改变量（删除列）**当缺失值是随机分布的，且缺失的数量并不是很多是，也可以删除这些缺失的观测（删除行）**。

#### 替补法

对于连续型变量，**如果变量的分布近似或就是正态分布的话，可以用均值替代那些缺失值；如果变量是有偏的，可以使用中位数来代替那些缺失值；对于离散型变量，我们一般用众数去替换那些存在缺失的观测**。

#### 插补法

插补法是基于蒙特卡洛模拟法，结合线性模型、广义线性模型、决策树等方法**计算出来的预测值替换缺失值**。

<br>

我们这里就介绍简单的删除法和替补法：

![g18](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQws6vuy44QJh1bxibn1chNZ7yFp5biaSH66ibpejKEf12pZ4lySs03ZCoGQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这是一组含有缺失值的序列，我们可以**结合sum函数和isnull函数来检测数据中含有多少缺失值**：

```python
In [130]: sum(pd.isnull(s))
Out[130]: 9
```

#### 直接删除缺失值

![g19](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwML3bmaZfeibcqxoq1ib8IZ9l3jPTjrWtvhgs6ib1mmM09qp9KDib6etrSg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

默认情况下，dropna会删除任何含有缺失值的行，我们再构造一个数据框试试：

返回结果表明，数据中只要含有缺失值NaN,该数据行就会被删除，如果使用参数 `how='all'`，则表明只删除所有行为缺失值的观测。

![g20](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwsibqIfEqKKRkLBdUjmKw79aem8wSEq7p1ZVk7l0oHxtIgWNhCksssWg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

补充一个对比例子：

```python
>>> df  = pd.DataFrame({'x1':[0,1,None,3,None],'x2':[None,1,None,None,4],'x3':[0,None,None,3,4]})
>>> df
    x1   x2   x3
0  0.0  NaN  0.0
1  1.0  1.0  NaN
2  NaN  NaN  NaN
3  3.0  NaN  3.0
4  NaN  4.0  4.0

>>> df.dropna(how='all') # 只有全部列都为NaN的行被删掉
    x1   x2   x3
0  0.0  NaN  0.0
1  1.0  1.0  NaN
3  3.0  NaN  3.0
4  NaN  4.0  4.0

>>> df.dropna() # 只要有一列包含NaN就会删除掉
Empty DataFrame
Columns: [x1, x2, x3]
Index: []
```

再补充一下，如果是想删除列，直接用 `DataFrame.drop(['列名1'，'列名2']， axis=1)` 这个语句就可以了。

再补充一下，被别人问问题的时候发现了一个新手容易犯的错误，在初始化时，如果某一个值为空，也即想初始化为NaN，注意不是填'NaN'，这样初始化的话，数据框中对应的元素就是一个字符串而不是空值，自然也就没法使用pandas库提供的缺失值处理函数了，正确的方法是初始化为None。

#### 使用一个常量来填补缺失值

可以使用fillna函数实现简单的填补工作：

##### 1）用0填补所有缺失值

![g21](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwVHrjCgzFXdkuLKazjB7rvnp7vmfSEfGx1BYEkNR4QO2HMLfTJ1omXg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

##### 2)采用前项填充或后向填充

![g22](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwjcPabZCw4lSfZWwPwfNZ0B59zQhQ7p6AsicgHYllJic5k2ib1ibicYpoJibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

补充，使用这种填充方法，如果第一行/最后一行出现缺失值，它们将不被填充。

##### 3)使用常量填充不同的列

![g23](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwOWVEYWnJ5mjkVXs0nZvSFnpZS1O1O9PhicxnXMhnLl9KmvXar28iaXKQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

##### 4)用均值或中位数填充各自的列

![g24](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwLoaicwu7N4ZicMsqUPwKiblWNbmYXw6PlsG1Yh5v8657HbaP82lZhMWgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

很显然，在使用填充法时，相对于常数填充或前项、后项填充，使用各列的众数、均值或中位数填充要更加合理一点，这也是工作中常用的一个快捷手段。

## 数据透视表
在Excel中有一个非常强大的功能就是数据透视表，通过托拉拽的方式可以迅速的查看数据的聚合情况，这里的聚合可以是计数、求和、均值、标准差等。

pandas为我们提供了非常强大的函数pivot_table()，该函数就是实现数据透视表功能的。对于上面所说的一些**聚合函数，可以通过参数aggfunc设定**。我们先看看这个函数的语法和参数吧：

```python
pivot_table(data,values=None,
                   index=None,
                   columns=None,
                   aggfunc='mean',
                   fill_value=None,
                   margins=False,
                   dropna=True,
                   margins_name='All')
```

- **data**：需要进行数据透视表操作的数据框
- **values**：指定需要聚合的字段
- **index**: 指定某些原始变量作为行索引
- **columns**: 指定哪些离散的分组变量
- **aggfunc**: 指定相应的聚合函数，默认为`numpy.mean()`
- **fill_value**：使用一个常数替代缺失值，默认不替换
- **margins**: 是否进行行或列的汇总，默认不汇总
- **dropna**: 默认所有观测为缺失的列
- **margins_name**：默认行汇总或列汇总的名称为'All'

我们仍然以student表为例，来认识一下数据透视表pivot_table函数的用法：

对一个分组变量（Sex），一个数值变量（Height）作统计汇总：

![g25](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwOkHSnJ9b1pknpQhtialtrlYWtOHl3RjGcAlm82dOQkEmtnanH0hpPicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对一个分组变量（Sex），两个数值变量（Height,Weight）作统计汇总：

![g26](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwEvkhXjZcLXZDTyVq8y4knSonHt0vIgNE9xSWiaQ1rlJhwYEoeknBqWA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对两个分组变量（Sex，Age)，两个数值变量（Height,Weight）作统计汇总：

![g27](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwwARphPnKPQgMaDicFcJMb5xZM36joibOicwokjfLiate8lwetpmq0wucTw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

很显然这样的结果并不像Excel中预期的那样，该如何变成列联表的形式的？很简单，只需**将结果进行非堆叠操作（unstack）**即可：

![g29](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwHO4HwBfnD3hOW7YnQicoFVq6QMEB12PdiczUlkN1vqb7DICqewDlXQMg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

看，这样的结果是不是比上面那种看起来更舒服一点？

使用多个聚合函数

![g30](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQw6iaYk8qnZAbTygiaSMfHyM9ZU7vMlUtJTnHy78IdINNWb4VibSCgSHic1w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

有关更多数据透视表的操作，可参考[《Pandas透视表（pivot_table）详解》](http://python.jobbole.com/81212/)一文。

## 多层索引的使用

最后我们再来讲讲pandas中的一个重要功能，那就是多层索引。在序列中它可以实现在一个轴上拥有多个索引，就类似于Excel中常见的这种形式：

![g31](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwhz1bhbkPiaWrZgCz4aRy9TFgYkiboicCbmGRr7J7yLwhFUCFspTKYhHLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对于这样的数据格式有什么好处呢？pandas可以帮我们实现用低维度形式处理高维数数据，这里举个例子也许你就能明白了：

![g32](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwRoJDknN7JMx68wXn8t6OkywCe2GpPo7U7AXTbbwlFNictYqmo6icvHcQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

对于这种多层次索引的序列，取数据就显得非常简单了：

![g33](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwMLicib4RAGgB5NjmXeVicRribQPINgEGuHu5AGOuDmyicRKHmKz8JDFYKow/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

**对于这种多层次索引的序列，我们还可以非常方便的将其转换为数据框的形式**：

![g34](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwq8B44icT2ibH9IX3ewMzqehxbUHJyajdRibPCyyxhic6ibXWxsuoUeN4FLA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

**以上针对的是序列的多层次索引**，数据框也同样有多层次的索引，而且**每条轴上都可以有这样的索引**，就类似于Excel中常见的这种形式：

![g35](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwJPL9IAKqLqeeWRgr4b7uCkPxTDotQia6rw8ia6qKic4GDDzzmbSSZ9N4A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

我们不妨构造一个类似的高维数据框：

![g36](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwVrTuia5lV5R72F3jcfGz18K4Fic1tCmBJVuqtsm1Umc3C6XY9Ymkv6Og/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

同样，数据框中的多层索引也可以非常便捷的取出大块数据：

![g37](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwtiaU3cJJX3D9U0YWFHob0aibRfYibicN9RQibIp5Hr6icibMxxBEZKadsy2DQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

**在数据框中使用多层索引，可以将整个数据集控制在二维表结构中**，这对于数据重塑和基于分组的操作（如数据透视表的生成）比较有帮助。

就拿student二维数据框为例，我们构造一个多层索引数据集：

![g38](http://mmbiz.qpic.cn/mmbiz_png/yjFUicxiaClgWibpwROULeFdlOuXM57CpQwUOxrqzve8p2GiaI1YbTTiawGY3qf5g5FJQZpZ4w7MqZ2trCAKia4JbRmw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

讲到这里，我们关于pandas模块的学习基本完成，其实在掌握了pandas这8个主要的应用方法就可以灵活的解决很多工作中的数据处理、统计分析等任务。有关更多的pandas介绍，可参考[pandas官方文档](http://pandas.pydata.org/pandas-docs/version/0.17.0/whatsnew.html)。

此外，在[Efficient_Pandas_Skills](https://github.com/familyld/learnpython/blob/master/Efficient_Pandas_Skills.md)中收录了Pandas库使用的一些进阶技巧，也可以参考一下。
