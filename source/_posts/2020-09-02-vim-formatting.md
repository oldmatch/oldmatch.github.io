---
thumbnail: https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/59ac2e13d0d8dcdd16c581b706b77b51_1.jpg
title: vim中自动格式化代码
date: 2020-09-02 10:39
tags:
  - linux
  - vim
categories:
  - 分享
---

在vim中其实也有像Eclipse中的ctrl + shift +F 的自动格式化代码的操作，尽管非常强大，但是通常会破坏代码的原有的缩进，
所以不建议在python这样缩进代替括号的语言中和源程序已经缩进过的代码中使用，废话少说，下面说步骤：

#### 步骤

* gg 跳转到第一行
* shift+v 转到可视模式
* shift+g 全选
* 按下神奇的 =
你会惊奇的发现代码自动缩进了，呵呵，当然也可能是悲剧了