#卷積神經網路的運作原理

原文連結：[**How do Convolutional Neural Networks work?**](https://brohrer.github.io/how_convolutional_neural_networks_work.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}FmpDIaiMIeA{%endyoutube%}

相關連結：
* [PDF [2MB]](https://github.com/brohrer/public-hosting/raw/master/How_CNNs_work.pdf)、[PPT [6MB]](https://github.com/brohrer/public-hosting/blob/master/how_CNNs_work.pptx?raw=true)
* [日文版](http://postd.cc/how-do-convolutional-neural-networks-work/)
* [波斯文版](https://elham-khanche.github.io/blog/HowCNNsWork/)（由 [Elham Khanchebemehr](https://www.linkedin.com/in/elham-khanchebemehr-b679547b/) 翻譯）
    * 以及 [Mohammad KHalooei](https://www.linkedin.com/in/khalooei/) 所製作的[波斯文簡報](http://www.slideshare.net/khalooei/ss-70486910)
* Nvidia GPU 上的 `MATLAB` 和 `Caffe` 實作由 [Alexander Hanuschkin](http://www.optophysiology.uni-freiburg.de/labmembers/hanuschkin) 完成

每當讀者聽到深度學習又有什麼重大突破時，這些進展十有八九都和**卷積神經網路**（Convolutional Neural Networks，CNN）有關。CNN 又稱為 CNNs 或 ConvNets，它是目前深度神經網路（deep neural network）領域的發展主力，在圖片分類上甚至可以做到比人類還精準的程度。如果要說有任何方法能實現深度學習的狂想，CNN 絕對是首選。

CNN 最棒的地方在於它很好理解，至少在一步一步說明原理的情況下。所以我將為各位說明 CNN，也歡迎參考上方比圖片更詳細的影片。如果中間有什麼不懂的地方，只要點擊圖片，就能跳到影片中對應的說明。

## 圈圈叉叉

[![](http://brohrer.github.io/images/cnn1.png "CNN 和圈圈叉叉")](https://youtu.be/FmpDIaiMIeA?t=1m43s)

為了說明 CNN，我們可以從一個非常簡單的例子開始：辨識圖片上的符號是圈還叉。這個例子一方面已經能很好說明 CNN 的運作原理，另一方面也夠簡單，以免拘泥於不必要的細節。在這裡 CNN 最重要的工作，就是每當我們給它一張圖，它就得回報上面的符號是圈還是叉。對它來說結果永遠是兩者之一。

[![](http://brohrer.github.io/images/cnn2.png "直接比對的問題")](https://youtu.be/FmpDIaiMIeA?t=3m05s)

有個很簡單的方法，是直接用圈和叉的圖片去比對新的圖片，看圖上的符號比較像哪個。但事情沒有這麼簡單，因為電腦在比對這些圖片的時候非常刻板。在電腦看來，這些圖片只是一群排成二維矩陣、帶有位置編號的像素（就跟棋盤一樣）。於是在我們的例子裡，白色格子（即筆畫）的值為 1，黑色格子（即背景）的值為 -1。在比對圖片時，如果有任何一個格子的值不相等，那電腦就會認為兩張圖是不一樣的。理想上我們希望不管在平移、縮小、旋轉或變形等情況下，電腦都能判斷符號。這時 CNN 就派上用場了。

## 特徵

[![](http://brohrer.github.io/images/cnn3.png "局部比對特徵")](https://youtu.be/FmpDIaiMIeA?t=3m59s)

CNNs 會根據各個局部比較兩張圖片，每個由 CNN 比對的局部被稱為一個特徵（feature）。比起比較整張圖片，藉由在相似的位置上比對大略特徵，CNNs 能更好地分辨兩張圖片是否相同。

[![](http://brohrer.github.io/images/cnn4.png "一張圖片裡的每個特徵")](https://youtu.be/FmpDIaiMIeA?t=4m20s)

一張圖片裡的每個特徵都像一張更小的圖片，也就是更小的二維矩陣。這些特徵會捕捉圖片中的共通要素。以叉叉的圖片為例，它最重要的特徵包括對角線和中間的交叉。任何叉叉的線條或中心點應該都會符合這些特徵。

## 卷積

[![](http://brohrer.github.io/images/cnn5.png "對應像素的乘積")](https://youtu.be/FmpDIaiMIeA?t=4m55s)

每當 CNN 分辨一張新圖片時，在不知道上述特徵在哪的情況下，CNN 會比對圖片中的任何地方。為了計算整張圖片裡有多少相符的特徵，我們在這裡創造了一套篩選機制。這套機制背後的數學原理被稱為**卷積**（convolution），也就是 CNN 的名稱由來。

卷積的基本原理，其實只需要小六程度的數學能力就能理解。要計算特徵和圖片局部的相符程度，只要將兩者各個像素上的值相乘、再將總和除以像素的數量。如果兩個像素都是白色（值為 1），乘積就是 1 \* 1 = 1；如果都是黑色（值為 -1），乘積就是 (-1) \* (-1) = 1。也就是說，像素相符的乘積為 1，像素相異的乘積為 -1。如果兩張圖的每個相素都相符，將這些乘積加總再除以像素數量就會得到 1；反之，如果兩者的像素完全不相符，就會得到 -1。


[![](http://brohrer.github.io/images/cnn6.png "不同位置的卷積")](https://youtu.be/FmpDIaiMIeA?t=7m20s)

我們只要重複上述過程、歸納出圖片中各種可能的特徵，就能完成卷積。然後，我們可以根據每次卷積的值和位置製作一個新的二維矩陣。這也就是利用特徵篩選過後的原圖，它可以告訴我們在原圖的哪些地方可以找到該特徵。值越接近 1 的局部和該特徵越相符，值越接近 -1 則相差越大，至於接近值接近 0 的局部，則幾乎沒有任何相似度。

[![](http://brohrer.github.io/images/cnn7.png "不同特徵的卷積圖")](https://youtu.be/FmpDIaiMIeA?t=8m20s)

下一步是將同樣的方法應用在不同特徵上，在圖片中各個部位的卷積。最後我們會得到一組篩選過的原圖，每一張圖都對應了一個特徵。我們可以將整段卷積運算，簡單想成單一的處理步驟。在 CNNs 的運作裡，這個步驟被稱為卷積層，這也代表後面還有更多層。

從 CNN 的運作原理，不難看出它很耗運算資源。雖然我們可以只用一張紙解釋完 CNN 如何運作，但運作途中相加、相乘和相除的數量可以增加得非常快。用數學來表達，可以說這些運算的數量，會隨著（一）圖片中相素的數量、（二）每個特徵中像素的數量、以及（三）特徵的數量呈現性增長。有了這麼多影響運算數量的因素，CNN 所處理的問題可以不費吹灰之力地變得非常複雜，也難怪一些晶片製造商為了因應 CNNs 的運算需求，正在設計和製造專用的晶片。
