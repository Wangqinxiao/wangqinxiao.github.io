---
layout: post
title: 大数据处理二剑客之Hadoop
sub-title: 大数据处理领域的分布式系统基础架构
date:   2019-07-28 15:44 +0800
categories: jekyll update
---

进入大数据领域，Hadoop和Spark是最先要了解的两个技术，至少目前看来是这样的。

虽然一直在用它们，但对其没有系统的认识，甚至把二者混为一谈。现在梳理一下。

要了解一项技术，首先思考它是干什么用的？`大数据领域的工作包括"数据处理"和"数据分析"`。数据处理似食材准备，数据分析似烹饪过程。准备食材包括买菜、洗菜、切菜、腌制等过程，为下一步烹饪做准备；数据处理包括数据收集、存储、清洗、转换、组合等动作，方便我们进行下一步数据分析。`Hadoop和Spark就是数据处理这一阶段（大数据分析也有涉及，但以处理为主）的关键两项技术，二者有重合，但各司其职.` 

这篇文章先梳理Hadoop。首先，`一句话描述Hadoop：大数据分析处理领域的分布式系统基础架构`。Hadoop核心思想是分布式，即将任务分到多台计算机上进行处理。

# 起源：受启发于Google

Hadoop的诞生比Spark要早。可追溯到2004年，两个程序员Doug Cutting和Mike Cafarella受Google Lab 开发的Map/Reduce和Google File System(GFS)的启发，开始实施最初的版本（称为HDFS和MapReduce），最初Hadoop只与网页索引有关，用于处理海量网页数据进行搜索。随后由Apache基金会支持开发，逐渐发展为一个大数据分析处理领域的分布式系统基础架构。 

该项目的创建者，Doug Cutting解释Hadoop的得名 ：“这个名字是我孩子给一个棕黄色的大象玩具命名的“.

Hadoop的主要组成部分是HDFS和MamReduce。

# HDFS：分布式文件系统 
HDFS（Hadoop Distributed File System，Hadoop分布式文件系统，顾名思义是用于存储数据的系统，与传统的分级文件系统（使用目录组织文件）相比，其一样可以增、删、改、查，HDFS的不同之处是分布式存储。

分布式存储指的是存储在HDFS中的文件被分成块。HDFS有两个关键概念：NameNode和DataNode，NameNode可以控制所有文件的操作，DataNode用于存储文件。分布式文件系统设计的优势在于可支持海量数据de的快速存储和查询等操作。

# MapReduce：分布式计算框架
MapReduce，顾名思义其核心是Map（影射）函数和Reduce（化简、规约）函数。

Map函数遍历集合里的每个目标对其应用同一个操作。再用烹饪的例子，在准备食材时需要洗菜，把茄子、辣椒、黄瓜......都洗一遍，逐个清洗这一过程就是Map，集合对象是各种食材，同一操作就是清洗。

Reduce函数遍历集合里的每个目标将其综合称为一个结果。还是烹饪的例子，菜洗好后，开始做大杂烩，锅里加入茄子、加入辣椒、加入黄瓜......最终得到一盆大杂烩。加入各种食材这一过程就是Reduce，将多种食材归约为最重的一个结果———大杂烩菜。

MapReduce这两个核心函数并不新鲜，其内容不仅是这二个函数（其他如Group，Sort等），而是一种编程模型，是一种方法，抽象理论。MapReduce借鉴了语言Lisp的函数式编程设计（什么是函数式编程，新文再议），体现了大数据处理过程中分而治之的思想。分布式是分而治之思想的实践，MapReduce专为分布式计算设计。

除了HDFS和MapReduce，Hadoop还包括以下模块：
* Hadoop Common：支持其他Hadoop模块的通用工具。
* Hadoop YARN：一种作业调度和集群资源管理的框架。

Apache中其他Hadoop相关的项目包括：
* Ambari：一种用于提供、管理和监督Apache Hadoop集群的基于Web UI的且易于使用的Hadoop管理工具。
* Avro：一种数据序列化系统。
* Cassandra：一种无单点故障的可扩展的分布式数据库。
* Chukwa：一种用于管理大型分布式系统的数据收集系统。
* HBase：一种支持存储大型表的结构化存储的可扩展的分布式数据库。
* Hive：一种提供数据汇总和特定查询的数据仓库。
* Mahout：一种可扩展的机器学习和数据挖掘库（Scala语言实现，可结合Spark后端）。
* Pig：一种高级的数据流语言且支持并行计算的执行框架（2017年发布的最新版本0.17.0是添加了Spark上的Pig应* 用）。
* Spark：一种用于Hadoop数据的快速通用计算引擎。Spark提供一种支持广泛应用的简单而易懂的编程模型，包括* ETL（ Extract-Transform-Load）、机器学习、流处理以及图计算。
* Tez：一种建立在Hadoop YARN上数据流编程框架，它提供了一个强大而灵活的引擎来任意构建DAG* （Directed-acyclic-graph）任务去处理用于批处理和交互用例的数据。
* ZooKeeper：一种给分布式应用提供高性能的协同服务系统。

那么问题，既然Hadoop生态家族这么庞大，我们为什么要选择Spark作为对于大数据进行数据分析和数据挖掘的基本计算框架？欲知后事如何，请听下回分解。

[参考资料]：
1. [https://blog.51cto.com/xpleaf/2080181](https://blog.51cto.com/xpleaf/2080181)
2. [https://baike.baidu.com/item/Hadoop/3526507](https://baike.baidu.com/item/Hadoop/3526507)















