---
title: JS中的字符串方法
date: 2018-12-01 19:44:24
categories: js
tags:
---
JS中的字符串方法有哪些呢？来列举一下吧~
<!--more-->

### 字符方法
* `charAt()` 接收一个参数，即基于0的字符位置。返回给定位置的字符
* `charCodeAt()` 接收一个参数，即基于0的字符位置。返回给定位置的字符的编码
* `stringValue()` 与`charAt()`类似
<pre><code>
    var stringValue = "hello world";
    alert(stringValue.charAt(1));   //"e"
    alert(stringValue.charCodeAt(1));   //101
    alert(stringValue[1]);   //"e"
</code></pre>

### 字符串操作方法
* `concat()` 用于将一或多个字符串拼接起来，返回拼接后的新字符串。不影响原字符串
* `slice()` 返回新字符串，不影响原字符串
* `substr()` 返回新字符串，不影响原字符串
* `substring()` 与`slice()`和`substr()`一样，返回被操作字符串的一个子字符串，不影响原字符串。
    <pre><code>
        var stringValue = "hello world";
        alert(stringValue.slice(3));    //"lo world"
        alert(stringValue.substring(3));    //"lo world"
        alert(stringValue.substr(3));    //"lo world"
        alert(stringVlue.slice(3,7));   //"lo w"
        alert(stringVlue.substring(3,7));   //"lo w"
        alert(stringVlue.substr(3,7));   //"lo worl"
        //传入负数时
        alert(stringValue.slice(-3));    //"rld"
        alert(stringValue.substring(-3));    //"hello world"
        alert(stringValue.substr(-3));    //"rld"
        //将负数与字符串长度相加
        alert(stringVlue.slice(3,-4));   //"lo w"
        //将所有负数都转为0，会交换位置
        alert(stringVlue.substring(3,-4));   //"hel"
        //第一个参数是负数加上字符串长度，第二个参数是负数转为0
        alert(stringVlue.substr(3,-4));   //""
    </code></pre>

### 字符串位置方法
* `indexOf()`
* `lastIndexOf()`
接收2个参数，要查找的项和（可选）表示查找起点位置的索引。

### 删除空格
* `trim()` 删除字符串前置和后缀的所有空格。
<pre><code>
    var str = "     hello world     ";
    var strValue = str.trim();
    console.log(str);   //"     hello world     "
    console.log(strValue);      //"hello world"
</code></pre>

### 字符串大小写转换方法
* `toLowerCase()` 返回新字符串，不影响原字符串
* `toUpperCase()` 返回新字符串，不影响原字符串

### 字符串的模式匹配方法
* `match()`只接收一个参数，要么是正则表达式，要么是一个RegExp对象。
    <pre><code>
        var text = "cat, bat, sat, fat";
        var pattern = /.at/;
        //与pattern.exec(text)相同
        var matches = text.match(pattern);
        alert(matches.index);   //0
        alert(matches[0]);      //cat
        alert(matches.lastIndex);   //undefined
        alert(text);    //"cat, bat, sat, fat"
    </code></pre>
* `search()`只接收一个参数，与`match()`一样。返回第一个匹配项的索引，没有找到返回-1。
    <pre><code>
        var text = "cat, bat, sat, fat";
        var pos = text.search(/at/);
        alert(pos);     //1
    </code></pre>
* `replace()`接收2个参数：第一个参数可以是一个`RegExp`对象或者一个字符串，第二个参数可以是一个字符串或者一个函数。
    <pre><code>
        var text = "cat, bat, sat, fat";
        var result = text.replace("at","ond");
        alert(result);      //cond, bat, sat, fat
        result = text.replace(/at/g,"ond");
        alert(result);      //cond, bond, sond, fond
        console.log(text);  //"cat, bat, sat, fat"
    </code></pre>
    如果第二个参数是字符串，还可以使用一些特殊的字符序列，将正则表达式操作得到的值插入到结果字符串中。下表列出了`ECMAScript`提供的这些特殊的字符序列。<br>
    <img src="/images/js/15.png">
    <pre><code>
        var text = "cat, bat, sat, fat";
        result = text.replace(/(.at)/g,"word ($1)");
        alert(result);  //word (cat), word (bat), word (sat), word (fat)
    </code></pre>
* `split()` 基于指定的分割符，将数组分割成多个字字符串。接收2个参数，第一个可以是字符串，也可以是RegExp对象，第二个参数可选，用于指导数组的大小。
    <pre><code>
        var text = "cat, bat, sat, fat";
        var val = text.split(", ");
        console.log(val);   //["cat"," bat"," sat"," fat"];
        console.log(text);  //"cat, bat, sat, fat"
    </code></pre>
    
### 比较两个字符串
* `localeCompare()`
<pre><code>
    var stringValue = "yellow";
    alert(stringValue.localeCopare("brick"));       //1
    alert(stringValue.localeCopare("yellow"));      //0
    alert(stringValue.localeCopare("zoo"));         //-1
</code></pre>

### 编码转字符串
* `fromCharCode()` 接收一或多个字符编码，然后将它们转换成一个字符串。
<pre><code>
    alert(String.fromCharCode(104,101,108,108,111);    //hello
</code></pre>