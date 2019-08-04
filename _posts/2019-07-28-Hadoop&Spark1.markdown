---
layout: post
title: 大数据处理二剑客之Spark
sub-title: 比Hadoop更快的大数据计算框架
date:   2019-08-04 17:25 +0800
categories: jekyll update
---

进入大数据领域，Hadoop和Spark是最先要了解的两个技术，至少目前看来是这样的。

虽然一直在用它们，但对其没有系统的认识，甚至把二者混为一谈。现在梳理一下。

要了解一项技术，首先思考它是干什么用的？`大数据领域的工作包括"数据处理"和"数据分析"`。数据处理似食材准备，数据分析似烹饪过程。准备食材包括买菜、洗菜、切菜、腌制等过程，为下一步烹饪做准备；数据处理包括数据收集、存储、清洗、转换、组合等动作，方便我们进行下一步数据分析。`Hadoop和Spark就是数据处理这一阶段（大数据分析也有涉及，但以处理为主）的关键两项技术，二者有重合，但各司其职.` 

这篇文章梳理Spark。首先，`一句话描述Spark：最受欢迎的大数据计算框架`。

Spark是加州大学伯克利分校的AMP实验室开发的类似Hadoop MapReduce的通用并行框架，诞生于2009年，2013年成为了Aparch基金项目。Spark是MapReduce的替代方案，而且兼容HDFS、Hive，可融入Hadoop的生态系统，以弥补MapReduce的不足。

# Spark VS Hadoop

[大数据处理二剑客之Hadoop](https://wangqinxiao.github.io/jekyll/update/2019/07/28/Hadoop&Spark.html)一文梳理了Hadoop生态系统，Hadoop已有MapReduce计算框架，为什么又要用Spark作为大数据计算框架呢？

答案是：因为`Spark比Hadoop MapReduce更快`。Spark比MapReduce更快的原因是，Hadoop MapReduce直接读写存储设备硬盘（HDFS），而`Spark基于内存进行计算`。

Spark和Hadoop MaoReduce计算框架一样，是分布式架构。其特点是在数据计算的过程中，把中间结果缓存在内存中。在进行大量数据计算时，直接从内存中读取数据，这要比从硬盘中读取快很多，速度优势明显。

但是，基于内存的计算同样也会带来缺点。`与Hadoop MapReduce相比，Spark的缺点是不稳定`。内存毕竟有限，成本也高，如果数据量过大的话，容易造成内存溢出的问题，从而导致计算过程崩溃。Hadoop MapReduce写的程序虽然慢，但是总会算出结果。而Spark写的程序常常由于数据量过大、内存不够或者计算资源配置不合理，导致崩溃（如Lost stages等等）。笔者在一线使用Spark进行数据处理，程序崩溃是最头疼的事情。

鉴于Spark基于内存计算而导致的速度快的优点和不稳定的缺点。在大数据项目的计算框架技术选型时，需要`综合考虑数据量、业务的时间要求、可用计算资源`。一般在`数据处理阶段（原始数据量一般较大）的过程中用Hadoop MapReduce`，在`数据分析（数据已经过清洗，数据量一般较小）的过程中使用Spark`。

Spark已经融入Hadoop系统，可支持Hadoop Yarn资源管理，HDFS进行数据存储。

# Spark组成部分

Spark的主要组件有：

* SparkCore：Spark核心计算组件，实现分布式计算。它是我们最常用到的组件。

＊ SparkSQL：Spark Sql 是Spark来操作结构化数据的程序包，可以让我使用Hive SQL语句的方式来查询和处理数据。

SparkStreaming： 是Spark提供的对实时数据进行流式计算的组件。

MLlib：提供常用机器学习算法的实现库。

GraphX：提供一个分布式图计算框架，能高效进行图计算。

# Spark相关概念

这部分梳理我们最常用到的Spark Core的运行原理。Spark程序在运行过程中，涉及到的概念包括Application、Driver、Executer、Job、Stage、RDD等。

* Application：编写的Spark应用程序就是一个Application。
* Driver：Driver运行Application的Main()函数并创建SparkContext，即应用程序的运行环境。SparkContext负责分布式Cluster间的通信、任务分配管理等。通常SparkContext即代表Driver。当Executor部分运行完毕后，Driver负责将SparkContext关闭
* Excuter：执行器。一个Excuter代表一个进程，负责计算任务，并将结果存在内存或者磁盘上。Excuter越多，说明进程越多，执行速度也就更快。
* Job：Job是一个计算作业，由一个或着多个任务集组成。一次Spark Action，例如ReduceByKey就会催生一次计算作业，行成一个Job。一个Job包含多个RDD及作用于相应RDD上的各种Operation。
* Stage：Stage是一个任务集对应的调度阶段。每个Job会被拆分很多组Task，每组任务被称为Stage（或者TaskSet）。
* Task：任务被送到某个Executor上的工作任务;单个分区数据集上的最小处理流程单元.
* RDD：弹性分布式数据集（Resilient Distributed Datasets，RDD），Spark的一种数据对象，是Spark的基本计算单元。




参考资料：

https://www.cnblogs.com/xia520pi/p/8609938.html
https://baike.baidu.com/item/SPARK/2229312?fr=aladdin
https://blog.csdn.net/liuxiangke0210/article/details/79687240













