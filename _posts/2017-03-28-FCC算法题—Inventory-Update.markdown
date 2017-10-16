---
layout: post
title:  FCC算法题—Inventory Update
date:   2017-03-28 10:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/hacker.jpg # Add image post (optional)
tags: [FCC,算法]
author: Oriel # Add name author (optional)
---
## **Inventory Update**

**题目要求：依照一个存着新进货物的二维数组，更新存着现有库存(在 arr1 中)的二维数组. 如果货物已存在则更新数量 . 如果没有对应货物则把其加入到数组中，更新最新的数量. 返回当前的库存数组，且按货物名称的字母顺序排列.**
**这是一些对你有帮助的资源:**
	[**Global Array Object**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array "**Global Array Object**")

思路：分析题意，arr1和arr2都是二维数组，切都是键值对。我们要做的是比较两个键值对中的key，如果key相同，就把value相加，如果不同，就把该项加入到arr1中更新。

完整代码：
```javascript
function updateInventory(arr1, arr2) {
   arr2 = arr2.filter(function(temp){
     var res = true;
     for(var i=0;i<arr1.length;i++){
       if(temp[1] === arr1[i][1]){
         arr1[i][0]+=temp[0];
         res = false;
       }
     }
     return res;
   });
	//使用filter方法筛选arr2。如果arr1中包含有该项则使value相加，且在arr2中删除该项。
  arr1 = arr1.concat(arr2);
	//合并arr1和筛选后的arr2。
  arr1.sort(function(a,b){
    return a[1].charCodeAt(0)-b[1].charCodeAt(0);
  });
    return arr1;
}
	//给arr1排序且返回arr1.

// 仓库库存示例
var curInv = [
    [21, "Bowling Ball"],
    [2, "Dirty Sock"],
    [1, "Hair Pin"],
    [5, "Microphone"]
];
var newInv = [
    [2, "Hair Pin"],
    [3, "Half-Eaten Apple"],
    [67, "Bowling Ball"],
    [7, "Toothpaste"]
];

updateInventory(curInv, newInv);

```

转载请注明出处。