---
layout: post
title:  FCC算法题—Exact Change
date:   2017-03-22 10:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/freecodecamp_3.png # Add image post (optional)
tags: [FCC,算法]
author: Oriel # Add name author (optional)
---
## **Exact Change**

**题目要求：设计一个收银程序 checkCashRegister() ，其把购买价格(price)作为第一个参数 , 付款金额 (cash)作为第二个参数, 和收银机中零钱 (cid) 作为第三个参数.cid 是一个二维数组，存着当前可用的找零.当收银机中的钱不够找零时返回字符串 "Insufficient Funds". 如果正好则返回字符串 "Closed".否则, 返回应找回的零钱列表,且由大到小存在二维数组中.**

**这是一些对你有帮助的资源:
[Global Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object "Global Object")**

思路：
  - 先将钱数替换单位。
  - 从大额的钱数开始找零，依次向下减。

完整代码：
```javascript
function checkCashRegister(price, cash, cid) {
  cash = cash*100;
  price = price*100;
  //将美元转换成美分
  var change = cash-price,
      changeLeft = change,
      total = getTotal(cid),
      result = [];
  //分别定义找零的钱数、收银机余额以及一个数组。
  function getTotal(cid){
    var total = 0;
    for(var i=0;i<cid.length;i++){
      total += cid[i][1]*100;
    }
    return total;
  }
  //获取总钱数。
  function getValue(coinName){
    switch(coinName){
      case "PENNY":
        return 1;
      case "NICKEL":
        return 5;
      case "DIME":
        return 10;
      case "QUARTER":
        return 25;
      case "ONE":
        return 100;
      case "FIVE":
        return 500;
      case "TEN":
        return 1000;
      case "TWENTY":
        return 2000;
      case "ONE HUNDRED":
        return 10000;
    }
  }
  //获取对应的美分
    if(change>total){
      return "Insufficient Funds";
    }else if(change === total){
      return  "Closed";
    }
    //如果收银机钱数正好或者不够则返回Insufficient Funds或者Closed。
    for(var i = cid.length-1;i>=0;i--){
      var coinName = cid[i][0],
          coinTotal = cid[i][1]*100,
          coinValue = getValue(coinName),
          coinAmount = coinTotal/coinValue,
          toReturn = 0;
      while(changeLeft>=coinValue&&coinAmount>0){
        changeLeft-=coinValue;
        coinAmount--;
        toReturn++;
      }
  //依次减去钱数
      if(toReturn>0){
        result.push([coinName,toReturn*coinValue/100]);
       
      }
    }
    if(getTotal(result) !== change){
       return "Insufficient Funds";
    }
  return result;
}

checkCashRegister(19.50, 20.00, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.10], ["QUARTER", 4.25], ["ONE", 90.00], ["FIVE", 55.00], ["TEN", 20.00], ["TWENTY", 60.00], ["ONE HUNDRED", 100.00]]);

```

转载请注明出处。