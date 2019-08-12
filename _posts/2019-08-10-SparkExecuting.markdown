---
layout: post
title: Spark程序运行过程
sub-title: 了解Spark程序崩溃的原因
date:   2019-08-10 16:30 +0800
categories: jekyll update
---

笔者在一线碰到过很多次Spark程序崩溃的问题，而且解决起来比较费劲。纠其根本原因在于开发人员在编写Spark程序时只注重功能实现，不了解Spark程序运行过程，程序性能不佳，导致现场运行问题频发。而解决问题的过程中，由于不了解运行过程，只能靠各种尝试，解决问题效率极低。

总之，不了解Spark运行原理，就没法写出可靠的Spark程序。

现在就用一段Spark代码实例来一步一步解析Spark运行过程。

{% highlight python %}
# Spark program
Val lines1 = sc.textFile(inputPath1). map(···)). map(···)
Val lines2 = sc.textFile(inputPath2) . map(···)
Val lines3 = sc.textFile(inputPath3)
Val dtinone1 = lines2.union(lines3)
Val dtinone = lines1.join(dtinone1)
dtinone.saveAsTextFile(···)
dtinone.filter(···).foreach(···)
{% endhighlight %}

程序运行过程如下：

# 1. 资源配置
资源分配不由Spark程序决定，而是由资源管理管理器决定，如Hadoop Yarn。大数据计算需要大量硬件计算资源，资源分配决定着程序的运行性能。运行Spark程序前需要保证已配置足够可用的计算资源。资源配置参数包括，资源队列、Driver内存、Executor内存、Executor核数等等。

资源配置是Spark程序运行前的准备工作，本文暂且不做深入探讨。

# 2. 创建SparkContext
运行Spark程序后，第一步Driver进程启动并创建SparkContext，即该Spark程序的专属运行环境。SparkContext负责资源的申请，任务的分配和管理。相当于Spark程序运行过程中的管理者。以下步骤，除了计算，其他几乎都有SparkContext来负责。

当程序运行完成后，Driver会关闭SparkContext。

# 3. 启动Executor
SparkContext再创建完成后，会向资源管理器申请Executor资源，并启动Executor。

# 4. 创建DAG图
环境和资源一切就绪之后。SparkContext会根据Spark程序创建DAG（Directed Acycle graph，有向无环图），即反应所有RDD之间依赖关系的图。

本文示例的程序DAG图大致如下：（本图比较简易，默认RDD没有分区。基本元素单位应该是是RDD的分区）

![DAG]({{ site.url }}/assets/images/DAG.jpg)

# 3. 触发Job
根据DAG图，一次RDD的Action触发一次Job（计算作业）。例如，示例代码中的saveAsTextFile就会触发一次Job。该Job包括对应RDD上的各种操作。


# 4. 分配Stage（TaskSet）
Job可进一步分为Stage。分配原则为根据DAG图，Job流程从后向前，遇到宽依赖，则将当前的流程分为一个Stage。各个Stage一般并非只有线性关系，还有嵌套、并行关系。

本文代码例子的Stag划分如图：

![JOB]({{ site.url }}/assets/images/JOB.jpg)

一个Stage对应一个TaskSet，即包含多个Task的集合。每个Task对于一个RDD（分区）的操作。

![TASK]({{ site.url }}/assets/images/TASK.jpg)

# 6. 提交Stage（TaskSet）
提交Stage，也就是提交TaskSet。DAGScheduler通过TaskScheduler接口提交TaskSet给各个Executor，并以多线程的形式执行。

![TASK-SET]({{ site.url }}/assets/images/TASK-SET.jpg)

# 7. 执行Task
TaskScheduler构建一个TaskSetManager的实例来管理一个TaskSet的生命周期，跟踪每一个task，如果task失败，负责重试task直到达到task重试次数的最多次数。

一个TaskSet在Executor中执行结束后，其结果会返回给DAGScheduler。如果得到TaskSet执行失败的信息，则会重新动态分配该Task到其他节点执行，直到重试次数的最多次数（根据笔者经验，应该是默认4次，如果继续失败，则程序崩溃）。

[参考资料]：

1. [https://blog.csdn.net/liuxiangke0210/article/details/79687240](https://blog.csdn.net/liuxiangke0210/article/details/79687240)
