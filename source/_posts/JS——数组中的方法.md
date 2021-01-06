---
title: JS——数组中的方法
date: 2017-06-30 18:16:20
categories: js
tags:
---

JS中数组的方法有哪些呢，我们一起来看看。

<!--more-->
### 检测数组
* Array.isArray(obj) 判断一个对象是否是数组，是返回true，否则返回false。
<pre><code>
    var numbers = 3;
    var arr = [1,2,3];
    console.log(Array.isArray(numbers));        //false
    console.log(Array.isArray(arr));        //true
</code></pre>

### 转换方法
* `join()` 将数组转为字符串。如果数组中的某一项的值是`null`或者`undefined`，返回的结果中以空字符串表示。
<pre><code>
    var arr = ['aa','bb','cc'];
    var str = arr.join();
    console.log(str);   //aabbcc
</code></pre>

### 栈方法
* `push()` 将元素ele添加到数组的末尾，返回数组的新长度
<pre><code>
    var arr = ['aa','bb'];
    arr.push('qq');
    console.log(arr);   //aa,bb,qq
</code></pre>
* `pop()` 删除数组末尾的元素，返回移除的项
<pre><code>
    var arr = [1,2,3,4];
    arr.pop();
    console.log(arr);   1,2,3
</code></pre>

### 队列方法
* `shift()` 删除数组第一个元素，返回移除的项
<pre><code>
    var arr = [5,1,2,3,4];
    arr.shift();
    console.log(arr);   1,2,3,4
</code></pre>
* `unshift()` 将元素添加到数组的开头，可以传递多个参数，返回数组的新长度
<pre><code>
    var nums = [3,4,5];
    nums.unshift(1,2);
    console.log(nums);      //1,2,3,4,5
</code></pre>
`shift()`和`push()`结合使用；`unshift()`和`pop()`结合使用。

### 重排序方法
* `reverse()` 返回排序后的数组，会影响原数组

* `sort()`返回排序后的数组，会影响原数组
    <pre><code>
        var values = [0,1,5,10,15];
        values.sort();
        alert(values);      //0,1,10,15,5
    </code></pre>

### 操作方法
* `concat()`：不影响原数组
    `var colors2 = colors.concat("yellow",["black","brown"];`
* `slice()`: 不影响原数组。基于当前数组创建一个新数组。接收一或两个参数，即要返回项的起始和结束位置，包含起始项，不包含结束项。
    如果`slice()`方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。比如一个长度为5的数组上调用slice(-2,-1)与调用slice(3,4)得到的结果是一样的，如果结束位置小于起始位置，则返回空数组。
* `splice()`：会影响原数组。返回数组中删除的项（没有删除元素则返回空数组）
    `splice()`的3种用法：
    * 删除：指定2个参数：要删除的第一项的位置和要删除的项数。
    * 插入：至少3个参数：起始位置、0（要删除的项数）和要插入的项。插入多项后面传第四、第五以至任意多个项。
    * 替换：至少3个参数：起始位置、要删除的项数和要插入的任意数量的项。
    <pre><code>
        var colors = ["red","green","blue"];
        var removed = colors.splice(0,1);
        alert(colors);      //green,blue
        alert(removed);     //red
        ///////////////////////////////////////////////
        removed = colors.splice(1,0,"yellow","orange");
        alert(colors);      //green,yellow,orange,blue
        alert(removed);     //空数组
        ///////////////////////////////////////////////
        removed = colors.splice(1,1,"red","purple");
        alert(colors);      //green,red,purple,orange,blue
        alert(removed);     //yellow
    </code></pre>

### 位置方法
* `lastIndexOf()` 在数组中查找传入的参数str是否存在，存在返回该参数在数组中的索引，否则返回-1。如果存在多个相同的元素，返回第一个与参数相同的元素的索引。
<pre><code>
    arr.indexOf(str);
</code></pre>
* `lastIndexOf()` 返回最后一个与参数相同的元素的索引，不存在则返回-1

这两个方法都接收两个参数：要查找的项和（可选）表示查找起点位置的索引。

### 迭代方法
每个方法都接收2个参数：要在每一项上运行的函数和（可选）运行该函数的作用域对象——影响this的值。传入这些方法中的函数会接收3个参数：数组项的值、该项在数组中的位置和数组对象本身。

* `every()` 对数组中的每一项运行给定函数，如果该函数对每一项都返回`true`，则返回`true`。
* `fliter()` 对数组中的每一项运行给定函数，返回该函数会返回`true`的项组成的数组。
* `forEach()` 对数组中的每一项运行给定函数。这个方法没有返回值。
* `map()` 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
* `some()` 对数组总的每一项运行给定函数，如果该函数对任一项返回`true`，则返回`true`。
以上方法都不会修改数组中包含的值。
<pre><code>
    var numbers = [1,2,3,4,5,4,3,2,1];
    var everyResult = numbers.every(function(item,index,array){
        return (item>2);
    });
    //可以将every换成以上任一一个方法
</code></pre>

### 缩小方法
每个方法都会迭代数组的所有项，然后构建一个最终返回的值。都接收2个参数：一个在每一项上调用的函数和（可选）作为缩小基础的初始值。传人这两个方法中的函数接收4个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。
* `reduce()`从数组的第一项开始，逐个遍历到最后
<pre><code>
    var values = [1,2,3,4,5];
    var sum = values.reduce(function(pre,cur,index,array){
        return pre + cur;
    });
    alert(sum);     //15
</code></pre>
* `reduceRight()`则从数组的最后一项开始，向前遍历到第一项。
