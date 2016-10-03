# 用于搭建推荐系统的Python库

这是一篇发布在[www.faroba.com](http://www.faroba.com/)的小短文，[原文](http://www.faroba.com/2015/12/03/a-python-libraries-for-building-recommender-systems/)是英文版的，这里简单地进行记录和翻译。

> Recommender systems or Collaborative filtering is the process of filtering for information using techniques involving collaboration among multiple agents. Applications of collaborative filtering typically involve very large data sets. Collaborative filtering methods have been applied to many different kinds of data including: sensing and monitoring data, such as in mineral exploration, environmental sensing over large areas or multiple sensors; financial data, such as financial service institutions that integrate many financial sources; or in electronic commerce and web applications where the focus is on user data

推荐系统，或者说协同过滤，是一个使用包括协同算法等技术进行信息过滤的过程。协同过滤的应用一般都涉及到规模非常大的数据集。 协同过滤算法已经应用到了很多不同类型的数据上，比方说：感知和监管数据，包括矿质勘探、大区域/多传感器环境感知；金融数据，包括集成多种资金来源的金融服务机构；电商和网络应用的用户数据。

> Here is the list of python libraries for building recommender systems.

下面是一些可以用于构建推荐系统的Python库。

## [Crab](http://muricoca.github.io/crab/)

> Crab is a flexible, fast recommender engine for Python that integrates classic information filtering recommendation algorithms in the world of scientific Python packages (numpy, scipy, matplotlib). The engine aims to provide a rich set of components from which you can construct a customized recommender system from a set of algorithms. The tutorial is from official documentation of Crab.

Crad是一个灵活，快速的Python推荐引荐，继承了经典的信息过滤推荐算法。它的目标是提供一个丰富的组件集合，使得我们可以用它来构建一个定制化的推荐系统。官方文档里面提供了详细的教程。

这个库其实是Python里面比较有名的推荐系统库了，开源，代码都可以下载到，只是这个库只支持Python2.x版本，有点跟不上现在的潮流了。之前一直挺想读一遍它的源码，好好研究一下的，写一个兼容Python3.x的版本的，应该会有不错的收获。

## [pysuggest 1.0](https://pypi.python.org/pypi/pysuggest/1.0)

> Python wrapper for the SUGGEST, which is a Top-N recommendation engine that implements a variety of recommendation algorithms for collaborative filtering. SUGGEST is a Top-N recommendation engine that implements a variety of recommendation algorithms. Top-N recommender systems, a personalized information filtering technology, are used to identify a set of N items that will be of interest to a certain user. In recent years, top-N recommender systems have been used in a number of different applications such to recommend products a customer will most likely buy; recommend movies, TV programs, or music a user will find enjoyable; identify web-pages that will be of interest; or even suggest alternate ways of searching for information.

这个库是SUGGEST的Python封装，（SUGGEST是）一个实现了多种协同过滤推荐算法的Top-N推荐引擎。Top-N推荐，是一种个性化信息过滤技术，用于识别出特定用户会感兴趣的N件物品。在近几年，top-N推荐系统已经被应用到了很多不同的领域，比方说推荐用户最可能买的产品，推荐电影、电视节目、还有用户最喜欢的音乐，推荐用户感兴趣的网页，甚至在用户使用搜索引擎查询时给出更好的建议。

## [unison-recsys](https://github.com/python-recsys/unison-recsys)

> Unison is the recommendation system behind GroupStreamer, an Android application that recommends music for groups. You can also checkout the source code of the mobile application if you're interested.

Unison是GroupStreamer所使用的推荐系统，GroupStreamer是一个为群组推荐音乐的Android应用。感兴趣的话还可以看看它们手机应用的源码。

## [python-recsys](https://github.com/python-recsys/python-recsys)

> A python library for implementing a recommender system, for documentation and examples click.

一个实现推荐系统的Python库，点入链接可以查看文档和具体的例子。

## [PySpark](https://spark.apache.org/docs/1.4.0/api/python/pyspark.mllib.html#module-pyspark.mllib.recommendation)

> The Spark Python API (PySpark) exposes the Spark programming model to Python.

一个Spark的Python API，可以借助它调用Spark的编程模型。

Spark里面也有很多好玩的处理机器学习问题，处理大数据的组件，希望之后有时间可以多多研究吧~
