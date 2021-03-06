#递归神经网络（RNN）和长短期记忆模型（LSTM）的运作原理

原文：[**How Recurrent Neural Networks and Long Short-Term Memory Work**](https://brohrer.github.io/how_rnns_lstm_work.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}WCUNPb-5EYI{%endyoutube%}

相关链接：

* [Google 投影片](https://docs.google.com/presentation/d/1hqYB3LRwg_-ntptHxH18W1ax9kBwkaZ1Pa_s3L7R-2Y/edit?usp=sharing)
* 本文翻译自 [Elham Khanchebemehr](https://www.linkedin.com/in/elham-khanchebemehr-b679547b/) 的[英文逐字稿](https://elham-khanche.github.io/blog/RNNs_and_LSTM/)，非常感谢

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide1.png "递归神经网络和长短期记忆模型的运作原理")](https://youtu.be/WCUNPb-5EYI?t=2)

这几年，机器学习（machine learning）相关的应用获得了许多关注，其中有几大领域特别热门：其中一个是图片辨识，像是在网络上搜索猫咪的图片，或是将任何问题转为类似形式；另一个则是串行到串行翻译（sequence to sequence translation），包括将语音转为文本或翻译不同语言。前者大多是利用[**卷积神经网络**](../how_machine_learning_works/how_convolutional_neural_networks_work.html)（convolutional neural networks，CNN）所完成，后者则多利用**递归神经网络**（recurrent neural networks，RNN），尤其是**长短期记忆模型**（long short-term memory，LSTM）。

## 晚餐要吃什么

为了理解 LSTM 的运作原理，我们可以考虑一下「晚餐要吃什么」这个问题：假设读者住在公寓，很幸运地有个爱煮晚餐的室友。每天晚上室友都会准备寿司、松饼或披萨，而你希望能预测某个晚上你会吃什么，并借此规划其他晚餐。为了预测晚餐，读者建了一个神经网络模型。这个模型的输入数据报括星期几、第几个月、以及室友是否开会开到很晚等会影响晚餐的因素。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide2.png "晚餐要吃什么？")](https://youtu.be/WCUNPb-5EYI?t=76)

讲到这里，如果读者对神经网络还不熟悉，可以花一点时阅读〈[神经网络的运作原理](../how_machine_learning_works/how_neural_networks_work.html)〉。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/CNNs.png "神经网络的运作原理")](https://youtu.be/WCUNPb-5EYI?t=91)

如果读者想先跳过其他文章，又还不清楚神经网络是什么，可以先把神经网络想成一个投票过程。神经网络里包含了一个复杂的投票过程，而我们所输入的数据，如星期几、第几个月等等，都会进入这个过程。接着我们可以根据过去的晚餐训练这个模型，并预测今天的晚餐。

不过，用这种方法训练的模型，表现并不是很好。就算我们谨慎挑选输入数据并训练模型，它的表现还是没有比随机猜测好上多少。

和其他复杂的机器学习问题一样，先退一步回顾数据，可以帮助我们找出其中的规律。于是我们发现，原来室友在做完披萨后的隔天会准备寿司，再隔一天会准备松饼，然后又回去做披萨，就这样持续下去。由于这个循环很普通，跟星期几没什么关系，我们可以根据这项特征训练一个新的神经网络模型。

在这个新的模型里，唯一重要的因素只有昨天吃过的晚餐，所以如果昨天吃披萨，今天就会吃寿司；昨天吃寿司，今天吃松饼；昨天吃松饼，今天吃披萨。整个投票过程变得非常简单，预测也很准确，因为你的室友做事非常连贯。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide3.png "晚餐出现的循环")](https://youtu.be/WCUNPb-5EYI?t=170)

现在考虑另一个情况：如果读者有某一晚不在家，像是昨天晚上出门了，那就无从得知昨天晚餐吃什么。不过，我们还是能从几天前的晚餐推测今天会吃什么——只要先从更早之前推回昨天的晚餐，就能接着预测今天的晚餐。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide4.png "根据昨天的历史或预测结果预测")](https://youtu.be/WCUNPb-5EYI?t=218)

总之，我们不只能利用昨晚实际吃什么，也能利用昨晚的预测结果。

## 矢量和 one-hot 编码

讲到这里，我们可以先岔开谈一谈什么是**矢量**（vector）。矢量只是用来表示一组数字的数学名词。如果我想描述某一天的天气，我可以说当天的最高温是华氏 76 度（约摄氏 24.5 度），最低温是 43 度（约摄氏 6 度），风速是每小时 13 英里（约 21 公里），而且降下 0.25 吋（约 6.4 公厘）雨量的机率是 83%。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide5.png "包含天气信息的矢量")](https://youtu.be/WCUNPb-5EYI?t=235)

这就是所谓的矢量。矢量，也就是一组数字，之所以方便，在于它是电脑原生的语言。如果读者想将数据转成电脑能够运算、处理、并能应用机器学习的形式，一组数字即为正确选择。所以任何信息在经过算法处理前，都会先被转换成一组数字。就连 「今天是周二」这个概念，我们也可以利用矢量表示。

要表达这类的信息，我们只要先在矢量中包含所有可能的值，也就是周一到周日，再为每个值赋予特定的数字，将周二设为 1（Boolean True），其他日子设为 0（Boolean False）。这种格式被称作 **one-hot 编码**（或译作「独热编码」），而这种包含一连串 0、只有一个 1 的矢量在数据分析中也很常见。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide6.png "使用 one-hot 编码的矢量")](https://youtu.be/WCUNPb-5EYI?t=282)

虽然 one-hot 编码看起来很没效率，实际上这能帮助电脑更简单地处理信息，所以我们可以将「今天晚餐吃甚么」的预测转换成一个 one-hot 矢量，将预测结果之外的数值都设为 0。在下图的例子里，我们预测的晚餐是寿司。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide7.png "将晚餐选择转为 one-hot 矢量")](https://youtu.be/WCUNPb-5EYI?t=309)

现在我们可以将所有的输入和输出组合成几个矢量，也就是几组数字。这可以帮助我们解释整个神经网络的架构。利用我们归纳出的三个矢量：昨天的预测、昨天的结果、以及今天的预测，这里的神经网络架构即为每个输入因素和输出因素间的**关联**（connection）。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide9.png "输入因素和输出因素间的关联")](https://youtu.be/WCUNPb-5EYI?t=320)

为了完成上面的示意图，我们可以加上今日预测结果的回收循环。下图中的虚线，表示了今天的预测结果如何在明天被重新利用，成为明天的「昨日预测」。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide10.png "输入因素和输出因素间的关联")](https://youtu.be/WCUNPb-5EYI?t=360)

这个模型也说明了为什么我们在缺少某些信息的情况下（例如不在家两周），还是能准确预测今晚吃甚么。只要忽略接收新信息的部分，我们就能将纪录时间和餐点的矢量延伸到最近的数据点，并接着预测下去。

延伸过后的神经网络如下图所示。我们可以从最前端一直往过去的数据延伸，看更早之前的晚餐是甚么，再往两周的空缺延伸，直到我们得到今晚的预测。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide12.png "延伸后的预测结果")](https://youtu.be/WCUNPb-5EYI?t=388)

## 写一本童书

以上就是一个理解 RNN 运作原理的好例子。不过为了突显这个模型的限制，我们可以换一个写童书的例子。这本童书里只有三种句子：「道格看见珍（句号）」、「珍看见小点（句号）」、以及「小点看见道格（句号）」。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide13.png "写一本童书")](https://youtu.be/WCUNPb-5EYI?t=427)

这本童书的字汇量很小，只有「道格」、「珍」、「小点」、「看见」以及句号。在这个例子里，神经网络的功用在于将这些单字按正确的顺序排好，完成一本童书。我们先将前面的晚餐矢量，换成这个例子里的字典矢量。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide14.png "前后关联的单字预测")](https://youtu.be/WCUNPb-5EYI?t=445)

接着我们就能用前面的方法（即 one-hot）表达不同单字。如果道格是我最后读到的单字，那我的信息矢量中，就只有道格的数值为 1，其他的数值都为 0。我们也可以按前面的方法，利用昨天（前一次）的预测结果，继续预测明天（下一次）的结果。经过一定的训练后，我们应该能从模型中看出一些特定的规律。像是在「珍、道格、小点」之后，模型预测「看见」和句点的机率应该会大幅提升，因为这两个单字都会跟着特定名字出现。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide15.png "单字之间的关联（一）")](https://youtu.be/WCUNPb-5EYI?t=479)

同样地，如果我们前一次预测了名字，那这些预测也会加强接下来预测「看见」或句号的机率；如果我们看到「看见」或句号，也能想像模型接下来会倾向于预测「珍、道格、小点」等名字。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide16.png "单字之间的关联（二）")](https://youtu.be/WCUNPb-5EYI?t=501)

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide17.png "单字之间的关联（三）")](https://youtu.be/WCUNPb-5EYI?t=504)

于是，我们可以将这个流程和架构视为一个 RNN 模型。为了简单起见，这里我将矢量和投票权重用一个包含点和线（箭头）的符号代替。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide18.png "简化后的 RNN 模型")](https://youtu.be/WCUNPb-5EYI?t=527)

## 挤压函数（双曲正切函数）

除了模型本身，这张图还包括了一个我们前面没提到的符号。这个波浪符号代表**挤压函数**（squashing function，又译作 S 函数），它可以帮助整个神经网络更好运作。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide19.png "挤压函数（双曲正切函数）")](https://youtu.be/WCUNPb-5EYI?t=546)

挤压函数的功用，是将模型的投票结果限制在特定范围之间。比方说，如果有个投票结果得到 0.5 的值，我们可以在挤压函数上画一条 x = 0.5 的垂直线，并得到水平对应的 y 值，也就是挤压过后的数值。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide22.png "挤压函数上的数值对应（一）")](https://youtu.be/WCUNPb-5EYI?t=569)

对于小的数值而言，原始数值和挤压过构的数值通常很相近，但随着数值增大，挤压过后的数值会越来越接近 1。随着数值愈趋负无穷大，挤压过后的数值也会越来越接近 -1。不论如何，挤压过后的数值都会介于 1 和 -1 之间。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide25.png "挤压函数上的数值对应（二）")](https://youtu.be/WCUNPb-5EYI?t=574)

挤压函数的处理，对于神经网络这种重复运算相同数值的流程非常有用。比方说，如果有个选项每次都得到两次投票，它的数值也会被乘以二，随着流程重复，这个数字很容易被放大成天文数字。借由确保数值介于 1 和 -1 之间，即使我们将数值相乘无数次，也不用担心它会在循环中无限增大。这是一种**负回馈**（negative feedback）或**衰减回馈**（attenuating feedback）的例子。

## 可能出现的错误

读者可能已经注意到，这个例子里的神经网络会出现一些错误，像是得出「道格看见道格（句号）」等句子，因为每当「看见」出现在模型里，后面接著名字的机率就会大幅提升。当然，我们也可能得到「道格看见珍看见小点看见道格」之类的错误。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide27.png "当前模型可能出现的错误")](https://youtu.be/WCUNPb-5EYI?t=669)

这是因为我们的模型有着很短期的记忆，只会参考前一步的结果，所以它不会参考更早之前的信息，以避免这类错误。为了解决这个问题，我们需要在模型中加入更多内容。

## 记忆／遗忘路径

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide29.png "准备扩充的 RNN")](https://youtu.be/WCUNPb-5EYI?t=680)

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide30.png "加入记忆／遗忘路径后的 RNN")](https://youtu.be/WCUNPb-5EYI?t=682)

上图中，我们添加的关键是**记忆／遗忘**（memory and forgetting）路径，用于帮助模型能记住几个循环前发生的事情。为了解释记忆部分的运作原理，我们需要先认识几个新的符号。

## 逐元素相加、相乘和闸门

首先，圈圈里包含十字的符号是（矩阵）**逐元素加法**（element by element addition）。它的运作原理是将两个相同长度的矩阵按同位置和顺序的元素相加，所以我们可以将第一个矢量中的*第一个元素*，和第二个矢量中的*第一个元素*相加，得到输出矢量中的*第一个元素*。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide31.png "逐元素相加")](https://youtu.be/WCUNPb-5EYI?t=715)

于是在上图中我们可以看到 3 加 6 等于 9。我们可以接着处理下一个元素：4 加 7 等于 11，最后我们所得到的矢量会和原本长度一样，不过其中每一个元素都是前两个矢量逐元素的和。

相信读者也能猜到圈圈里有个交叉的符号是（矩阵）**逐元素乘法**（element by element multiplication）。它和逐元素加法的运作原理类似，只是运算方法从加法换成了乘法。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide32.png "逐元素相乘")](https://youtu.be/WCUNPb-5EYI?t=767)

逐元素相乘可以帮助我们完成一些很酷的事情。我们可以想像有一组信号，以及一组可以控制水量的水管。在这个例子里，我们把初始信号都当作 0.8。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide33.png "逐元素相乘和闸门")](https://youtu.be/WCUNPb-5EYI?t=782)

在每个水管上都有一个龙头，可以用来全开、全关或任意水量，让信号流通或堵塞。所以在这个例子里，全开的龙头有乘数 1，而全关的龙头则有乘数 0。根据前面提到的逐元素乘法，我们可以在一开始将 0.8 乘上全开的 1，得到原本的信号 0.8，也会在最后将 0.8 乘上 0，得到被屏蔽的信号 0。中间的 0.8 则会乘上 0.5，得到一个比较小、衰减过后的信号。这组**闸门**（gate）可以让我们控制信号的流通与否，非常有用。

## 挤压函数（逻辑函数）

为了实现上述的闸门，我们需要一组介于 0 和 1 之间的数值，所以这里又有另一种压缩函数。这个函数的符号是一个带有平底的圆形，它被称作**逻辑函数**（logistic function）。逻辑函数和我们前面提到的**双曲正切函数**（hyperbolic tangent function）很类似，除了前者的输出值介于 0 和 1 之间，而非后者的 -1 和 1 之间。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide34.png "挤压函数（逻辑函数）")](https://youtu.be/WCUNPb-5EYI?t=859)

介绍完这些新名词之后，在新的模型里，我们还是会利用先前的预测（昨天的预测）以及新的信息（昨天的结果）。我们会利用两者作出新的预测，而这些新的预测会在下一次循环回模型当中。

不过在新的模型里，这些新预测会通过下图中的新路径。在这条路径里，我们会透过逻辑函数，创建一个回忆或遗忘特定信息的闸门（即图中的「圈叉」符号），并将结果加回下次的预测当中 （即图中的「圈加」符号） 。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide35.png "记忆／遗忘路径")](https://youtu.be/WCUNPb-5EYI?t=900)

## 筛选路径

在这个新的循环里，我们不只有预测，而是由回忆筛选过的预测，这些回忆中包含了长久下来模型并未遗忘的信息。于是这边多了一组完全独立、用于记得和遗忘信息的神经网络。添加的遗忘路径可以帮助我们保留任意时间长短的信息。

这时读者可能会发现在结合了预测和回忆之后，我们不一定会直接将这组结果当作最终的预测。所以模型中最后还加上了一个**筛选**（selection）路径，将一部分的预测结果保留在模型中。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide36.png "多了筛选路径的 RNN")](https://youtu.be/WCUNPb-5EYI?t=959)

这个筛选路径也自成一个神经网络，有自己的投票机制。所以每次循环一开始的新信息和旧预测，也会用于决定筛选路径中的闸门大小。这组闸门会决定哪些结果该留在模型中，哪些结果该作为最终预测。

在筛选路径前，我们可以看到另一个压缩函数。因为在这之前我们做了一次逐元素相加 （即图中的「圈加」符号），预测结果可能会比 1 大或比 -1 小，所以这边的压缩函数是用来确保数值大小尚在控制范围中。

于是在上图中我们可以看到，每当新预测，也就是一组预测结果（possibilities）产生时，我们会将它和过去的回忆结合，并将从中选出特定几项作为该次循环的预测结果。在这个循环中，每条路径中的机制都是透过神经网络学习，包括何时该遗忘或将回忆删除。

## 忽视路径

最后在完成整个模型前，我们还需要加上另一组闸门。添加的**忽视**（ignoring）路径可以帮助我们筛选掉初步的预测结果。这项机制是故意的，让近期内不是很相关的结果先被忽视，避免它们影响之后的路径。和其他路径一样，忽视路径有自己的神经网络、逻辑挤压函数和闸门 （即图中右下角的「圈叉」符号） 。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide37.png "多了忽视路径的 RNN")](https://youtu.be/WCUNPb-5EYI?t=1046)

## 「珍看见小点（句号），道格⋯⋯」

到目前为止，LSTM 模型里已经有了许多组成部分，相信读者一时之间也摸不着头绪，所以我们可以透过一个很简单的例子了解整个模型如何运作。这确实是一个过度简化的例子，等一下欢迎纠正我，不过当读者能找出这些问题时，也代表你们能接着学习更高端的内容了。

我们先回到一开始提到的写童书的例子，并为了简单起见，我们可以假设整个 LSTM 模型已经被训练完毕，也就是说模型里的投票机制和权重都已经底定了。以下利用逐页动画说明。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide38.png "「珍看见小点（句号），道格⋯⋯」")](https://youtu.be/WCUNPb-5EYI?t=1137)

我们的故事到目前为止是「珍看见小点（句号），道格⋯⋯」。「道格」成了目前故事里的最后一个字，而且不意外地，前一次的预测结果包含了「道格、珍、小点」等名字。这是因为先前我们用句号结束了上个句子，所以下个句子可以用任何名字开头。

于是我们有了新信息「道格」，以及上次的预测「道格、珍、小点」。我们接着将这两组矢量输入包含预测、忽视、遗忘和筛选等四条路径的模型中。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide40.png "初步预测结果：「看见」和「非道格」")](https://youtu.be/WCUNPb-5EYI?t=1160)

模型中的第一步是先做出预测。由于「道格」是故事里的最后一个字，这个步骤可以判断接下来出现「看见」的机率很高，也会判断短期内不该再出现「道格」。所以路径最后会产出「看见」的**正预测**（positive prediction）和「道格」的**负预测**（negative prediction）。由于我们并不预期短时间内「道格」会再次出现，上图预测路径中的「道格」以黑字表示（即「非道格」）。

接着，在这个简单的例子里，我们可以不用考虑忽视路径，于是这组结果会进一步通过回忆路径。同样地，为了简单起见，我们就当作这个模型里还没有任何回忆，所以预测结果又直接进到了筛选路径。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide42.png "通过筛选路径前的「看见」和「非道格」")](https://youtu.be/WCUNPb-5EYI?t=1241)

由于筛选机制已经学习了「在前一个单字是名字的状况下，接下来的结果只能是『看见』或句号」这项规则，于是「非道格」这项预测就被挡掉了，剩下「看见」成为最终预测。

我们可以接着利用这个结果，开始下一个预测循环。在新的循环里，「看见」同时是新信息和旧预测，并和前一次循环一样，走过四条路径并形成新的预测。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide46.png "从「看见」开始的新循环")](https://youtu.be/WCUNPb-5EYI?t=1270)

由于「看见」才刚出现，我们预测下个字应该是「道格、珍、小点」其中之一。同样地，在这个简单的例子里我们可以先跳过忽视路径，并让这三个预测进入下个路径。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide50.png "准备和「非道格」抵销的初步预测")](https://youtu.be/WCUNPb-5EYI?t=1292)

这个路径也包括我们前一循环的预测结果，也就是「非道格」和「看见」。它们在前一回合中被保留了下来，并进到了遗忘闸门。

这时遗忘闸门的思路是：「嘿，既然前一个单字是『看见』，根据经验我可以将回忆中的『看见』忘掉，保留名字就好。」于是这条路经将前一次预测（看见、非道格）中的「看见」给忘了，并接着将「非道格」加入了初步的预测结果（道格、珍、小点）当中。

最后在这条路径里，「道格」的预测正负抵消，只留下了「珍」和「小点」继续前往下条路径。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide51.png "抵销后剩下「珍」和「小点」")](https://youtu.be/WCUNPb-5EYI?t=1342)

这里的筛选闸门已经知道当「看见」是前一个单字时，下个出现的单字应该是一个名字，所以它让「珍」和「小点」双双通过。

于是，在最后的预测结果中，我们得到了 「珍」和「小点」，但没有「道格」。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide52.png "「珍」和「小点」成为最终预测结果")](https://youtu.be/WCUNPb-5EYI?t=1363)

## LSTM 的其他应用

这个循环成功避免了先前「道格看见道格」的错误，并展示了 LSTM 模型如何借着回顾两个、三个甚至更多循环前的结果，以作出更合理的预测。不过话说回来，其实先前的基础 RNN 也能回顾几个循环前的结果，只是没有 LSTM 这么多。LSTM 可以成功回顾更多循环前的结果。

LSTM 模型对许多特别实用的应用来说很有帮助。例如，如果我想将某段话从一个语言翻译成另一个语言，LSTM 的表现相当良好。尽管翻译并非逐字、而是逐词组处理的过程， 有时甚至是逐句，LSTM 可以表现出某个语言中的文法架构。在以上说明的路径和步骤中，LSTM 模型似乎能捕捉某些更高层次的规则，并根据这些规则翻译不同语言。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide53.png "带串行性的数据")](https://youtu.be/WCUNPb-5EYI?t=1406)

另外 LSTM 也很擅长将语音转换为文本。由于语音指示随着时间变化的信号，LSTM 可以利用这些信号预测文本，并根据文本出现的次序更好地判断接下来的文本。LSTM 也因此擅长于任何和时间有关的信息，包括音频、影片，以及我最喜欢的**机器人学**（robotics）。由于机器人学基本上只是探讨**代理个体**（agent）根据一连串传感而得的信息所作出的判断和动作，这类数据本身就带有**串行性**（sequential），即不同时间内所采取的动作，也和之后的传感与判断有关。

如果读者还对 LSTM 的数学表达方式有兴趣，以下是 [Wikipedia 上的介绍](https://en.wikipedia.org/wiki/Recurrent_neural_network)。

[![](https://elham-khanche.github.io/blog/assets/img/RNN/Slide54.png "Wikipedia 上 RNN 的介绍")](https://youtu.be/WCUNPb-5EYI?t=1518)

虽然我不会详细介绍这些算式，但我鼓励读者可以体会一下这些看似复杂的算式，是如何勾勒出以上这个还满直观的流程。如果读者想了解更多，我也鼓励阅读你们读完 Wikipedia 上的介绍，并参考以下的讨论和教学。这些 LSTM 的解释应该也对读者很有帮助。

## 相关资源

- Chris Olah 的[教学文](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
- Andrej Karpathy 的[博客](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)、[RNN 原代码](https://gist.github.com/karpathy/d4dee566867f8291f086) 和 [Stanford CS231n 课程](https://www.youtube.com/watch?v=iX5V1WpxxkY)
- Deeplearning4J 的 [LSTM 教学文](https://deeplearning4j.org/lstm)中包含了一些很有帮助的讨论和一长串资源
- 〈[神经网络的运作原理](../how_machine_learning_works/how_neural_networks_work.html)〉（影片）

我也强烈推荐读者参考 Andrej Karpathy 的博客，其中包含了利用 LSTM 处理文本的例子。如果你还没读过〈[神经网络的运作原理](../how_machine_learning_works/how_neural_networks_work.html)〉，我也建议读者可以透过那篇文章了解更多细节，以及如何将神经网络化为实际的代码。

感谢收看，希望读者们都能顺利在自己的项目上应用 RNN！

Brandon，于 2017 年 6 月 27 日