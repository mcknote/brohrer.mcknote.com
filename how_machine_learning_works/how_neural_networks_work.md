#神經網路的運作原理

原文連結：[How neural networks work](https://brohrer.github.io/how_neural_networks_work.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}ILsA4nyG7I0{%endyoutube%}

> 請注意，本文內容主要為未翻譯的影片和投影片。

* [Google 簡報上的投影片](https://docs.google.com/presentation/d/1AAEFCgC0Ja7QEl3-wmuvIizbvaE-aQRksc7-W8LR2GY/edit?usp=sharing)
* [PDF 版投影片](../slides/How neural networks work.pdf)（381 KB），於本文完成時存取

Far from being incomprehensible, the principles behind neural networks are surprisingly simple. Here's a gentle walk through how to use deep learning to categorize images from a very simple camera.

很多讀者可能會感到驚訝，**神經網路**（Neural Networks）的運作原理其實非常簡單，而且絕非無法理解。我將為各位簡單說明如何利用**深度學習**（Deep Learning）和一台簡易相機辨認圖片。

You have responded with overwhelmingly positive comments to my two previous videos on convolutional neural networks and deep learning. You have also made two requests: find a better example and explain backpropagation. This video does both.

這篇文章是為了回應讀者對我前兩篇文章（[卷積神經網路](../how_machine_learning_works/how_convolutional_neural_networks_work.md)和[深度學習](../how_machine_learning_works/deep_learning_demystified.md)）的熱烈迴響，以及你們的兩點建議：換一個更簡單的例子，和詳細解釋**反向傳播**（Backpropagation）。

Bradon，於 2016 年 5 月 22 日