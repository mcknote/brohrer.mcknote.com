# 挑选合适的算法

原文：[**Find an Algorithm that Fits**](https://brohrer.github.io/find_the_right_algorithm.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

![](https://brohrer.github.io/images/shoes.jpg)

挑选机器学习的算法（machine learning algorithm）就和挑鞋一样，我们不会只考虑性能，否则我们都应该穿着要价上千元的轻量慢跑鞋；我们会根据使用情况挑选适合的款式：有些鞋子适合用来站一整天，有些鞋子则适合用来爬山。鞋子的价格通常很重要；当然，鞋子的外型可能比什么都还重要。

这些考量套用在算法上也是一样的。虽然模型的预测能力很重要，但这并不是全部。有些算法很容易解释，有些算法很能处理杂讯和数据缺失等问题（译注一）。算法所需的运行时间很重要，而且算法的名称有时就跟鞋子外型一样重要。我已经遇过不只一位客户跟我说，不管我做什么，分析名称都得包含「神经网络」（neural network）。

所以挑选机器学习的算法就和挑鞋一样，没有任何一种算法可以解决所有问题，但有很多种不错的算法。以下是一些挑选算法的诀窍。

## 一、定义问题和搜集数据

如果读者不清楚要回答什么问题，就很难挑选一个好的算法。厘清问题是整个分析流程中最重要的一步。

厘清问题后的下一步，是确保搜集到的数据足以应付相关分析。这一步通常被称作「品质保证」（Quality Assurance）或「数据净化」（Sanitization）；这个步骤并不是很有趣，但它非常重要。这个步骤的主要目的，是确保我们有精确且相关的数据、妥善处理了数据缺失问题、以及能从这些数据中找出有意义的答案。

想了解更多关于这个步骤的读者，请参考〈[有效利用数据科学](../using_data/make_data_science_work_for_you.md)〉。

## 二、选择合适的算法种类

一旦有了合适的数据和准确的问题以后，读者就可以放心开始分析了。算法主要分为几大类别。根据读者提的问题，我们可以从这些类别中挑选适当的分析方法。请利用以下〈哪种算法可以回答我的问题〉中的表格选择适合特定问题的算法。如果找不到，读者或许得发挥创意，试着换个方法提问。

想了解更多关于这个步骤的读者，请参考〈[哪种算法可以回答我的问题](https://blogs.technet.microsoft.com/machinelearning/2015/09/01/which-algorithm-family-can-answer-my-question/)〉（英文）和〈[五种可以用机器学习回答的问题](../using_machine_learning/five_questions_data_science_answers.md)〉。

## 三之一、选择一个算法（快速版）

一旦选好了算法种类，最后一步就是选用特定的算法以分析数据。如果读者只是想找个不错的算法，但还不特别要求找出最佳方法，可以先参考这张精美的[算法秘笈](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-algorithm-cheat-sheet/)，里面包含了 [Microsoft Azure Machine Learning](https://studio.azureml.net/) 服务上的一些算法。

![](https://brohrer.github.io/images/cheat_sheet.png "Azure 算法秘笈")

## 三之二、选择一个的算法（深入版）

使用以上秘笈，就像跑去附近的折扣商店买鞋子一样，读者可以快速找到便宜、大小合适的鞋子，然后直接上路。但读者或许会想多做一点功课，借由阅读评论、比较重量和材质、并多试试几个款式，以找出真正适合自己的鞋子。由于每种算法对数据的要求和分析性能相差颇大，读者可以深入了解[如何选择适合的算法](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-algorithm-choice/)（英文），以满足更特定的需求。

当然，为了选出最适合自己的鞋子，我们会先多试几双。为了找出最合适的算法，我们也可以多试几种方法。如果读者有足够的时间和热忱，找出最佳算法的不二法门，就是将所有方法都试过一遍。

Brandon，于 2016 年 2 月 4 日。

如果这篇文章对读者有帮助，我也推荐阅读[我的博客](https://brohrer.github.io/blog.html)。虽然我在微软工作，但这些只是我的个人意见。这篇文章最先发表于[微软的 TechNet Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/2015/09/22/how-to-find-an-algorithm-that-fit)。文章开头的图片来自 [foeoc kannilc](https://www.flickr.com/photos/foeock/7892970836)、[TOONbasist](http://toonbasist.deviantart.com/art/converse-shoes-196298018)、[Ealdgyth](https://commons.wikimedia.org/wiki/File:Paddockboots.jpg) 和 [todrick](https://commons.wikimedia.org/wiki/File:Five_Ten_Anasazi_Verde.jpg)。

## 译注

1. 在翻译这段时，我一时能想到几种比较容易解释的模型包括线性回归（linear regression）和决策树（decision tree）；比较好处理数据缺失问题的算法则包括决策树（decision tree）、随机森林（random forest）和朴素贝叶斯分类器（naive Bayes classifiers），取决于缺失的情况。也欢迎读者补充和交流其他算法的特性。

