# 使效率倍增的Pandas使用技巧

本文取自Analytics Vidhya的一个帖子[12 Useful Pandas Techniques in Python for Data Manipulation](http://www.analyticsvidhya.com/blog/2016/01/12-pandas-techniques-python-data-manipulation/)，浏览原帖可直接点击链接，中文版可参见Datartisan的[用 Python 做数据处理必看：12 个使效率倍增的 Pandas 技巧](http://datartisan.com/article/detail/80.html)。这里主要对帖子内容进行检验并记录有用的知识点。

## 数据集

首先这个帖子用到的数据集是datahack的[贷款预测](http://datahack.analyticsvidhya.com/contest/practice-problem-loan-prediction)(load prediction)竞赛数据集，点击链接可以访问下载页面，如果失效只需要注册后搜索loan prediction竞赛就可以找到了。~~入坑的~~感兴趣的同学可以前往下载数据集，我就不传上来github了。

先简单看一看数据集结构(此处表格可左右拖动)：

| Loan_ID | Gender | Married | Dependents | Education | Self_Employed | ApplicantIncome | CoapplicantIncome | LoanAmount | Loan_Amount_Term | Credit_History | Property_Area | Loan_Status |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|LP001002|Male|No|0|Graduate|No|5849|0||360|1|Urban|Y|
|LP001003|Male|Yes|1|Graduate|No|4583|1508|128|360|1|Rural|N|
|LP001005|Male|Yes|0|Graduate|Yes|3000|0|66|360|1|Urban|Y|
|LP001006|Male|Yes|0|Not Graduate|No|2583|2358|120|360|1|Urban|Y|
|LP001008|Male|No|0|Graduate|No|6000|0|141|360|1|Urban|Y|

共614条数据记录，每条记录有十二个基本属性，一个目标属性(Loan_Status)。数据记录可能包含缺失值。

数据集很小，直接导入Python环境即可：

```python
import pandas as pd
import numpy as np

data = pd.read_csv(r"F:\Datahack_Loan_Prediction\Loan_Prediction_Train.csv", index_col="Loan_ID")
```

注意这里我们指定index_col为Loan_ID这一列，所以程序会把csv文件中Loan_ID这一列作为DataFrame的索引。默认情况下index_col为False，程序自动创建int型索引，从0开始。

## 1. 布尔索引(Boolean Indexing)

有时我们希望基于某些列的条件筛选出需要的记录，这时可以使用loc索引的布尔索引功能。比方说下面的代码筛选出全部无大学学历但有贷款的女性列表：

```python
data.loc[(data["Gender"]=="Female") & (data["Education"]=="Not Graduate") & (data["Loan_Status"]=="Y"), ["Gender","Education","Loan_Status"]]
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Education</th>
      <th>Loan_Status</th>
    </tr>
    <tr>
      <th>Loan_ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LP001155</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP001669</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP001692</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP001908</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002300</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002314</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002407</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002489</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002502</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002534</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002582</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002731</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002757</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>LP002917</th>
      <td>Female</td>
      <td>Not Graduate</td>
      <td>Y</td>
    </tr>
  </tbody>
</table>
</div>

loc索引是优先采用label定位的，label可以理解为索引。前面我们定义了Loan_ID为索引，所以对这个DataFrame使用loc定位时就可以用Loan_ID定位：

```python
data.loc['LP001002']
```

```python
Gender                   Male
Married                    No
Dependents                  0
Education            Graduate
Self_Employed              No
ApplicantIncome          5849
CoapplicantIncome           0
LoanAmount            129.937
Loan_Amount_Term          360
Credit_History              1
Property_Area           Urban
Loan_Status                 Y
LoanAmount_Bin         medium
Loan_Status_Coded           1
Name: LP001002, dtype: object
```

可以看到我们指定了一个Loan_ID，就定位到这个Loan_ID对应的记录。

loc允许四种input方式:

1. 指定一个label;

2. label列表，比如`['LP001002','LP001003','LP001004']`;

3. label切片，比如`'LP001002':'LP001003'`;

4. 布尔数组

我们希望基于列值进行筛选就用到了第4种input方式-布尔索引。**使用()括起筛选条件，多个筛选条件之间使用逻辑运算符`&`,`|`,`~`与或非进行连接**，特别注意，和我们平常使用Python不同，这里用`and`,`or`,`not`是行不通的。

此外，这四种input方式都支持第二个参数，使用一个columns的列表，表示只取出记录中的某些列。上面的例子就是只取出了`Gender`,`Education`,`Loan_Status`这三列，当然，获得的新DataFrame的索引依然是Loan_ID。

想了解更多请阅读 [Pandas Selecting and Indexing](http://pandas.pydata.org/pandas-docs/stable/indexing.html)。

## 2. Apply函数

Apply是一个方便我们处理数据的函数，可以把我们指定的一个函数应用到DataFrame的每一行或者每一列(使用参数axis设定，默认为0，即应用到每一列)。

如果要应用到特定的行和列只需要先提取出来再apply就可以了，比如`data['Gender'].apply(func)`。特别地，这里指定的函数可以是系统自带的，也可以是我们定义的(可以用匿名函数)。比如下面这个例子统计每一列的缺失值个数：

```python
# 创建一个新函数:

def num_missing(x):

  return sum(x.isnull())

# Apply到每一列:
print("Missing values per column:")

print(data.apply(num_missing, axis=0)) # axis=0代表函数应用于每一列
```

```python
Missing values per column:
Gender               13
Married               3
Dependents           15
Education             0
Self_Employed        32
ApplicantIncome       0
CoapplicantIncome     0
LoanAmount           22
Loan_Amount_Term     14
Credit_History       50
Property_Area         0
Loan_Status           0
dtype: int64
```

下面这个例子统计每一行的缺失值个数，因为行数太多，所以使用head函数仅打印出DataFrame的前5行：

```python
# Apply到每一行:
print("\nMissing values per row:")

print(data.apply(num_missing, axis=1).head()) # axis=1代表函数应用于每一行
```

```python
Missing values per row:
Loan_ID
LP001002    1
LP001003    0
LP001005    0
LP001006    0
LP001008    0
dtype: int64
```

想了解更多请阅读 [Pandas Reference (apply)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html#pandas.DataFrame.apply)

## 3. 替换缺失值

一般来说我们会把某一列的缺失值替换为所在列的平均值/众数/中位数。`fillna()`函数可以帮我们实现这个功能。但首先要从scipy库导入获取统计值的函数。

```python
# 首先导入一个寻找众数的函数：

from scipy.stats import mode

GenderMode = mode(data['Gender'].dropna())
print(GenderMode)
print(GenderMode.mode[0],':',GenderMode.count[0])
```

```python
ModeResult(mode=array(['Male'], dtype=object), count=array([489]))
Male : 489

F:\Anaconda3\lib\site-packages\scipy\stats\stats.py:257: RuntimeWarning: The input array could not be properly checked for nan values. nan values will be ignored.
  "values. nan values will be ignored.", RuntimeWarning)
```

可以看到对DataFrame的某一列使用mode函数可以得到这一列的众数以及它所出现的次数，由于众数可能不止一个，所以众数的结果是一个列表，对应地出现次数也是一个列表。可以使用`.mode`和`.count`提取出这两个列表。

**特别留意**，可能是版本原因，我使用的scipy (0.17.0)不支持原帖子中的代码，直接使用`mode(data['Gender'])`是会报错的:

```python
F:\Anaconda3\lib\site-packages\scipy\stats\stats.py:257: RuntimeWarning: The input array could not be p
roperly checked for nan values. nan values will be ignored.
  "values. nan values will be ignored.", RuntimeWarning)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "F:\Anaconda3\lib\site-packages\scipy\stats\stats.py", line 644, in mode
    scores = np.unique(np.ravel(a))       # get ALL unique values
  File "F:\Anaconda3\lib\site-packages\numpy\lib\arraysetops.py", line 198, in unique
    ar.sort()
TypeError: unorderable types: str() > float()
```

必须使用`mode(data['Gender'].dropna())`，传入dropna()处理后的DataFrame才可以。虽然Warning仍然存在，但是可以执行得到结果。Stackoverflow里有这个问题的讨论：[Scipy Stats Mode function gives Type Error unorderable types](http://stackoverflow.com/questions/36239953/scipy-stats-mode-function-gives-type-error-unorderable-types-str-float)。指出scipy的mode函数无法处理列表中包含混合类型的情况，比方说上面的例子就是包含了缺失值NAN类型和字符串类型，所以无法直接处理。

同时也指出Pandas自带的mode函数是可以处理混合类型的，我测试了一下：

```python
from pandas import Series
Series.mode(data['Gender'])
```

```python
0    Male
dtype: object
```

确实没问题，不需要使用dropna()处理，但是只能获得众数，没有对应的出现次数。返回结果是一个Pandas的Series对象。可以有多个众数，索引是int类型，从0开始。

掌握了获取众数的方法后就可以使用fiilna替换缺失值了：

```python
#值替换:
data['Gender'].fillna(GenderMode.mode[0], inplace=True)

MarriedMode = mode(data['Married'].dropna())
data['Married'].fillna(MarriedMode.mode[0], inplace=True)

Self_EmployedMode = mode(data['Self_Employed'].dropna())
data['Self_Employed'].fillna(Self_EmployedMode.mode[0], inplace=True)
```

先提取出某一列，然后用fillna把这一列的缺失值都替换为计算好的平均值/众数/中位数。inplace关键字用于指定是否直接对这个对象进行修改，默认是False，如果指定为True则直接在对象上进行修改，其他地方调用这个对象时也会收到影响。这里我们希望修改直接覆盖缺失值，所以指定为True。

```python
#再次检查缺失值以确认:
print(data.apply(num_missing, axis=0))
```

```python
Gender                0
Married               0
Dependents           15
Education             0
Self_Employed         0
ApplicantIncome       0
CoapplicantIncome     0
LoanAmount           22
Loan_Amount_Term     14
Credit_History       50
Property_Area         0
Loan_Status           0
dtype: int64
```

可以看到`Gender`,`Married`,`Self_Employed`这几列的缺失值已经都替换成功了，所以缺失值的个数为0。

想了解更多请阅读 [Pandas Reference (fillna)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.fillna.html#pandas.DataFrame.fillna)

## 4. 透视表

[透视表](http://baike.baidu.com/view/1709397.htm)(Pivot Table)是一种交互式的表，可以动态地改变它的版面布置，以便按照不同方式分析数据，也可以重新安排行号、列标、页字段，比如下面的Excel透视表：

![Excel透视表](http://img0.pconline.com.cn/pconline/1202/22/2681570_shujvtoushi.jpg)

可以自由选择用来做行号和列标的属性，非常便于我们做不同的分析。Pandas也提供类似的透视表的功能。例如`LoanAmount`这个重要的列有缺失值。我们不希望直接使用整体平均值来替换，这样太过笼统，不合理。

这时可以用先根据 `Gender`、`Married`、`Self_Employed`分组后，再按各组的均值来分别替换缺失值。每个组 `LoanAmount`的均值可以用如下方法确定：

```python
#Determine pivot table

impute_grps = data.pivot_table(values=["LoanAmount"], index=["Gender","Married","Self_Employed"], aggfunc=np.mean)

print(impute_grps)
```

```python
                              LoanAmount
Gender Married Self_Employed
Female No      No             114.691176
               Yes            125.800000
       Yes     No             134.222222
               Yes            282.250000
Male   No      No             129.936937
               Yes            180.588235
       Yes     No             153.882736
               Yes            169.395833
```

关键字`values`用于指定要使用集成函数(字段`aggfunc`)计算的列，选填，不进行指定时默认对所有index以外符合集成函数处理类型的列进行处理，比如这里使用`mean`作集成函数，则合适的类型就是数值类型。`index`是行标，可以是多重索引，处理时会按序分组。

想了解更多请阅读 [Pandas Reference (Pivot Table)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.pivot_table.html#pandas.DataFrame.pivot_table)

## 5. 多重索引

不同于DataFrame对象，可以看到上一步得到的Pivot Table每个索引都是由三个值组合而成的，这就叫做多重索引。

从上一步中我们得到了每个分组的平均值，接下来我们就可以用来替换`LoanAmount`字段的缺失值了：

```python
#只在带有缺失值的行中迭代：

for i,row in data.loc[data['LoanAmount'].isnull(),:].iterrows():

  ind = tuple([row['Gender'],row['Married'],row['Self_Employed']])

  data.loc[i,'LoanAmount'] = impute_grps.loc[ind].values[0]
```

特别注意对Pivot Table使用loc定位的方式，Pivot Table的索引是一个tuple。从上面的代码可以看到，先把DataFrame中所有`LoanAmount`字段缺失的数据记录取出，并且使用iterrows函数转为一个按行迭代的对象，每次迭代返回一个索引(DataFrame里的索引)以及对应行的内容。然后从行的内容中把 `Gender`、`Married`、`Self_Employed`三个字段的值提取出放入一个tuple里，这个tuple就可以用作前面定义的Pivot Table的索引了。

接下来的赋值对DataFrame使用loc定位根据索引定位，并且只抽取`LoanAmount`字段，把它赋值为在Pivot Table中对应分组的平均值。最后检查一下，这样我们又处理好一个列的缺失值了。

```python
#再次检查缺失值以确认：

print(data.apply(num_missing, axis=0))
```

```python
Gender                0
Married               0
Dependents           15
Education             0
Self_Employed         0
ApplicantIncome       0
CoapplicantIncome     0
LoanAmount            0
Loan_Amount_Term     14
Credit_History       50
Property_Area         0
Loan_Status           0
dtype: int64
```

## 6. 二维表

二维表这个东西可以帮助我们快速验证一些基本假设，从而获取对数据表格的初始印象。例如，本例中`Credit_History`字段被认为对会否获得贷款有显著影响。可以用下面这个二维表进行验证：

```python
pd.crosstab(data["Credit_History"],data["Loan_Status"],margins=True)
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Loan_Status</th>
      <th>N</th>
      <th>Y</th>
      <th>All</th>
    </tr>
    <tr>
      <th>Credit_History</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>82</td>
      <td>7</td>
      <td>89</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>97</td>
      <td>378</td>
      <td>475</td>
    </tr>
    <tr>
      <th>All</th>
      <td>192</td>
      <td>422</td>
      <td>614</td>
    </tr>
  </tbody>
</table>
</div>

`crosstab`的第一个参数是index，用于行的分组；第二个参数是columns，用于列的分组。代码中还用到了一个`margins`参数，这个参数默认为False，启用后得到的二维表会包含汇总数据。

然而，相比起绝对数值，百分比更有助于快速了解数据。我们可以用apply函数达到目的：

```python
def percConvert(ser):
  return ser/float(ser[-1])

pd.crosstab(data["Credit_History"],data["Loan_Status"],margins=True).apply(percConvert, axis=1)
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Loan_Status</th>
      <th>N</th>
      <th>Y</th>
      <th>All</th>
    </tr>
    <tr>
      <th>Credit_History</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>0.921348</td>
      <td>0.078652</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>0.204211</td>
      <td>0.795789</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>All</th>
      <td>0.312704</td>
      <td>0.687296</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>

从这个二维表中我们可以看到有信用记录 (`Credit_History`字段为1.0) 的人获得贷款的可能性更高。接近80%有信用记录的人都 获得了贷款，而没有信用记录的人只有大约8% 获得了贷款。令人惊讶的是，如果我们直接利用信用记录进行训练集的预测，在614条记录中我们能准确预测出460条记录 (不会获得贷款+会获得贷款：82+378) ，占总数足足75%。不过呢~在训练集上效果好并不代表在测试集上效果也一样好。有时即使提高0.001%的准确度也是相当困难的，这就是我们为什么需要建立模型进行预测的原因了。

想了解更多请阅读 [Pandas Reference (crosstab)](http://pandas.pydata.org/pandas-docs/version/0.17.0/generated/pandas.crosstab.html)

## 7. 数据框合并

就像数据库有多个表的连接操作一样，当数据来源不同时，会产生把不同表格合并的需求。这里假设不同的房产类型有不同的房屋均价数据，定义一个新的表格，如下：

```python
prop_rates = pd.DataFrame([1000, 5000, 12000], index=['Rural','Semiurban','Urban'],columns=['rates'])

prop_rates
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>1000</td>
    </tr>
    <tr>
      <th>Semiurban</th>
      <td>5000</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>12000</td>
    </tr>
  </tbody>
</table>
</div>

农村房产均价只用1000，城郊这要5000，城镇内房产比较贵，均价为12000。我们获取到这个数据之后，希望把它连接到原始表格Loan_Prediction_Train.csv中以便观察房屋均价对预测的影响。在原始表格中有一列Property_Area就是表明贷款人居住的区域的，可以通过这一列进行表格连接：

```python
data_merged = data.merge(right=prop_rates, how='inner',left_on='Property_Area',right_index=True, sort=False)

data_merged.pivot_table(values='Credit_History',index=['Property_Area','rates'], aggfunc=len)
```

```python
Property_Area  rates
Rural          1000     179.0
Semiurban      5000     233.0
Urban          12000    202.0
Name: Credit_History, dtype: float64
```

使用merge函数进行连接,解析一下各个参数：

- 参数right即连接操作右端表格；
- 参数how指示连接方式，默认是inner，即内连接。可选left、right、outer、inner；

    * left: use only keys from left frame (SQL: left outer join)
    * right: use only keys from right frame (SQL: right outer join)
    * outer: use union of keys from both frames (SQL: full outer join)
    * inner: use intersection of keys from both frames (SQL: inner join)

- 参数left_on用于指定连接的key的列名，即使key在两个表格中的列名不同，也可以通过left_on和right_on参数分别指定。 如果一样的话，使用on参数就可以了。可以是一个标签(单列)，也可以是一个列表（多列）；

- right_index默认为False，设置为True时会把连接操作右端表格的索引作为连接的key。同理还有left_index；

- sort参数默认为False，指示是否需要按key排序。

所以上面的代码是把data表格和prop_rates表格连接起来。连接时，data表格用于连接的key是`Property_Area`，而prop_rates表格用于连接的key是索引，它们的值域是相同的。

连接之后使用了第四小节透视表的方法检验新表格中`Property_Area`字段和`rates`字段的关系。后面跟着的数字表示出现的次数。

想了解更多请阅读 [Pandas Reference (merge)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html#pandas.DataFrame.merge)

## 8. 给数据排序

Pandas可以轻松地基于多列进行排序，方法如下：

```python
data_sorted = data.sort_values(['ApplicantIncome','CoapplicantIncome'], ascending=False)

data_sorted[['ApplicantIncome','CoapplicantIncome']].head(10)
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ApplicantIncome</th>
      <th>CoapplicantIncome</th>
    </tr>
    <tr>
      <th>Loan_ID</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>LP002317</th>
      <td>81000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP002101</th>
      <td>63337</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP001585</th>
      <td>51763</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP001536</th>
      <td>39999</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP001640</th>
      <td>39147</td>
      <td>4750.0</td>
    </tr>
    <tr>
      <th>LP002422</th>
      <td>37719</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP001637</th>
      <td>33846</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP001448</th>
      <td>23803</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>LP002624</th>
      <td>20833</td>
      <td>6667.0</td>
    </tr>
    <tr>
      <th>LP001922</th>
      <td>20667</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>

**Notice**：Pandas 的`sort`函数现在已经不推荐使用，使用 `sort_values`函数代替。

这里传入了`ApplicantIncome`和`CoapplicantIncome`两个字段用于排序，Pandas会先按序进行。先根据`ApplicantIncome`进行排序，**对于`ApplicantIncome`相同的记录再根据`CoapplicantIncome`进行排序。** ascending参数设为False，表示降序排列。不妨再看个简单的例子：

```python
from pandas import DataFrame
df_temp = DataFrame({'a':[1,2,3,4,3,2,1],'b':[0,1,1,0,1,0,1]})
df_sorted = df_temp.sort_values(['a','b'],ascending=False)
df_sorted
```

<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

这里只有a和b两列，可以清晰地看到Pandas的多列排序是先按a列进行排序，a列的值相同则会再按b列的值排序。

想了解更多请阅读 [Pandas Reference (sort_values)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html#pandas.DataFrame.sort_values)

## 9. 绘图（箱型图&直方图）

Pandas除了表格操作之外，还可以直接绘制箱型图和直方图且只需一行代码。这样就不必单独调用matplotlib了。

```python
%matplotlib inline
# coding:utf-8
data.boxplot(column="ApplicantIncome",by="Loan_Status")
```

![箱型图1](https://raw.githubusercontent.com/familyld/learnpython/master/graph/pandas_box_1.png)

###箱型图

因为之前没怎么接触过箱型图，所以这里单独开一节简单归纳一下。

箱形图（英文：Box-plot），又称为盒须图、盒式图、盒状图或箱线图，是一种用作显示一组数据分散情况资料的统计图。因型状如箱子而得名。详细解析看[维基百科](https://zh.wikipedia.org/wiki/%E7%AE%B1%E5%BD%A2%E5%9C%96)。

因为上面那幅图不太容易看，用个简单点的例子来说，还是上一小节那个只有a列和b列的表格。按b列对a列进行分组：

```python
df_temp.boxplot(column="a",by="b")
```
![箱型图2](https://raw.githubusercontent.com/familyld/learnpython/master/graph/pandas_box_3.png)

定义b列值为0的分组为Group1，b列值为1的分组为Group2。Group1分组有4，2，1三个值，毫无疑问最大值4，最小值1，在箱型图中这两个值对应箱子发出的**虚线顶端的两条实线**。Group2分组有3，3，2，1四个值，由于最大值3和上四分位数3(箱子顶部)相同，所以重合了。

Group1中位数是2，而Group2的中位数则是中间两个数2和3的平均数，也即2.5。在箱型图中由箱子中间的有色线段表示。

###四分位数

[四分位数](https://zh.wikipedia.org/wiki/%E5%9B%9B%E5%88%86%E4%BD%8D%E6%95%B0)是统计学的一个概念。把所有数值由小到大排列好，然后分成四等份，处于三个分割点位置的数值就是四分位数，其中：

- **第一四分位数** (Q1)，又称“**较小四分位数**”或“**下四分位数**”，等于该样本中所有数值由小到大排列后，在四分之一位置的数。
- **第二四分位数** (Q2)，又称“**中位数**”，等于该样本中所有数值由小到大排列后，在二分之一位置的数。
- **第三四分位数** (Q3)，又称“**较大四分位数**”或“**上四分位数**”，等于该样本中所有数值由小到大排列后，在四分之三位置的数。

**Notice：**Q3与Q1的差距又称**四分位距**（InterQuartile Range, IQR）。

计算四分位数时首先计算位置，假设有n个数字，则：

- Q1位置 = (n-1) / 4
- Q2位置 = 2 * (n-1) / 4 = (n-1) / 2
- Q3位置 = 3 * (n-1) / 4

如果n-1恰好是4的倍数，那么数列中对应位置的就是各个四分位数了。但是，**如果n-1不是4的倍数呢**？

这时位置会是一个带小数部分的数值，四分位数以**距离该值最近的两个位置的加权平均值求出**。其中，距离较近的数，权值为小数部分；而距离较远的数，权值为(1-小数部分)。

再看例子中的Group1，Q1位置为0.5，Q2位置为1，Q3位置为1.5。（**注意：位置从下标0开始！**），所以：

    Q1 = 0.5*1+0.5*2 = 1.5
    Q2 = 2
    Q3 = 0.5*2+0.5*4 = 3

而Group2中，Q1位置为0.75，Q2位置为1.5，Q3位置为2.25。

    Q1 = 0.25*1+0.72*2 = 1.75
    Q2 = 0.5*2+0.5*3 = 2.5
    Q3 = 0.25*3+0.75*3 = 3

这样是否就清晰多了XD  然而，**四分位数的取法还存在分歧**，定义不一，我在学习这篇文章时也曾经很迷茫，**直到阅读了源码！！**

因为Pandas库依赖numpy库，所以它计算四分位数的方式自然也是使用了numpy库的。而**numpy中实现计算百分比数的函数为percentile**，代码实现如下：

```python
def percentile(N, percent, key=lambda x:x):
    """
    Find the percentile of a list of values.

    @parameter N - is a list of values. Note N MUST BE already sorted.
    @parameter percent - a float value from 0.0 to 1.0.
    @parameter key - optional key function to compute value from each element of N.

    @return - the percentile of the values
    """
    if not N:
        return None
    k = (len(N)-1) * percent
    f = math.floor(k)
    c = math.ceil(k)
    if f == c:
        return key(N[int(k)])
    d0 = key(N[int(f)]) * (c-k)
    d1 = key(N[int(c)]) * (k-f)
    return d0+d1
```

读一遍源码之后就更加清晰了。最后举个例子：

```python
from numpy import percentile, mean, median
temp = [1,2,4] # 数列包含3个数
print(min(temp))
print(percentile(temp,25))
print(percentile(temp,50))
print(percentile(temp,75))
print(max(temp))

1
1.5
2.0
3.0
4

temp2 = [1,2,3,3] # 数列包含4个数
print(min(temp2))
print(percentile(temp2,25))
print(percentile(temp2,50))
print(percentile(temp2,75))
print(max(temp2))

1
1.75
2.5
3.0
3

temp3 = [1,2,3,4,5] # 数列包含5个数
print(min(temp2))
print(percentile(temp3,25))
print(percentile(temp3,50))
print(percentile(temp3,75))
print(max(temp3))

1
2.0
3.0
4.0
5
```

不熟悉的话就再手撸一遍！！！不要怕麻烦！！！这一小节到此Over~

###直方图

```python
data.hist(column="ApplicantIncome",by="Loan_Status",bins=30)
```

![直方图](https://raw.githubusercontent.com/familyld/learnpython/master/graph/pandas_hist.png)

直方图比箱型图熟悉一些，这里就不详细展开了。结合箱型图和直方图，我们可以看出获得贷款的人和未获得贷款的人没有明显的收入差异，也即收入不是决定性因素。

###离群点

特别地，从直方图上我们可以看出这个数据集在收入字段上，比较集中于一个区间，区间外部有些散落的点，这些点我们称为离群点(Outlier)。前面将箱型图时没有细说，现在再回顾一下箱型图：

![箱型图1](https://raw.githubusercontent.com/familyld/learnpython/master/graph/pandas_box_1.png)

可以看到除了箱子以及最大最小值之外，还有很多横线，这些横线其实就表示离群点。那么可能又会有新的疑问了？**怎么会有离群点在最大最小值外面呢**？这样岂不是存在比最大值大，比最小值小的情况了吗？

其实之前提到一下，有一个概念叫四分位距（IQR），数值上等于Q3-Q1，记作ΔQ。定义：

- **最大值区间**： Q3+1.5ΔQ
- **最小值区间**： Q1-1.5ΔQ

也就是说最大值必须出现在这两个区间内，**区间外的值被视为离群点**，并显示在图上。这样做我们可以避免被过分偏离的数据点带偏，更准确地观测到数据的真实状况，或者说普遍状况。

想了解更多请阅读 [Pandas Reference (hist)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.hist.html#pandas.DataFrame.hist) | [Pandas Reference (boxplot)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.boxplot.html#pandas.DataFrame.boxplot)

##10. 用Cut函数分箱

有时把数值聚集在一起更有意义。例如，如果我们要为交通状况（路上的汽车数量）根据时间（分钟数据）建模。具体的分钟可能不重要，而时段如“上午”“下午”“傍晚”“夜间”“深夜”更有利于预测。如此建模更直观，也能避免过度拟合。
这里我们定义一个简单的、可复用的函数，轻松为任意变量分箱。

```python
#分箱:

def binning(col, cut_points, labels=None):

  #Define min and max values:

  minval = col.min()

  maxval = col.max()

  #利用最大值和最小值创建分箱点的列表

  break_points = [minval] + cut_points + [maxval]

  #如果没有标签，则使用默认标签0 ... (n-1)

  if not labels:

    labels = range(len(cut_points)+1)

  #使用pandas的cut功能分箱

  colBin = pd.cut(col,bins=break_points,labels=labels,include_lowest=True)

  return colBin



#为年龄分箱:

cut_points = [90,140,190]

labels = ["low","medium","high","very high"]

data["LoanAmount_Bin"] = binning(data["LoanAmount"], cut_points, labels)

print(pd.value_counts(data["LoanAmount_Bin"], sort=False))


low          104
medium       273
high         146
very high     91
dtype: int64
```

解析以下这段代码，首先定义了一个cut_points列表，里面的三个点用于把`LoanAmount`字段分割成四个段，对应地，定义了labels列表，四个箱子(段)按贷款金额分别为低、中、高、非常高。然后把用于分箱的`LoanAmount`字段，cut_points，labels传入定义好的binning函数。

binning函数中，首先拿到分箱字段的最小值和最大值，把这两个点加入到break_points列表的一头一尾，这样用于切割的所有端点就准备好了。如果labels没有定义就默认按0~段数-1命名箱子。 最后借助pandas提供的cut函数进行分箱。

cut函数原型是`cut(x, bins, right=True, labels=None, retbins=False, precision=3, include_lowest=False)`，x是分箱字段，bins是区间端点，right指示区间右端是否闭合，labels不解释..retbins表示是否需要返回bins，precision是label存储和显示的精度，include_lowest指示第一个区间的左端是否闭合。

在把返回的列加入到data后，使用value_counts函数，统计了该列的各个离散值出现的次数，并且指定不需要对结果进行排序。

想了解更多请阅读 [Pandas Reference (cut)](http://pandas.pydata.org/pandas-docs/version/0.17.1/generated/pandas.cut.html)

## 11.为分类变量编码

有时，我们会面对要改动分类变量的情况。原因可能是：

有些算法（如logistic回归）要求所有输入项目是数字形式。所以分类变量常被编码为0, 1….(n-1)
有时同一个分类变量可能会有两种表现方式。如，温度可能被标记为“High”， “Medium”， “Low”，“H”， “low”。这里 “High” 和 “H”都代表同一类别。同理， “Low” 和“low”也是同一类别。但Python会把它们当作不同的类别。
**一些类别的频数非常低，把它们归为一类是个好主意**。

这里我们定义了一个函数，以字典的方式输入数值，用‘replace’函数进行编码

```python
#使用Pandas replace函数定义新函数：

def coding(col, codeDict):

  colCoded = pd.Series(col, copy=True)

  for key, value in codeDict.items():

    colCoded.replace(key, value, inplace=True)

  return colCoded


#把贷款状态LoanStatus编码为Y=1, N=0:

print('Before Coding:')

print(pd.value_counts(data["Loan_Status"]))

data["Loan_Status_Coded"] = coding(data["Loan_Status"], {'N':0,'Y':1})

print('\nAfter Coding:')

print(pd.value_counts(data["Loan_Status_Coded"]))


Before Coding:
Y    422
N    192
Name: Loan_Status, dtype: int64

After Coding:
1    422
0    192
Name: Loan_Status_Coded, dtype: int64
```

在coding函数中第二个参数是一个dict，定义了需要编码的字段中每个值对应的编码。
实现上首先把传入的列转换为Series对象，copy参数表示是否要进行复制，关于copy可以看我另一篇笔记：[深拷贝与浅拷贝的区别](https://github.com/familyld/learnpython/blob/master/Difference_between_DeepCopy_and_ShallowCopy.md)。

copy得到的Series对象不会影响到原本DataFrame传入的列，所以可以放心修改，这里replace函数中的inplace参数表示是否直接在调用对象上作出更改，我们选择是，那么colCoded对像在调用replace后就会被修改为编码形式了。最后，把编码后的列加入到data中，比较编码前后的效果。

## 12. 在一个数据框的各行循环迭代


