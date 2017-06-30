---
title: Google
permalink: google
id: 65
updated: '2017-03-06 05:53:29'
date: 2016-11-27 14:59:04
---

<style type="text/css">
    .post-title, .footer { display: none; }
    .container { padding-left: 0; padding-right: 0; }
    #body { display:table;width:100%;height:100%;margin:0; }
    #search { width:600px;margin:auto; }
    #searchBox { width:95%; height:34px; margin:20px auto 25px auto; font-size:18px; padding-left: 10px;}
    .mybtn { padding:0px 15px;height:36px;font-size:13px;font-weight:bold;background:#f2f2f2;border:1px solid #f2f2f2;color:#757575;border-radius:2px;}
    .mybtn:hover { border:1px solid #c6c6c6;box-shadow:0 1px 1px rgba(0,0,0,0.1);color:#222; }
    .mybtn[value="Google 搜索"] { margin: 0 3px 0 3px; }
    #content { display:table-cell; vertical-align:middle; text-align:center; padding: 120px 0;}
</style>
<div id="body">
<div id="content">
    <img id="logo-img" width="272px" src="https://www.google.cn/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png"/>
    <form id="search" action="https://guso.ml/search" method="get" target="_top" onsubmit="return q.value!=''">
        <input type="text" id="searchBox" name="q" autocomplete="off" />
        <select id="selMir" class="mybtn">
            <option value ="https://guso.ml/search">镜像 1</option>
            <option value ="https://g.namaho.com/search">镜像 2</option>
            <option value ="https://ss.wtf/search">镜像 3</option>
        </select>
        <input type="submit" class="mybtn" value="Google 搜索" />
        <input type="submit" class="mybtn" name="btnI" value="手气不错" />
    </form>
</div>
</div>
<script type="text/javascript">
    var f = document.getElementById("search");
    var s = document.getElementById("selMir");
    if (document.cookie && (i=document.cookie.indexOf("selected=")) >= 0) {
        s.selectedIndex = parseInt(document.cookie.slice(i+9, i+10))
    }
    (window.onresize = function() {
        var w = document.documentElement.clientWidth;
        f.style.width=((w-20)>600 ? 600 : w-20) +"px";
        document.getElementById("logo-img").style.width=(w>500 ? 272: w*0.55) +"px";
        s.options[0].value = "https://" + (w < 600 ? "m." : "") + "guso.ml/search";
        s.options[1].value = "https://" + (w < 600 ? "m." : "") + "guso.ml:8443/search";
        f.action = s.options[s.selectedIndex].value;
    })();
    (s.onchange = function() {
        f.action = s.options[s.selectedIndex].value;
        var t = new Date();
        t.setDate(t.getDate()+0.5);
        document.cookie = "selected=" + s.selectedIndex + "; expires=" + t;
    })();
    if (document.documentElement.clientWidth >= 600 && document.documentElement.clientHeight >= 600) {
        document.getElementById("searchBox").focus();
    }
</script>

# 这是测试页面，正式版--> https://guso.gq/
