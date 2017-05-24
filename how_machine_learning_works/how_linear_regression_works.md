# 線性迴歸的運作原理

[**How linear regression works**](https://brohrer.github.io/how_linear_regression_works.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{% youtube src="https://www.youtube.com/watch?v=fE0bnkNX77A" %}{% endyoutube %}

**線性迴歸**（linear regression）是在資料點中找出規律、畫出一條直線的專業說法，以下我將透過選購鑽石的例子說明其運作原理。

我的奶奶曾經留給我一只戒指。這個戒指上有個 1.35 克拉大小的鑲台（setting），可惜上面沒有安任何鑽石。我想修理這只戒指，所以我找了一間珠寶行詢價，以了解我需要準備多少錢。

到了珠寶行以後，我發現店裡沒有 1.35 克拉的鑽石，也就沒有價格。但我沒有因此放棄，我拿起了紙跟筆，把店裡所有其他鑽石的尺寸跟價格都抄了下來。

![](https://brohrer.github.io/images/linear_regression/linear_regression_1.png "左邊是鑽石的克拉數，右邊是價格")

我發現絕大部分的鑽石都在 2 克拉以下，所以我畫了一條橫軸以紀錄克拉數。

![]
(https://brohrer.github.io/images/linear_regression/linear_regression_2.png "紀錄鑽石克拉數的橫軸")

接下來我畫了一條用來記錄價格的縱軸。

![]
(https://brohrer.github.io/images/linear_regression/linear_regression_3.png "加上紀錄價格的縱軸")