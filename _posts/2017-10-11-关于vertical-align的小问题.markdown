---
layout: post
title:  关于vertical-align的小问题
date:   2017-10-11 11:00:00 +0800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: 201703/bg/行高.jpg # Add image post (optional)
tags: [BUG]
author: Oriel # Add name author (optional)
---
**经历了十一小长假，要开始上班了~脑壳疼。然而回来第一天就遇到个小问题。想起来以前遇见过很多次。这里就记录一下。不知道有没有人遇见过这么一个bug，那就是当你在很高兴的使用div或者span写并排按钮时，当其中一个按钮填写文字后出现了三个span或者div高度不一致的问题**

![截图]({{site.baseurl}}/assets/img/201703/行高.png)

有没有觉得很头疼？你明明都已经吧margin和padding设置为0了。使用chrome开发者工具查看也没有问题。那么这个问题到底是怎么造成的呢？

这时候我们调节字体的大小。会发现第一个span掉落的高度随之增加。
现在我们就会发现问题所在了——字体。

现在我们试着使用vertical-align直接修改文字的重心位置：


    vertical-align:top

bug好了！。
其实这里的原因是因为浏览器中vertica-align默认为baseline，就是“display属性inline或inline-block”的控件的底端跟文字的baseline（基线）对齐。所以当没有文字的span和有文字的span基线对齐后，高度就不一致了。

这里贴一下：[vertical-align](http://www.w3school.com.cn/cssref/pr_pos_vertical-align.asp "vertical-align")的属性。

    1.  baseline    默认。元素放置在父元素的基线上。
    2.     sub 垂直对齐文本的下标。
    3.     super   垂直对齐文本的上标
    4.     top 把元素的顶端与行中最高元素的顶端对齐
    5.     text-top    把元素的顶端与父元素字体的顶端对齐
    6.     middle  把此元素放置在父元素的中部。
    7.     bottom  把元素的顶端与行中最低的元素的顶端对齐。
    8.     text-bottom 把元素的底端与父元素字体的底端对齐。
    9.     length   
    10.     %   使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。
    11.     inherit 规定应该从父元素继承 vertical-align 属性的值。

转载请注明出处。