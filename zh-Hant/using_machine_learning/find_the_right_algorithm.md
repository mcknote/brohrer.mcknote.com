# 挑選合適的演算法

原文：[**Find an Algorithm that Fits**](https://brohrer.github.io/find_the_right_algorithm.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

![](https://brohrer.github.io/images/shoes.jpg)

挑選機器學習的演算法（machine learning algorithm）就和挑鞋一樣，我們不會只考慮性能，否則我們都應該穿著要價上千元的輕量慢跑鞋；我們會根據使用情況挑選適合的款式：有些鞋子適合用來站一整天，有些鞋子則適合用來爬山。鞋子的價格通常很重要；當然，鞋子的外型可能比什麼都還重要。

這些考量套用在演算法上也是一樣的。雖然模型的預測能力很重要，但這並不是全部。有些演算法很容易解釋，有些演算法很能處理雜訊和資料缺失等問題（譯註一）。演算法所需的執行時間很重要，而且演算法的名稱有時就跟鞋子外型一樣重要——我已經遇過不只一位客戶跟我說，不管我做什麼，分析名稱都得包含「神經網路」（neural network）。

所以挑選機器學習的演算法就和挑鞋一樣，沒有任何一種演算法可以解決所有問題，但有很多種不錯的演算法。以下是一些挑選演算法的訣竅。

## 一、定義問題和蒐集資料

如果讀者不清楚要回答什麼問題，就很難挑選一個好的演算法，所以釐清問題是整個分析流程中最重要的一步。

釐清問題後的下一步，是確保蒐集到的資料足以應付相關分析。這一步通常被稱作「品質保證」（Quality Assurance）或「資料淨化」（Sanitization）；這個步驟並不算非常有趣，但它非常重要。這個步驟的主要目的，是確保我們有精確且相關的資料、妥善處理了資料缺失問題、以及能從這些資料中找出有意義的答案。

想了解更多關於這個步驟的讀者，請參考〈[有效利用資料科學](../using_data/make_data_science_work_for_you.md)〉。

## 二、選擇合適的演算法種類

一旦有了合適的資料和準確的問題以後，讀者就可以放心開始分析了。演算法主要分為幾大類別，根據讀者提的問題，我們可以從這些類別中挑選適當的分析方法。請利用以下〈哪種演算法可以回答我的問題〉中的表格選擇適合特定問題的演算法。如果找不到，讀者或許得發揮創意，試著換個方法提問。

想了解更多關於這個步驟的讀者，請參考〈[哪種演算法可以回答我的問題](https://blogs.technet.microsoft.com/machinelearning/2015/09/01/which-algorithm-family-can-answer-my-question/)〉（英文）和〈[五種可以用機器學習回答的問題](../using_machine_learning/five_questions_data_science_answers.md)〉。

## 三之一、選擇一個演算法（快速版）

一旦選好了演算法種類，最後一步就是選用特定的演算法以分析資料。如果讀者只是想找個不錯的演算法，但還不特別要求找出最佳方法，可以先參考這張精美的[演算法秘笈](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-algorithm-cheat-sheet/)，裡面包含了 [Microsoft Azure Machine Learning](https://studio.azureml.net/) 服務上的一些演算法。

![](https://brohrer.github.io/images/cheat_sheet.png "Azure 演算法秘笈")

## 三之二、選擇一個的演算法（深入版）

使用以上秘笈，就像跑去附近的折扣商店買鞋子一樣，讀者可以快速找到便宜、大小合適的鞋子，然後直接上路。但讀者或許會想多做一點功課，藉由閱讀評論、比較重量和材質、並多試試幾個款式，以找出真正適合自己的鞋子。由於每種演算法對資料的要求和分析性能相差頗大，讀者可以深入了解[如何選擇適合的演算法](https://azure.microsoft.com/en-us/documentation/articles/machine-learning-algorithm-choice/)（英文），以滿足更特定的需求。

當然，為了選出最適合自己的鞋子，我們會先多試幾雙。為了找出最合適的演算法，我們也可以多試幾種方法。如果讀者有足夠的時間和熱忱，找出最佳演算法的不二法門，就是將所有方法都試過一遍。

Brandon，於 2016 年 2 月 4 日。

如果這篇文章對讀者有幫助，我也推薦閱讀[我的部落格](https://brohrer.github.io/blog.html)。雖然我在微軟工作，但這些只是我的個人意見。這篇文章最先發表於[微軟的 TechNet Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/2015/09/22/how-to-find-an-algorithm-that-fit)。文章開頭的圖片來自 [foeoc kannilc](https://www.flickr.com/photos/foeock/7892970836)、[TOONbasist](http://toonbasist.deviantart.com/art/converse-shoes-196298018)、[Ealdgyth](https://commons.wikimedia.org/wiki/File:Paddockboots.jpg) 和 [todrick](https://commons.wikimedia.org/wiki/File:Five_Ten_Anasazi_Verde.jpg)。

## 譯註

1. 在翻譯這段時，我一時能想到幾種比較容易解釋的模型包括線性迴歸（linear regression）和決策樹（decision tree）；比較好處理資料缺失問題的演算法則包括決策樹（decision tree）、隨機森林（random forest）和樸素貝葉斯分類器（naive Bayes classifiers），取決於缺失的情況。也歡迎讀者補充和交流其他演算法的特性。

