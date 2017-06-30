---
title: 技术支持——Ghost博客
tags: |-

  - ghost
permalink: blog-help
id: 41
updated: '2016-08-02 13:29:16'
date: 2016-06-08 09:01:53
---

**本文收集所有使用ghost博客中的学习到的技术和一些心得体会，长期更新**

## Markdown

Markdown是Ghost博客使用的编辑文章的语法。关于Markdown的技术文档到处都能找到。例如：http://support.ghost.org/markdown-guide/ 。需要注意的是，并不是所有语法Ghost都支持。Ghost官方给了一份很详细的使用说明，基本涵盖了所有常用的Markdown语法。当你搭建好了Ghost博客，这份文档就会是你的第一篇博文。在编辑页面能够很直观地看到Markdown和对应的效果。

这就是那篇默认创建的博文，我一直都留着：[Welcome to Ghost](http://chenjx.cn/welcome-to-ghost/)。

使用中可能会忽略的一点是：插入图片的语法是
`![图片名](图片链接)`，而如果图片是需要上传的，只要链接留空，就能在编辑框右边的预览框上传图片了。上传后会自动补全链接。

## 社会化评论框

本站使用的是**友言**社会化评论框。官网链接：http://www.uyan.cc/ 。在官网可以看到介绍和使用说明，嵌入Ghost的方式非常简单，只需要将html代码插入到你所使用的主题里的博客页面，一般是blog.hbs 。

需要注意的是，有许多Ghost的主题采用了单页应用的架构（SPA），例如：[bleak](https://github.com/zutrinken/bleak)。如果你使用的是这种主题，友言的评论框不会在每一次页面跳转都重新加载。解决的方法是在引入友言的js文件之前加上一句js代码，如下，添加第3行。
```
<!-- UY Begin -->  
<div id="uyan_frame"></div>  
<script type="text/javascript">uyan_loaded = uyan_loadover = undefined</script>  
<script type="text/javascript" src="http://v2.uyan.cc/code/uyan.js?uid={yourID}"></script>  
<!-- UY End --> 
```

另外，同样推荐友言的兄弟：社会分享按钮[JiaThis](http://www.jiathis.com/)和推荐引擎[友荐](http://www.ujian.cc/)。

## 代码高亮

本站代码高亮的插件来自[highlight.js](https://highlightjs.org/)。在本页就有实例。使用方法非常简单，推荐以下的做法：

1、在下载页面 https://highlightjs.org/download/ 勾选你想要高亮的语言，然后下载js文件，并放到自己的网站。如果只要使用几种基本的语言，可以直接引用官网给的cdn链接。

2、在demo页面 https://highlightjs.org/static/demo/ 选择喜欢的高亮风格，并下载对应的css文件放到自己的网站，或者引用链接。

3、在页面添加以下html代码启用高亮。
```html
<link rel="stylesheet" href="/path/to/styles/default.css">
<script src="/path/to/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

4、这时页面上插入的代码应该已经自动高亮了。如果需要指定使用的高亮类型，只需要在```后面加上代码类。例如：
```
 // markdown语法
 ```js
 // 这里插入js代码
 ```
```

官方使用说明：https://highlightjs.org/usage/

推荐看看官方的API说明：http://highlightjs.readthedocs.io/en/latest/api.html

**代码显示行号**

查阅官方的API得知highlight不提供显示行号的功能，需要自己添加。本站添加行号的方法请看：[网页代码高亮并显示行号](http://chenjx.cn/code-line-number#linenumbers) 

## 标签云

截至发文，标签云是Ghost的实验室功能，默认不启用。需要在设置里开启，如下图：
![](/content/images/2016/06/----20160608183156.png)

然后在你想要显示标签云的地方加入类似下面的代码：
```html
{{#get "tags" limit="all" include="count.posts" order="count.posts desc" }}
    {{#foreach tags}}
        <a href="/tag/{{slug}}" class="tag-item">
        <div>{{name}} ({{count.posts}})</div></a>
    {{/foreach}}
{{/get}}
```

注意可能需要重启ghost。然后标签会按照数量和更新时间倒序输出。