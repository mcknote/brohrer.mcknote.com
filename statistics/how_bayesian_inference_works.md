# 貝葉斯推斷的運作原理

原文：[**How Bayesian inference works**](http://brohrer.github.io/how_bayesian_inference_works.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

* [Google 簡報上的投影片](https://docs.google.com/presentation/d/1325yenZP_VdHoVj-tU0AnbQUxFwb8Fl1VdyAAUxEzfg/edit?usp=sharing)

**貝葉斯推斷**（Bayesian Inference）是一套可以用來精進預測的方法，在資料不是很多、又想盡量發揮預測能力時特別有用。

雖然有些人會抱著敬畏的心情看待貝葉斯推斷，但它其實一點也不神奇或神秘，而且撇後背後的數學運算，理解其原理完全沒有問題。簡單來說，貝葉斯推斷可以幫助你根據資料、整合相關資訊、並下更強的結論。

「貝葉斯推斷」取名自一位大約三百年前的倫敦長老會（Presbyterian）牧師——湯瑪士．貝葉斯（Thomas Bayes）。他寫過兩本書，一本和神學有關，另一本和統計學有關，其中包含了當今有名的**貝氏定理**（Bayes Theorem）的雛形。這個定理之後被廣泛應用於推斷問題，即用來做出合理的猜測。貝葉斯的諸多想法之所以會這麼熱門，另一位主教[理查德·普莱斯](http://onlinelibrary.wiley.com/doi/10.1111/j.1740-9713.2013.00638.x/full)（Richard Price）也功不可沒。他發現了這些想法的重要性、改進和發表了它們。考慮到這些歷史因素，更精確一點說，貝氏定理應該被稱作「貝葉斯－普萊斯規則」。

## 電影院裡的貝葉斯推斷

[![](http://brohrer.github.io/images/bayesian_5.png)](https://youtu.be/5NMxiOGL39M?t=48s)

現在請讀者想像你人在電影院，剛好看到前面有個人掉了電影票。這個人的背影如上圖所示，你想叫住他／她，但你只知道這個人有一頭飄逸長髮，卻不知道他／她的性別。問題來了：你該大喊「先生，不好意思」還是「女士，不好意思」？根據讀者對兩性頭髮長度的印象，你或許會認為這個人是女的。（在這個簡單的例子裡，我們只考慮長髮和短髮、男性和女性。）

但現在考慮另一個狀況：如果這個人排在男性洗手間的隊伍當中呢？多了這項資訊，讀者或許會認為這個人是男的。我們可以不經思索地根據不同的常識和知識調整判斷，而貝葉斯推斷正是將這點化為數學，幫助我們做出更精準的評估。

[![](http://brohrer.github.io/images/bayesian_11.png)](https://youtu.be/5NMxiOGL39M?t=2m35s)

為了將前面的例子用數學表達，我們可以先假設電影院裡的人有一半是女的，有一半是男的。也就是說 100 個人裡面，有 50 名男性和 50 名女性。在這 50 名女性裡，有一半的人有長髮（25 人），另一半有短髮（25 人）；在 50 名男性當中，48 個人有短髮，兩個人有長髮。因為在 27 位長髮觀眾裡，有 25 位是女性和兩位男性，所以前面第一個猜測很安全。

[![](http://brohrer.github.io/images/bayesian_12.png)](https://youtu.be/5NMxiOGL39M?t=2m49s)

但如果我們換一個場景：在男性洗手間隊伍的 100 個人裡面，有 98 位男性和兩位陪伴中的女性。這裡的女性雖然也有一半是長髮、一半是短髮，但人數減為一位長髮女性和一位短髮女性。男性觀眾中長髮和短髮的比例也不變，不過因為總人數變成了 98 人，現在隊伍裡有 94 位短髮男性，和四位長髮男性。由於現在長髮觀眾中有一名女性和四名男性，保守的猜測變成了男性。從這個例子，我們可以很容易地理解貝葉斯推斷的原理。根據不同的先決條件——也就是這名觀眾是否站在男性洗手間的隊伍裡，我們可以做出更準確的評估。

為了好好說明貝葉斯推斷，我們最好先花點時間清楚定義一些觀念。很不湊巧這段會用到一些數學，不過我們會避免談任何不必要的細節，請務必耐心讀完以下幾段，這對理解之後的內容很有幫助。為了打好基礎，我們需要快速認識四個觀念：**機率**（probabilities）、**條件機率**（conditional probabilities）、**聯合機率**（joint probabilities）和**邊際機率**（marginal probabilities）。

## 機率

[![](http://brohrer.github.io/images/bayesian_13.png)](https://youtu.be/5NMxiOGL39M?t=2m55s)

某事件發生的機率，就是將「該事件的數量」除以「所有可能發生的事件數量」。在我們的例子裡，某位觀眾是女性的機率是 50 名女性除以 100 位觀眾，即 0.5 或 50%。觀眾是男性的機率也是 50%。

[![](http://brohrer.github.io/images/bayesian_14.png)](https://youtu.be/5NMxiOGL39M?t=3m7s)

在男性洗手間的隊伍裡，觀眾是女性的機率為 0.02，男性的機率為 0.98。

## 條件機率

[![](http://brohrer.github.io/images/bayesian_15.png)](https://youtu.be/5NMxiOGL39M?t=3m16s)

條件機率所能回答的問題是「如果這名觀眾是女性，那她有長頭髮的機率為何？」條件機率的計算方式和一般機率相同，只不過條件機率只會涉及符合條件的少部分樣本。在我們的例子裡，女性觀眾中有長髮的條件機率 $$ P(long hair | woman) $$ 即「長髮女性人數」除以「女性總人數」，也就是 0.5。不管我們是計算電影院裡的觀眾，還是男性廁所隊伍裡的觀眾，都會得到相同的條件機率。

[![](http://brohrer.github.io/images/bayesian_17.png)](https://youtu.be/5NMxiOGL39M?t=4m9s)

運用相同的方法，男性觀眾中有有長髮的條件機率 $$ P(long hair | man) $$ 為 0.04，不管是隊伍裡還是隊伍外的男性。

[![](http://brohrer.github.io/images/bayesian_18.png)](https://youtu.be/5NMxiOGL39M?t=4m17s)

關於條件機率，有一件需要特別注意的事：$$ P(A | B) $$ 並不等於 $$ P(B | A) $$。例如 $$ P (cute | puppy) $$ 並不等於 $$ P (puppy | cute) $$。如果我手上抱著的東西是一隻狗狗，那這個東西很可愛的機率就很高，但如果我只知道我手上抱著的東西很可愛，那這個東西是狗狗的機率只有中下，因為它也有可能是貓貓、兔兔、小刺蝟或小嬰兒。

## 聯合機率

[![](http://brohrer.github.io/images/bayesian_19.png)](https://youtu.be/5NMxiOGL39M?t=5m2s)

聯合機率適合用來回答這類問題：「這位觀眾是一名短髮女性的機率為何？」回答這個問題的過程分為兩個步驟。首先，我們會先找出觀眾是女性的機率 $$ P(woman) $$；接著，我們再找出在女性觀眾中短髮的條件機率 $$ P(short hair | woman) $$。將兩個機率相乘，就可以得到前面問題所求的聯合機率，即 $$ P(woman with short hair) = P(woman) * P(short \space hair | woman) $$。利用這個方法，我們可以重算一次之前的結論——電影院裡，某位觀眾為長髮女性的機率 $$ P(woman with long hair) $$ 為 0.25，但男性洗手間的隊伍裡，某位觀眾為長髮女性的機率 $$ P(woman with long hair) $$ 為 0.01。兩者之所以不同，是因為觀眾為女性的機率 $$ P(woman) $$ 在兩個情況下不同。

[![](http://brohrer.github.io/images/bayesian_23.png)](https://youtu.be/5NMxiOGL39M?t=5m37s)

同理，我們也可以算出電影院裡，某位觀眾為長髮男性的聯合機率 $$ P(man with long hair) $$ 為 0.02，但在男性洗手間隊伍裡的聯合機率則為 0.04。

[![](http://brohrer.github.io/images/bayesian_25.png)](https://youtu.be/5NMxiOGL39M?t=5m49s)

聯合機率和條件機率不同的地方，在於聯合機率的計算順序並不影響結果。也就是說 $$ P(A \space and \space B) $$ 和 $$ P(B \space and \space A) $$ 是一樣的。我喝牛奶又吃果醬甜甜圈（jelly donut）的機率，和我吃果醬甜甜圈又喝牛奶的機率是一樣的。

## 邊際機率

[![](http://brohrer.github.io/images/bayesian_26.png)](https://youtu.be/5NMxiOGL39M?t=6m16s)

最後，我們需要了解的是邊際機率，它可以用來回答這類問題：「某位觀眾有長髮的機率為何？」要回答這個問題，我們需要將所有符合這個條件的事件發生機率加總——即長髮男性和長髮女性的聯合機率。將這兩個聯合機率相加以後，我們可以得出在電影院裡的觀眾是長頭髮的機率 $$ P(long hair) $$ 為 0.27，但在男性洗手間的隊伍裡這個機率只有 0.05。

## 貝氏定理

介紹完四個重要的觀念以後，終於可以來談我們關心的問題了。我們想要了解「如果我們知道某人有長頭髮，這個人是女性（或男性）的機率為何？」這也是條件機率 $$ P(man | long hair) $$ 的解，但我們現在只知道順序相反的情況 $$ P(long \space hair | man) $$，即「如果我們知道某人是男性，這個人有長頭髮的機率為何？」因為條件機率的順序不可任意調換，我們還沒辦法回答這個問題。




