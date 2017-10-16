---
layout: post
title:  FCC算法题—Symmetric Difference
date:   2017-03-11 09:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/freecodecamp.png # Add image post (optional)
tags: [FCC，算法]
author: Oriel # Add name author (optional)
---
## **Symmetric Difference**

**题目要求：创建一个函数，接受两个或多个数组，返回所给数组的 对等差分(symmetric difference) (△ or ⊕)数组.给出两个集合 (如集合 A = {1, 2, 3} 和集合 B = {2, 3, 4}), 而数学术语 "对等差分" 的集合就是指由所有只在两个集合其中之一的元素组成的集合(A △ B = C = {1, 4}). 对于传入的额外集合 (如 D = {2, 3}), 你应该安装前面原则求前两个集合的结果与新集合的对等差分集合 (C △ D = {1, 4} △ {2, 3} = {1, 2, 3, 4}).**

根据题目的要求FCC给出了两个有用的资源链接：
- [Array.reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce "Array.reduce()")
- [Symmetric Difference](https://www.youtube.com/watch?v=PxffSUQRkG4 "Symmetric Difference")

根据这两个资源链接我们大概可以了解到本题主要考察的是对于reduce()方法的考察。对于调用函数传回的参数，我们可以使用[arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments "arguments")来处理。其中最关键的部分是去重的部分。

完整代码：
````javascript
function sym(args) {
  var arr = [];
  for(var i=0;i<arguments.length;i++){
    arr.push(arguments[i]);
  }
  var temp = arr.reduce(function(prev,cur,index,array){
    var a = prev.filter(function(item){
      return cur.indexOf(item)<0;
    });
    var b = cur.filter(function(item){
      return prev.indexOf(item)<0;
    });
    return a.concat(b);
  });
  return temp.filter(function(item,index,array){
    return array.indexOf(item) == index;
  });
}
sym([1, 2, 3], [5, 2, 1, 4]);
````
转载请注明出处。