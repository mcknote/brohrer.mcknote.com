# 线性回归的运作原理

原文链接：[**How linear regression works**](https://brohrer.github.io/how_linear_regression_works.html)

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}fE0bnkNX77A{%endyoutube%}


**线性回归**（linear regression）是在数据点中找出规律、画出一条直线的专业说法，以下我将透过选购钻石的例子说明其运作原理。

故事是这样的：我的奶奶曾经留给我一只戒指。这个戒指上有个 1.35 克拉大小的镶台（setting），可惜上面没有安任何钻石。某天，我萌生了修复这只戒指的念头，所以我找了一间珠宝行询价，以了解我需要准备多少钱。

到了珠宝行以后，我发现店里既没有 1.35 克拉的钻石，也没有价格。但我没有因此打退堂鼓。我拿起了纸跟笔，把店里所有其他钻石的尺寸跟价格都抄了下来。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_1.png "左边是钻石的克拉数，右边是价格")](https://youtu.be/fE0bnkNX77A)

我发现绝大部分的钻石都在 2 克拉以下，所以我画了一条横轴以纪录钻石的重量。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_2.png "纪录钻石克拉数的横轴")](https://youtu.be/fE0bnkNX77A?t=1m10s)

接下来我画了一条用来记录价格的纵轴。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_3.png "加上纪录价格的纵轴")](https://youtu.be/fE0bnkNX77A?t=1m24s)

于是上图就有了座标系的两轴。在一座像曼哈顿有着网格道路系统（gridded streets）的城市里，读者可以循着南北向、东西向道路找出任何交叉路口；同理，在一个座标系里，读者可以利用横轴和纵轴上的位置锁定任何点。所以我们可以先根据钻石的重量，从纪录克拉数的横轴往上画一条直线，再根据钻石的价格，从纪录价格的纵轴往右画另一条直线。两条直线的交点，就是第一个钻石的数据。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_4.png "利用横轴跟纵轴锁定数据点")](https://youtu.be/fE0bnkNX77A?t=1m39s)

用同样的方法，我们可以把所有钻石画在这个座标系上。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_5.png "把所有钻石画在这个座标系上")](https://youtu.be/fE0bnkNX77A?t=2m08s)

于是一开始的表格变成了一张散点图。到目前为止，我没有增加或舍弃任何信息，我只是换了另一种表达方式，也就是散点图。从图片里我们可以看出一个明显的形状，好像有条很宽的直线往右上方延伸。所以我的下一步是在这个范围内将这条直线画出来。当这条直线穿过数据时，在它的上下两侧会分布着差不多数量的数据点。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_6.png "将贯穿数据点的直线画出来")](https://youtu.be/fE0bnkNX77A?t=3m25s)

将这条直线画出来是很关键的一步。虽然这条线在我们看来很明显，但这是因为我们早就具备了媲美超级电脑、擅长辨认特征的神经运算能力。在画这条线的时候，我们将原本的数据提炼成了更简单的形式，就像将真实的影像化约为简单的漫画（线条）。虽然在这一步，我确实舍弃了一些信息，但我也能利用这个简化模型回答前面的问题。找出符合数据规律的直线，就叫线性回归（[译注一](#译注)）。

有了线性模型以后，我终于可以回答前面的问题：「1.35 克拉的钻石多少钱？」要回答这个问题，我只需要用看的，先从横轴上的 1.35 克拉对到模型上，再从模型对到纵轴上，就能知道价格大约是 8,000 元。问题解决！

[![](https://brohrer.github.io/images/linear_regression/linear_regression_7.png "轻松看出 1.35 克拉的钻石卖 8,000 元")](https://youtu.be/fE0bnkNX77A?t=5m24s)

为了让这个估计值更符合现实情形，我注意到大部分的观测值并不直接落在模型的在线，这代表我要买的 1.35 克拉钻石大概也不会刚好是 8,000 元。所以一个很明显的问题是「实际价格会多接近 8,000 元？」为了了解这点，我在直线的两侧画了涵盖大部分（差不多 95%）观测值的范围。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_8.png "包含大约 95% 观测值的价格范围")](https://youtu.be/fE0bnkNX77A?t=6m00s)

如此一来，我可以说我有（大约 95% 的）信心认为未来任何钻石的价格和重量都会落在这个范围内。为了了解这和我要找的钻石有什么关系，我又沿着 1.35 克拉的垂直线，从价格范围的上下两端多看了两条水平线。

[![](https://brohrer.github.io/images/linear_regression/linear_regression_9.png "1.35 克拉钻石的价格范围")](https://youtu.be/fE0bnkNX77A?t=6m44s)

现在我满有自信地说：「我要找的钻石，价格不会低于 5,800，但也不会超出 10,200。」了解了这点以后，我就可以开始规划要花多久，定期从薪水中存入一笔「奶奶的钻戒修复基金」。

借着这个例子，我希望说明线性回归至少在观念上是个很简单的方法。任何人都可以用一支笔、一张餐巾纸和双眼完成线性回归分析，而不一定要使用电脑或数学知识。不过实务上具备数学知识还是很有用的。

在钻石的例子里，如果我搜集更多信息，例如颜色、净度、切割和内含物数量，此时数据的维度会从原本的两个增加为六个，也就更难化为图形。这时数学知识就能帮助我们在六个维度中「画出」一条直线（[译注二](#译注)）。

另一方面，如果上面的数据不只有 17 笔，而是 1,700 甚至 1,700 万笔，就算是最厉害的艺术家也很难从中画出直线，这时电脑就派上用场了。

Brandon，于 2016 年 12 月 20 日

## 译注

1. 「找出符合数据规律的直线，就叫线性回归」的原文为 *Finding the curve that best fits your data is called regression, and when that curve is a straight line, it's called linear regression.* 虽然我以前学的说法是「只要因变量为自变量的**线性组合**，就可以称作线性回归」，这代表就算线条不是直的，也可能是线性回归；但如果将包括多项式的回归细分作 [polynomial regression](https://en.wikipedia.org/wiki/Polynomial_regression)，将作者说法算作 [simple linear regression](https://en.wikipedia.org/wiki/Simple_linear_regression) 也没错。

2. 这边的数学知识应该以**线性代数**（linear algebra）为主，所使用的工具为矩阵（matrix）运算。


