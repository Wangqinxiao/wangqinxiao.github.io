---
layout: post
title: 朴素贝叶斯算法
sub-title: 易理解、快速的有监督分类算法
date: 2020-03-05 16:46 +0800
categories: jekyll update
---

朴素贝叶斯是一种分类算法，常用于垃圾邮件识别和文本分类。如果想要一个易于理解、快速的分类器，那么朴素贝叶斯会是首选。

朴素贝叶斯基于贝叶斯提出的贝叶斯定理，并且假设样本特征彼此独立，没有相关关系，而这个假设在现实世界中是很不真实的，这很“朴素（NAIVE）”，朴素贝叶斯因此得名。


## 贝叶斯分类应用示例
本例来自阮一峰的[《朴素贝叶斯分类器的应用》](http://www.ruanyifeng.com/blog/2013/12/naive_bayes_classifier.html)

某个医院早上收了六个门诊病人，统计数据如下表：

症状|职业|疾病
--|:--:|--:
打喷嚏|  护士　|　感冒
打喷嚏|  农夫　　|　过敏
头痛|  建筑工人　| 脑震荡
头痛|  建筑工人　| 感冒
打喷嚏|  教师　|　感冒
头痛|  教师　| 脑震荡
  

现在又来了第七个病人，是一个打喷嚏的建筑工人。计算他患上感冒的概率有多大。

根据贝叶斯定理：

`P(A|B) = P(B|A)P(A) / P(B)`

可得：`P(感冒|打喷嚏x建筑工人) = P(打喷嚏x建筑工人|感冒) x P(感冒) / P(打喷嚏x建筑工人)`

假定"打喷嚏"和"建筑工人"这两个特征是独立的，则：`P(感冒|打喷嚏x建筑工人) = P(打喷嚏|感冒) x P(建筑工人|感冒) x P(感冒) / P(打喷嚏) x P(建筑工人) = 0.66 x 0.33 x 0.5 / 0.5 x 0.33 = 0.66`

因此，这个打喷嚏的建筑工人，有66%的概率是得了感冒。同理，可以计算这个病人患上过敏或脑震荡的概率。比较这几个概率，就可以知道他最可能得什么病。

这就是贝叶斯分类器的基本方法：在统计资料的基础上，依据某些特征，计算各个类别的概率，从而实现分类。


## 算法原理
根据概率知识，已知：
![PAB]({{ site.url }}/assets/images/PAB.png)，
![PBA]({{ site.url }}/assets/images/PBA.png)

合并公式，则==>
![BAYES]({{ site.url }}/assets/images/PABfinal.png)，即贝叶斯定理。

贝叶斯定理中，A是标签，B是属性集合。假定B属性集合中的属性（B1,B2,B3...Bi）彼此独立，则，`P(A|B) = P(B1|A) x P(B2|A) x P(B3|A)...x P(Bi|A) x P(A) / P(B1) x P(B2) x P(B3)...x P(Bi)`.

在分类的过程中，由于P(B1) x P(B2) x P(B3)...x P(Bi)的值在求各类的概率时都是一样，所以在计算时可去掉此常数，简化为：`P(A|B) = P(B1|A) x P(B2|A) x P(B3|A)...x P(Bi|A) x P(A)`.

以上公式中各项都可以从训练样本中统计而得。


## 算法优缺点
优点：
1. 计算简单，快速。统计各类别的概率，计算并不复杂。
2. 易理解。计算各个类别的概率，从而实现分类，概率这一概念也容易被人理解。
3. 对离散类型的属性效果很好，比如文本分类。
4. 具有高维数据的良好性能（特征数量很大）。 
5. 可以有效地进行多类预测。
6. 对无关特征不敏感。

缺点：
1. 不太适合处理连续型的属性。需要将连续型数据离散化，效果不一。
2. 特征的独立性不成立。现实生活中，各属性完全独立的情况很少。


---------本文结束


［参考文档］

1. [阮一峰的《朴素贝叶斯分类器的应用》](http://www.ruanyifeng.com/blog/2013/12/naive_bayes_classifier.html)




