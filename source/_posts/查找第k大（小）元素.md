---
title: 查找第k大（小）元素
date: 2019-09-27 22:19:26
categories: 算法
tags:
---
查找第K大元素，我首先想到的是先给数据排序，然后找打第K个元素就是了。转念一想，应该没这么简单，网上找了一下，果然......

<!--more-->
### 算法描述
快速选择算法：实际上就是修改后的快速排序算法。其主要思想就是在快速排序中得到划分结果之后，判断要求的第k个数是在划分结果的左边还是右边，然后只处理对应的那一部分，从而降低复杂度。

### 代码实现
```
function Partition(arr, i, j){
    var temp = arr[i];
    if(i<j){
        while(i < j){
            while( i < j && arr[j] >= temp ){
                j--;
            }
            if(i < j){
                arr[i] = arr[j];
            }
            while( i < j && arr[i] < temp ){
                i++;
            }
            if(i < j){
                arr[j] = arr[i];
            }
        }
        arr[i] = temp;
        return i;
    }
}

function Search(arr, i, j, k){
    var m = Partition(arr, i, j);
    if( k == m-i+1){
        return arr[m];
    }else if( k < m-i+1){
        return Search(arr, i, m-1, k);
    }else{
        return Search(arr, m+1, j, k-(m-i+1));
    }
}

var num = [1,2,3,4,5,6,7,8];
var k = 3;
console.log(Search(num, 0, 7, k));
```