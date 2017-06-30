---
title: Jiathis 分享按钮测试
permalink: jiathis-fen-xiang-an-niu-ce-shi
id: 23
updated: '2016-08-02 06:16:27'
date: 2016-04-26 14:43:54
tags:
---

代码如下（{yourId}为用户id）

```html line-numbers
<!-- JiaThis Button BEGIN -->
<div class="jiathis_style_32x32">
    <a class="jiathis_button_qzone"></a>
    <a class="jiathis_button_tsina"></a>
    <a class="jiathis_button_tqq"></a>
    <a class="jiathis_button_weixin"></a>
    <a class="jiathis_button_renren"></a>
    <a href="http://www.jiathis.com/share?uid={yourId}" class="jiathis jiathis_txt jtico jtico_jiathis" target="_blank"></a>
    <a class="jiathis_counter_style"></a>
</div>
<script type="text/javascript">
var jiathis_config = {data_track_clickback:'true'};
</script>
<script type="text/javascript" src="http://v3.jiathis.com/code/jia.js?uid={yourId}" charset="utf-8"></script>
<!-- JiaThis Button END -->
```

效果如下：

<!-- JiaThis Button BEGIN -->
<div class="jiathis_style_32x32" style="height: 40px;">
	<a class="jiathis_button_qzone"></a>
	<a class="jiathis_button_tsina"></a>
	<a class="jiathis_button_tqq"></a>
	<a class="jiathis_button_weixin"></a>
	<a class="jiathis_button_renren"></a>
	<a href="http://www.jiathis.com/share?uid=2097190" class="jiathis jiathis_txt jtico jtico_jiathis" target="_blank"></a>
	<a class="jiathis_counter_style"></a>
</div>
<script type="text/javascript">
    var jiathis_config = {data_track_clickback:'true'};
</script>
<script type="text/javascript" src="http://v3.jiathis.com/code/jia.js?uid=2097190" charset="utf-8"></script>
<!-- JiaThis Button END -->

___

