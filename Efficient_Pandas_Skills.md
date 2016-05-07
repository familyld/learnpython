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

想了解更多请阅读 Pandas Selecting and Indexing。

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

想了解更多请阅读 Pandas Reference (apply)

## 3. 替换缺失值




