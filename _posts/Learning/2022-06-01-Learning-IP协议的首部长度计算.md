---
layout:     post 
title:      "IP协议之首部长度的计算"
subtitle:   "最短20个字节，最长60个字节"
date:       2022-06-01 12:00
author:     "Xiaohan"
header-img: "img/post-twopointers-linkedlist.jpg"
category: Learning 
catalog: true 
tags:
    - 计算机网络
    - IP协议
---

> 今天和大家聊一聊什么如何计算IP协议中的首部长度

## 首先什么是首部：

IP协议的首部指的是IP报文中除去后面实际的IP数据后，前方用来标记IP的信息，称之为IP首部

## 那么什么是首部长度呢？

首部长度是一个4bit的数据，用来记录IP协议首部的长度，这样我们在读取信息的时候，才知道哪里是IP数据内容的开始

## 那么首部长度为什么有变化的呢？

IP报文的结构如图所示，其中有一个options选项，而这一部分的长度的改变也导致了首部长度数值的改变。

![Untitled](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/twopointers/10.png)

## 那么首部长度应该如何计算呢？

首先我们要知道首部长度的记录都是按照4个字节的单位进行增减的，所以我们算出4bit的首部长度的值之后，乘以4个字节，就可以知道首部的长度了，一个字节代表了8bit

## 最短长度的计算：

首部最短就是没有options这一项，那么也有五行必要的信息，每行信息有32bit，4个字节，因此总共有 5 * 4 = 20 个字节，这时首部长度的表示应为0101 = 5

## 最长长度的计算：

首部长度只占到4bit，因此4bit可以表示的范围是0～15，所以最大值是15，那么最长就是15 * 4 = 60 个字节，此时首部长度的表示应为1111 = 15

* 知道了最短和最长长度之后我们要思考，首部长度的4个bit可以代表16个数字，也就代表了16中长度，然而由于IP协议的前五行是必要信息，所以小于5的值也就不会出现了，像是0000，0001，0010，0011。