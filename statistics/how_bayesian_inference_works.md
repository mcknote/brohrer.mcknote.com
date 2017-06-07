# 貝葉斯推斷的運作原理

原文：[**How Bayesian inference works**](http://brohrer.github.io/how_bayesian_inference_works.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

* [Google 簡報上的投影片](https://docs.google.com/presentation/d/1325yenZP_VdHoVj-tU0AnbQUxFwb8Fl1VdyAAUxEzfg/edit?usp=sharing)

**貝葉斯推斷**（Bayesian Inference）是一套可以用來精進預測的方法，在資料不是很多、又想盡量發揮預測能力時特別有用。

雖然有些人會抱著敬畏的心情看待貝葉斯推斷，但它其實一點也不神奇或神秘，而且撇後背後的數學運算，理解其原理完全沒有問題。簡單來說，貝葉斯推斷可以幫助你根據資料、整合相關資訊、並下更強的結論。

「貝葉斯推斷」取名自一位大約三百年前的倫敦長老會（Presbyterian）牧師——湯瑪士．貝葉斯（Thomas Bayes）。他寫過兩本書，一本和神學有關，另一本和統計學有關，其中包含了當今有名的**貝氏定理**（Bayes Theorem）的雛形。這個定理之後被廣泛應用於推斷問題，即用來做出合理的猜測。貝葉斯的諸多想法之所以會這麼熱門，另一位主教[理查德·普莱斯](http://onlinelibrary.wiley.com/doi/10.1111/j.1740-9713.2013.00638.x/full)（Richard Price）也功不可沒。他發現了這些想法的重要性、改進和發表了它們。考慮到這些歷史因素，更精確一點說，貝氏定理應該被稱作「貝葉斯－普萊斯規則」。

## 電影院裡的貝葉斯推斷

![](http://brohrer.github.io/images/bayesian_5.png)

