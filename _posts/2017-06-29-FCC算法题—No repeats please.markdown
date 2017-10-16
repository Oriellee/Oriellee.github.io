---
layout: post
title:  FCC算法题—No repeats please
date:   2017-06-29 11:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/html2.jpg # Add image post (optional)
tags: [FCC，算法]
author: Oriel # Add name author (optional)
---
**No repeats please**

题目要求：把一个字符串中的字符重新排列生成新的字符串，返回新生成的字符串里没有连续重复字符的字符串个数.连续重复只以单个字符为准。例如, aab 应该返回 2 因为它总共有6中排列 (aab, aab, aba, aba, baa, baa), 但是只有两个 (aba and aba)没有连续重复的字符 (在本例中是 a).

参考：
1. [Permutations](https://www.mathsisfun.com/combinatorics/combinations-permutations.html "Permutations")
2. [RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp "RegExp")

首先，把第一个字符和其后面的字符一一交换。
接着，固定第一个字符，求后面所有字符的排列。这个时候我们仍把后面的所有字符分成两部分：后面字符的第一个字符，以及这个字符之后的所有字符。然后把第一个字符逐一和它后面的字符交换。
去重的全排列就是从第一个数字起每个数分别与它后面非重复出现的数字交换。

完整代码：
```javascript
function permAlone(str) {
   var regex = /(.)\1+/g;
  var arr = str.split(""),
      permutations = [],
      tmp;
  //交换相邻的字符。
  function swap(a,b){
    tmp = arr[a];
    arr[a] = arr[b];
    arr[b] = tmp;
  }
  function generate(int){
    if(int ===1){
      permutations.push(arr.join(""));
    }else{
      for(var i=0;i!=int;++i){
        generate(int-1);
        swap(int%2?0:i,int-1);
      }
    }
  }
generate(arr.length);
  //使用正则表达式筛选。
  var filtered = permutations.filter(function(string){
    return !string.match(regex);
  });
  return filtered.length;
}

permAlone("aab");

```

转载请注明出处。