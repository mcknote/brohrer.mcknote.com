#卷積神經網路的運作原理

原文連結：[**How do Convolutional Neural Networks work?**](https://brohrer.github.io/how_convolutional_neural_networks_work.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}FmpDIaiMIeA{%endyoutube%}

相關連結：
* [PDF [2MB]](https://github.com/brohrer/public-hosting/raw/master/How_CNNs_work.pdf)、[PPT [6MB]](https://github.com/brohrer/public-hosting/blob/master/how_CNNs_work.pptx?raw=true)
* [日文版](http://postd.cc/how-do-convolutional-neural-networks-work/)
* [波斯文版](https://elham-khanche.github.io/blog/HowCNNsWork/)（由 [Elham Khanchebemehr](https://www.linkedin.com/in/elham-khanchebemehr-b679547b/) 翻譯）
    * 以及 [Mohammad KHalooei](https://www.linkedin.com/in/khalooei/) 所製作的[波斯文簡報](http://www.slideshare.net/khalooei/ss-70486910)
* [Nvidia GPU 上的 `MATLAB` 和 `Caffe` 實作](http://www.optophysiology.uni-freiburg.de/Research/research_DL/CNNsWithMatlabAndCaffe)由 [Alexander Hanuschkin](http://www.optophysiology.uni-freiburg.de/labmembers/hanuschkin) 完成

每當深度學習又有什麼重大突破時，這些進展十有八九都和**卷積神經網路**（Convolutional Neural Networks，CNN）有關。CNN 又被稱為 CNNs 或 ConvNets，它是目前**深度神經網路**（deep neural network）領域的發展主力，在圖片分類上甚至可以做到比人類還精準的程度。如果要說有任何方法能實現深度學習的狂想，CNN 絕對是首選。

CNN 最棒的地方在於它很好理解，至少在一步一步說明原理的情況下。所以我將為各位說明 CNN，也歡迎參考上方比圖片更詳細的影片。如果中間有什麼不懂的地方，只要點擊圖片，就能跳到影片中對應的說明。

## 圈圈叉叉

[![](http://brohrer.github.io/images/cnn1.png "CNN 和圈圈叉叉")](https://youtu.be/FmpDIaiMIeA?t=1m43s)

為了說明 CNN，我們可以從一個非常簡單的例子開始：辨識圖片上的符號是圈還叉。這個例子一方面已經能很好說明 CNN 的運作原理，另一方面也夠簡單，以免我們拘泥於不必要的細節。在這裡 CNN 最重要的工作，就是每當我們給它一張圖，它就會回報上面的符號是圈還是叉。對它來說結果永遠是圈或叉。

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

## 池化

[![](http://brohrer.github.io/images/cnn8.png "池化的運作方式")](https://youtu.be/FmpDIaiMIeA?t=9m20s)

另一個 CNNs 所使用的強大工具是**池化**（pooling）。池化是一個將原本的大圖壓縮並保留重要資訊的方法，它的運作原理只需要小二的數學程度就能弄懂。池化會在圖片上選取不同**窗口**（window），並在這個窗口範圍中選擇一個最大值。實務上，邊長為二或三的正方形範圍，搭配兩像素的**間隔**（stride）是滿理想的設定。

原圖經過池化以後，其所包含的像素數量會降為原本的四分之一，但因為池化後的圖片包含了原圖中各個範圍的最大值，它還是保留了每個範圍和各個特徵的相符程度。也就是說，池化後的資訊更專注於圖片中是否存在相符的特徵，而非圖片中哪裡存在這些特徵。這能幫助 CNN 判斷圖片中是否包含某項特徵，而不必分心於特徵的位置。這解決了前面提到電腦非常死板的問題。

[![](http://brohrer.github.io/images/cnn9.png "池化後的結果")](https://youtu.be/FmpDIaiMIeA?t=11m31s)

所以，池化層的功用是將一張或一些圖片池化成更小的圖片。最終我們會得到一樣數量、但包含更少像素的圖片。這也有助於改善剛才提到的耗費運算問題。事先將一張八百萬像素的圖片簡化成兩百萬像素，可以讓後續工作變得更輕鬆。

## 線性整流單元

[![](http://brohrer.github.io/images/cnn10.png "線性整流單元將負數化為 0")](https://youtu.be/FmpDIaiMIeA?t=11m46s)

另一個細微但重要的步驟是**線性整流單元**（Rectified Linear Unit，ReLU），它的數學原理也很簡單——將圖片上的所有負數轉為 0。這個技巧可以避免 CNNs 的運算結果趨近 0 或無限大，它就像 CNNs 的車軸潤滑劑一樣——沒有什麼很酷的技術，但沒有它 CNNs 也跑不了多遠。

[![](http://brohrer.github.io/images/cnn11.png "線性整流後的結果")](https://youtu.be/FmpDIaiMIeA?t=12m37s)

線性整流後的結果和原圖會有相同數量的像素，只不過所有的負值都會被換成零。

## 深度學習

[![](http://brohrer.github.io/images/cnn12.png "深度學習的流程")](https://youtu.be/FmpDIaiMIeA?t=12m51s)

到這裡，讀者可能已經發現每一層運算的輸入（二維矩陣）和輸出（二維矩陣）都差不多，這代表我們可以像疊樂高積木一樣，將每一層疊在一起。所以原圖在經過篩選、整流、池化之後會變成一組更小、包含特徵資訊的圖片。接下來，這些圖片還可以再被篩選和壓縮，它們的特徵會隨著每次處理變得更複雜，圖片也會變得更小。最後，比較低階的處理層會包含一些簡單的特徵，例如稜角或光點；更高階的處理層則會包含一些比較複雜的特徵，像是形狀或圖案。這些高階的特徵通常已經變得很好辨識，比方說在人臉辨識的 CNN 中，我們可以在最高階的處理層內看出完整的人臉。

[![](https://brohrer.github.io/images/cnn18.png "不同層所能辨認出的特徵")](http://web.eecs.umich.edu/~honglak/icml09-ConvolutionalDeepBeliefNetworks.pdf)


## 全連接層

[![](https://brohrer.github.io/images/cnn13.png "在全連結層內進行的投票")](https://youtu.be/FmpDIaiMIeA?t=14m03s)

最後，CNNs 還有一項秘密武器——**全連結層**（fully connected layers）。全連結層會集合高階層中篩選過的圖片，並將這些特徵資訊轉化為票數。在我們的例子裡有兩個選項：圈或叉。在傳統的神經網路架構中，全連結層所扮演的角色是**主要建構單元**（primary building block）。當我們對這個單元輸入圖片時，它會將所有像素的值當成一個一維清單，而不是前面的二維矩陣。清單裡的每個值都可以決定圖片中的符號是圈還是叉，不過這場選舉並不全然民主。由於某些值可以更好地判別叉，有些則更適合用來判斷圈，這些值可以投的票數會比其他值還多。所有值對不同選項所投下的票數，將會以**權重**（weight）或**連結強度**（connection strength）的方式來表示。

所以每當 CNN 判斷一張新的圖片時，這張圖片會先經過許多低階層，再抵達全連結層。在投票表決之後，擁有最高票數的選項將成為這張圖片的類別。

[![](https://brohrer.github.io/images/cnn14.png "從低階層到全連結層")](https://youtu.be/FmpDIaiMIeA?t=16m32s)

和其他階層一樣，多個全連結層也可以被組合在一起，因為它們輸入（清單）和輸出（投票結果）的形式非常相近。實務上，我們可以將多個全連結層組合在一起，其中幾層會出現一些虛擬、隱藏的投票選項。每當我們新增一層全連結層，整個神經網路就可以學習更複雜的特徵組合，所做出的判斷也會更準確。

## 反向傳播

[![](https://brohrer.github.io/images/cnn15.png "反向傳播")](https://youtu.be/FmpDIaiMIeA?t=18m13s)

到目前為止的說明看起來都很不錯，但其實背後還有一個大問題——特徵從何而來？以及在全連結層中，我們該如何決定權重？如果我們得親手完成這些工作，CNNs 應該不會像現在一樣熱門。幸好，有個名叫**反向傳播**（backpropagation）的機器學習技巧可以幫助我們解決這個問題。

為了使用反向傳播，我們需要先準備一些已經有答案的圖片。這代表我們需要先靜下心來，為幾千張圖片標上圈和叉。接著我們得準備一個未經訓練的 CNN，其中任何像素、特徵、權重和全連結層的值都是隨機決定的。然後我們就可以用一張張標好的圖片訓練這個 CNN。

經過 CNN 的處理，每張圖片最終都會有一輪決定類別的選舉。和之前標好的正解相比，這場選舉中的誤判，也就是辨識誤差，可以告訴我們怎樣才是好的特徵跟權重。我們可以透過調整特徵和權重，讓選舉所產生的誤差降低。在每次調整後，這些特徵和權重會被微調高一點或低一點，誤差也會被重新計算，成功降低誤差的調整將被保留。所以當我們調整過卷積層裡的每個像素、和全連結層裡的每個權重以後，我們可以得到一組稍微更擅於判斷當前圖片的權重。接著我們可以重複以上步驟，辨認更多已標記的圖片。訓練過程中，個別圖片裡的誤判會過去，但這些圖片中共通的特徵和權重會留下。如果有夠多的已標記圖片，這些特徵和權重的值最後會趨近一個擅於辨識大多數圖片的穩定狀態。

不用說，反向傳導也是個很耗費運算資源的步驟，這也是另一個驅使廠商生產特殊元件的原因。

## 超參數

除了以上說明，CNN 還是有些很難解釋和學習的面向。CNN 的設計者需要做很多決定，包括以下問題。

* 每個卷積層中該有多少特徵？每個特徵中該有多少像素？
* 每個池化層中的窗口大小為何？間隔又該多長？
* 每個額外的全連結層該有多少隱藏神經元（選項）？

除了這些問題，我們還需要考慮很多高階的結構問題，像是 CNN 中該有多少處理層、順序為何。有些深度神經網路可能包括上千個處理層，設計上也就有非常多可能性。

有了這麼多排列組合，我們只能測試其中一小部分的 CNN 設定。因此 CNN 的設計通常會隨著機器學習社群所累積下來的知識演進，效能上偶爾會出現一些出乎意料的提升。雖然我們已經介紹了一些基本的 CNN 結構，但除此之外還有很多經測試後發現有效的改進技巧，例如使用新型處理層，或用更複雜的方式連接不同處理層。


## 圖形處理之外

[![](https://brohrer.github.io/images/cnn16.png)](https://youtu.be/FmpDIaiMIeA?t=21m26s)

我們的圈和叉例子和圖像辨識有關，不過 CNN 也能處理其他型態的資料，技巧是將任何資料轉成類似圖片的形式。例如，我們可以將音訊根據時間細分，再將每一小段的聲音分成低音、中音、高音或其他更高的頻率。如此一來，我們就可以把這些資訊組成一個二維矩陣，其中各行代表不同時間、各列代表不同頻率。在這張假圖片裡，越相近的「像素」，彼此之間的關聯性越高。CNN 很擅長處理這樣的資料，研究者們也發揮創意，將**自然語言處理**（natural language processing）中的文字資料、和新藥研發的的化學資料都轉成 CNN 可以處理的形式。

[![](https://brohrer.github.io/images/cnn17.png)](https://youtu.be/FmpDIaiMIeA?t=23m36s)
