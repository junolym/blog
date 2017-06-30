---
title: 最新评论
permalink: test
id: 26
updated: '2016-11-20 14:12:26'
date: 2016-05-05 07:41:01
---

<button onclick="C();LoadDs();">chenjx.cn</button>
<button onclick="G();LoadDs();">guso.ml</button>
<!-- 多说最新评论 start -->
<div class="ds-recent-comments" data-num-items="5" data-show-avatars="1" data-show-time="1" data-show-title="1" data-show-admin="1" data-excerpt-length="70"></div>
<!-- 多说最新评论 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
function C() {
duoshuoQuery = {short_name:"chenjxcn"};
};
function G() {
duoshuoQuery = {short_name:"guso"};
};
function LoadDs() {
ds = document.createElement('script');
ds.type = 'text/javascript';ds.async = true;
ds.src = '//static.duoshuo.com/embed.js';
ds.charset = 'UTF-8';
document.getElementsByTagName('head')[0].appendChild(ds);
};
function toggleDuoshuoComments(container){
    var el = document.createElement('div');
    el.setAttribute('data-thread-key', '文章的本地ID');//必选参数
    el.setAttribute('data-url', '你网页的网址');//必选参数
    el.setAttribute('data-author-key', '作者的本地用户ID');//可选参数
    DUOSHUO.EmbedThread(el);
    jQuery(container).append(el);
}
</script>
<!-- 多说公共JS代码 end -->