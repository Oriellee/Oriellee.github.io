---
layout: post
title:  FCC算法题—Map the Debris
date:   2017-06-07 11:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/html.jpg # Add image post (optional)
tags: [FCC，算法]
author: Oriel # Add name author (optional)
---
**Map the Debris**

题目要求：返回一个数组，其内容是把原数组中对应元素的平均海拔转换成其对应的轨道周期.原数组中会包含格式化的对象内容，像这样 {name: 'name', avgAlt: avgAlt}.求得的值应该是一个与其最接近的整数，轨道是以地球为基准的.地球半径是 6367.4447 kilometers, 地球的GM值是 398600.4418, 圆周率为Math.PI

参考：
1. [轨道周期如何计算？](https://zh.wikipedia.org/wiki/%E8%BD%A8%E9%81%93%E5%91%A8%E6%9C%9F "轨道周期如何计算？")
2. [Math.pow()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/pow "Math.pow()")

这道题的难点关键在于轨道周期的计算公式。
```javascript
function getO(avg,GM,planetRadius){
    return Math.round(2*Math.PI*Math.sqrt(Math.pow(avg+planetRadius,3)/GM));
  }
```
完整代码：
```javascript
function orbitalPeriod(arr) {
	//定义变量。
  var GM = 398600.4418;
  var earthRadius = 6367.4447;
  var result = [];
	//求轨道周期的函数
  function getO(avg,GM,planetRadius){
    return Math.round(2*Math.PI*Math.sqrt(Math.pow(avg+planetRadius,3)/GM));
  }
	//遍历arr数组，将需要的变量添加到result数组中。
  arr.forEach(function(element){
    result.push({
      name:element.name,
      orbitalPeriod:getO(element.avgAlt,GM,earthRadius)
    });
  });
  return result;
}

orbitalPeriod([{name: "iss", avgAlt: 413.6}, {name: "hubble", avgAlt: 556.7}, {name: "moon", avgAlt: 378632.553}]);

```

转载请注明出处。