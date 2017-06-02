#反向傳播的運作原理

原文標題：**How backpropagation works** 
{% raw %}
<script>
var defaultEncoding = 1; // 預設語言：1-繁體中文 | 2-简体中文
var translateDelay = 0;
var cookieDomain = "https://brohrer.mcknote.com";	// 修改爲你的部落格地址
var msgToTraditionalChinese = "轉換爲繁體";	// 簡轉繁時所顯示的文字
var msgToSimplifiedChinese = "转换为简体"; 	// 繁转简时所显示的文字
var translateButtonId = "translateLink";	// 「轉換」<A>鏈接標籤ID
translateInitilization();
</script>
<a id="translateLink"></a>
{% endraw %}

Translated from Brandon Rohrer's Blog by Jimmy Lin

{%youtube%}ILsA4nyG7I0?t=12m39s{%endyoutube%}

> 請注意，本文內容為[神經網路](../how_machine_learning_works/how_neural_networks_work.md)的影片片段，從 12 分 39 秒開始。完整影片尚未被翻譯。