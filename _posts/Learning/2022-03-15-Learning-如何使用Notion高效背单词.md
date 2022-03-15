---
layout:     post 
title:      "如何使用Notion高效背单词"
subtitle:   "艾宾浩斯遗忘曲线"
date:       2022-03-15 20:00
author:     "Xiaohan"
header-img: "img/Notion01.jpg"
category: Learning 
catalog: true 
tags:
    - Notion
---

> 今天给大家介绍一个我常使用的笔记工具Notion，以及我是如何用它来背单词的。

## Notion

Notion是一个集成化，自由化的工具，可以有网页版，PC版，iPad版，手机版app，具有双向链接表可以进行自己的知识库建设。

在这里给大家介绍一下我是如何使用Notion来背单词的。

## 工具

Notion、欧路词典、Excel

## 单词来源

这里主要是记录我平时日常阅读会学到的单词

## 使用方法

#### 1.创建自己的单词表格

首先我会创建一个记录每日本单词的表格：

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/001.png)

主要属性包括：

单词的释义和音标，创建时间，背词进度，复习时间和复习打卡

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/002.png)

#### 2.背词时间与背词打卡机制

根据单词被创建的日期，会随之创建六个属性，分别代表着1，3，7，15，30，60天后需要对单词进行复习的日期。

同时建立打卡的6个属性，使用checkbox可以进行单词背记的标识。

使用背词进度来记录自己的复习情况：

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/003.png)

#### 3.每日的单词记背

对于每日要背记的单词，新建六个View

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/004.png)

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/005.png)

每个通过properties来进行筛选，只展示单词、释义、和当日打卡

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/006.png)


通过Filter只显示当日需要打卡的单词，选择第一层过滤，过滤单词未被标记的，打卡日期在当天的

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/007.png)

##### 4.背记

每天打开需要背的单词的View视角进行背记后标记背好的单词就可以了！！

## 每日单词哪里来？

这个时候就要祭出英语学习大杀器：欧路词典！

在欧路词典进行设置：

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/008.png)

设置使用辅助取词，并且搜索新单词的时候自动加入生词本，这样你就可以记录每天自己学到的新单词喽

## 如何导入Notion

> 难道要一个个手敲吗？

当然没有那么笨拙喽！

这个时候就需要打开欧路词典的管理 --> 生词本，将我们刚刚加入生词本的单词，进行导出就好了

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/009.png)

选择导出为简明解释，导出到csv中

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/010.png)

那么之后我们只需要在Notion中快捷粘贴就可以喽

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/011.png)

## 使用的小trick

我一般会使用7个视图（View）= 1个edit + 6个当天复习（分别包含了不同天数复习的单词）

在edit中添加新的单词，背单词的时候在六个当天复习的单词里依次背诵即可，当然了越靠后的时间间隔越长，复习起来就越不熟悉了。

## 相关的属性与公式

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/img/in-post/NotionWord/012.png)

Name：单词 （Text）

释义：（Text）

音标：（Text）

date：创建日期（created date）

已完成：完成次数（Formula）

```
toNumber(prop("打卡1")) + toNumber(prop("打卡2")) + toNumber(prop("打卡3")) + toNumber(prop("打卡4")) + toNumber(prop("打卡5")) + toNumber(prop("打卡6"))
```

背词进度：（Formula）

```
if(round(prop("已完成") / 6 * 100) == 100, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 100 and round(prop("已完成") / 6 * 100) >= 95, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌗 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 95 and round(prop("已完成") / 6 * 100) >= 90, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 90 and round(prop("已完成") / 6 * 100) >= 85, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌗 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 85 and round(prop("已完成") / 6 * 100) >= 80, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 80 and round(prop("已完成") / 6 * 100) >= 75, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌗 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 75 and round(prop("已完成") / 6 * 100) >= 70, "🌕 🌕 🌕 🌕 🌕 🌕 🌕 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 70 and round(prop("已完成") / 6 * 100) >= 65, "🌕 🌕 🌕 🌕 🌕 🌕 🌗 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 65 and round(prop("已完成") / 6 * 100) >= 60, "🌕 🌕 🌕 🌕 🌕 🌕 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 60 and round(prop("已完成") / 6 * 100) >= 55, "🌕 🌕 🌕 🌕 🌕 🌗 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 55 and round(prop("已完成") / 6 * 100) >= 50, "🌕 🌕 🌕 🌕 🌕 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 50 and round(prop("已完成") / 6 * 100) >= 45, "🌕 🌕 🌕 🌕 🌗 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 45 and round(prop("已完成") / 6 * 100) >= 40, "🌕 🌕 🌕 🌕 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 40 and round(prop("已完成") / 6 * 100) >= 35, "🌕 🌕 🌕 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 35 and round(prop("已完成") / 6 * 100) >= 30, "🌕 🌕 🌕 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 30 and round(prop("已完成") / 6 * 100) >= 25, "🌕 🌕 🌗 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 25 and round(prop("已完成") / 6 * 100) >= 20, "🌕 🌕 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 20 and round(prop("已完成") / 6 * 100) >= 15, "🌕 🌗 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 15 and round(prop("已完成") / 6 * 100) >= 10, "🌕 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 10 and round(prop("已完成") / 6 * 100) >= 5, "🌗 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 🌑 " + format(round(prop("已完成") / 6 * 100)) + "%", if(round(prop("已完成") / 6 * 100) < 5 and round(prop("已完成") / 6 * 100) >= 0, "⛽️ 快开始任务吧", "0")))))))))))))))))))))
```

复习1-7：（Formula）大家可以根据需要填写不同的天数，我用的是（1，3，7，15，30，60）

```
dateAdd(prop("date"), 1, "days")
```

打卡：（checkbox）

建立打卡1-7的checkbox就可以了

## 想偷个懒咋办

这里我把自己的表格模版贴到这里了哦，有需要的同学可以自取哦，复制粘贴到自己的Notion笔记本中就可以开始使用了哦。

[分享](https://plastic-yumberry-49d.notion.site/27dd84a21aff4443a8dd88268d402095?v=241344a30fcf498d9d9bf6ac0ef35cc8)

