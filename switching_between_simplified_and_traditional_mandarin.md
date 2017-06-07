# 中文簡繁轉換說明

**Switching between Simplified and Traditional Mandarin**

![](../assets/Screen Shot 2017-06-07 at 15.51.44.png)

資料科學・機器・人上的許多文章都由繁體中文所寫成。為了方便不同地區的讀者，在每篇文章最上方的導覽列裡，都有一個用於切換繁體和簡體中文的圖示，可以輕易變更文章的顯示語言。

本功能藉 GitBook 的[多語言功能](https://toolchain.gitbook.com/languages.html)和 [OpenCC](https://github.com/BYVoid/OpenCC) 所完成，非常感謝原作者。原先採用的解決方案為 [`gitbook-plugin-fanjian`](https://www.npmjs.com/package/gitbook-plugin-fanjian) 的逐字比對。使用時需注意的事情包括：

* 文章網址分為 `zh-Hant`（繁體中文）和 `zh-Hans`（簡體中文）。部分連結可能因此失效，請見諒
* 某些字詞的轉換上可能不盡完美，請多包涵和指教
* 切換圖示有時會消失。暫時的解決方法是重新載入文章

Jimmy，於 2017 年 6 月 7 日

