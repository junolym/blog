---
title: 正则表达式等价判断（Beta）
permalink: regexp-isequals
id: 69
updated: '2017-03-09 15:16:37'
date: 2016-12-28 11:53:55
---

采用暴力碰撞测试方法，如果两个正则表达式被相当长度的字符串碰撞成功，就认为两正则表达式等价。  
相比化为最小DFA再判断的优点是不存在算法错误导致的误判。

<form action='#' id='form'>
字符集：<input type="text" name="s" /> 例如：['a','b']<br><br>
正则表达式1： <input type="text" name="re1" /> 例如：b*(ab?)*<br><br>
正则表达式2： <input type="text" name="re2" /> 例如：b*a*(aba*)*<br><br>
测试长度：<input type="text" name="max" value="15" /><br><br>
<input type="button" value="测试" onclick='start()'/><br>
结果：<p id="result"></p>
</form>

<script type="text/javascript">
    var f = document.getElementById('form');
    function start() {
        var s = eval(document.getElementsByName('s')[0].value);
        var re1 = eval('/^'+document.getElementsByName('re1')[0].value+'$/');
        var re2 = eval('/^'+document.getElementsByName('re2')[0].value+'$/');
        var maxLength = parseInt(document.getElementsByName('max')[0].value);
        var msg = document.getElementById('result');
        msg.innerHTML = '测试中';
        msg.innerHTML = isEquals(re1, re2, s, maxLength) ? '等价' : '不等价，字符串：' + failed +
         ': 1' + (re1.test(failed) ? '匹配' : '不匹配') + ' 2' + (re2.test(failed) ? '匹配' : '不匹配');
    }

    var maxLength = 15;
    var s = [];
    var failed = "";

    function isEquals(re1, re2, s, maxLength) {
        var q = [''];
        while (q.length) {
            var st = q[0];
            if (re1.test(st) != re2.test(st)) {
                failed = st;
                return false;
            }
            if (st.length < maxLength) {
                for (var i in s) {
                    q.push(st+s[i]);
                }
            }
            q.shift();
        }
        return true;
    }
    function isEquals2(re1, re2, _s, _maxLength) {
        s = _s || s;
        maxLength = _maxLength || maxLength;
        return test(re1, re2, '');
    }
    function test(re1, re2, st) {
        if (st.length > maxLength)
            return true;
        if (re1.test(st) == re2.test(st)) {
            var pass = true;
            for (var i in s) {
                pass = pass && test(re1, re2, st+s[i]);
                if (!pass)
                    break;
            }
            return pass;
        }
        failed = st;
        return false;
    }
</script>