---
title: JS数组之你不知道的事
date: 2017-09-18 22:01:08
categories: js
tags:
---

js的splice()方法以及数组元素为数字类型的排序

<!--more-->
1)我们知道，如果要删除数组中的一个元素或者在数组指定位置添加一个元素，那么，后面的元素都要做相应的移动，这样的操作无疑是非常麻烦的。然而，splice()方法可以帮助我们执行这些操作。
splice(index,delCount,nums)
	index:起始索引（也就是你希望添加或者删除元素的起始位置）
	delCount:需要删除元素的个数（添加元素时设置为0）
	nums:要添加的元素（删除元素时，不用设置这个参数）
让我们看两个具体的例子：
eg.在数组中插入元素
	var nums = [1,2,6,7,8];
	nums.splice(2,0,3,4,5);
	console.log(nums);	//1,2,3,4,5,6,7,8
   删除数组中的元素
	var nums = [1,2,30,3,4,5];
	nums.splice(2,1);
	console.log(nums);	//1,2,3,4,5

2)要对数组中的元素进行排序时，我们首先会想到sort()。如果元素是字符串类型，sort()方法真的非常好使，但是，如果数组元素是数字类型，sort()方法也会把它当成字符串来处理，导致出现错误的结果。对于元素是数字类型的数组，我们可以做以下处理。
function compare(num1,num2){
	return num1-num2;	//由小到大排列
	//return num2-num1;	//由大到小排列
}
var nums = [6,8,1,2,3,9];
nums.sort(compare);
console.log(nums);	//1,2,3,6,8,9

