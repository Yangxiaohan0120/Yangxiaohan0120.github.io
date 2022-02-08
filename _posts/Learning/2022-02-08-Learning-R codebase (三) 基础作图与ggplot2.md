---
layout:     post
title:      "R codebase（三）基础作图与ggplot2"
subtitle:   "使用R处理数据的转换"
date:       2022-02-07 22:31
author:     "Xiaohan"
header-img: "img/Rscripts.jpg"
category: Learning
catalog: true
tags:
    - R codebase
---

> 段首语

此系列文章用来做R语言的学习，以及对于使用R语言进行数据处理和作图的代码汇总，方便大家随时进行查找、使用。

上一篇：[R codebase (二) 数据转换](https://yangxiaohan0120.github.io/learning/2022/02/07/Learning-R-codebase-(二)-数据转换)

下一篇：[R codebase (四) 基本数据分析](https://yangxiaohan0120.github.io/learning/2022/02/07/Learning-R-codebase-(四)-基本数据分析)


## 一、base R 基础作图

### 图形创建

#### 1.图形的创建

* 颜色准备


```r
library(RColorBrewer)
```

* 直方图 histogram

```r
data(VADeaths) 
hist(VADeaths,breaks=10, col=brewer.pal(3,"Set3"),main="Set3 3 colors")
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

* 线性图 line chart


```r
plot(AirPassengers,type="l")
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

* 柱状图 bar chart


```r
barplot(iris$Petal.Length) #Creating simple Bar Graph 
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

```r
barplot(table(iris$Species,iris$Sepal.Length),col = brewer.pal(3,"Set1"))
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-4-2.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

* 盒形图 boxplot


```r
data(iris) 
boxplot(iris$Sepal.Length,col="red")
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

```r
boxplot(iris$Sepal.Length~iris$Species,col=topo.colors(3,alpha = 0.8))
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-5-2.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

* 散点图


```r
plot(x=iris$Petal.Length) 
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

```r
plot(x=iris$Petal.Length,y=iris$Species)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-6-2.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

```r
plot(iris,col=brewer.pal(3,"Set1"))
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-6-3.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

* 六边形密度图 Hexbin Binning


```r
library(hexbin)
library(ggplot2)
## Need help getting started? Try
## the R Graphics Cookbook:
## https://r-graphics.org
diamonds <- diamonds
a=hexbin(diamonds$price,diamonds$carat,xbins=40)
plot(a)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

```r
rf <- colorRampPalette(rev(brewer.pal(40,'Set3')))
## Warning in brewer.pal(40, "Set3"): n too large, allowed maximum for palette Set3 is 12
## Returning the palette you asked for with that many colors
hexbinplot(diamonds$price~diamonds$carat, data=diamonds, colramp=rf)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-7-2.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

*马赛克图（Mosaic Plot），也叫做不等宽柱状图(Marimekko Chart)


```r
data(HairEyeColor)
mosaicplot(HairEyeColor)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

* 热图


```r
head(mtcars)
##                    mpg cyl disp
## Mazda RX4         21.0   6  160
## Mazda RX4 Wag     21.0   6  160
## Datsun 710        22.8   4  108
## Hornet 4 Drive    21.4   6  258
## Hornet Sportabout 18.7   8  360
## Valiant           18.1   6  225
##                    hp drat    wt
## Mazda RX4         110 3.90 2.620
## Mazda RX4 Wag     110 3.90 2.875
## Datsun 710         93 3.85 2.320
## Hornet 4 Drive    110 3.08 3.215
## Hornet Sportabout 175 3.15 3.440
## Valiant           105 2.76 3.460
##                    qsec vs am gear
## Mazda RX4         16.46  0  1    4
## Mazda RX4 Wag     17.02  0  1    4
## Datsun 710        18.61  1  1    4
## Hornet 4 Drive    19.44  1  0    3
## Hornet Sportabout 17.02  0  0    3
## Valiant           20.22  1  0    3
##                   carb
## Mazda RX4            4
## Mazda RX4 Wag        4
## Datsun 710           1
## Hornet 4 Drive       1
## Hornet Sportabout    2
## Valiant              1
heatmap(as.matrix(mtcars))
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />
* 地图 Map

```r
## devtools::install_github("rstudio/leaflet")
library(magrittr) 
## Warning: package 'magrittr' was
## built under R version 4.0.5
## 
## Attaching package: 'magrittr'
## The following object is masked from 'package:tidyr':
## 
##     extract
library(leaflet)
m <- leaflet() %>% 
  addTiles() %>% # Add default OpenStreetMap map tiles 
  addMarkers(lng=77.2310, lat=28.6560, popup="food of chandni chowk")
m
## Error in loadNamespace(name): there is no package called 'webshot'
```

* 3D

```r
data(iris, package="datasets")
library(car)
## Loading required package: carData
scatter3d(Petal.Width~Petal.Length+Sepal.Length|Species, data=iris, fit="linear",residuals=TRUE, parallel=FALSE, bg="black", axis.scales=TRUE, grid=TRUE, ellipsoid=FALSE)
## Loading required namespace: rgl
## Loading required namespace: mgcv

library(lattice)
attach(iris)# 3d scatterplot by factor level
  cloud(Sepal.Length~Sepal.Width*Petal.Length|Species, main="3D Scatterplot by Species")
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" style="display: block; margin: auto;" />

```r
  xyplot(Sepal.Width ~ Sepal.Length, iris, groups = iris$Species, pch= 20)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-11-2.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" style="display: block; margin: auto;" />
* 相关性图 Correlogram (GUIs)

```r
cor(iris[1:4])
##              Sepal.Length
## Sepal.Length    1.0000000
## Sepal.Width    -0.1175698
## Petal.Length    0.8717538
## Petal.Width     0.8179411
##              Sepal.Width
## Sepal.Length  -0.1175698
## Sepal.Width    1.0000000
## Petal.Length  -0.4284401
## Petal.Width   -0.3661259
##              Petal.Length
## Sepal.Length    0.8717538
## Sepal.Width    -0.4284401
## Petal.Length    1.0000000
## Petal.Width     0.9628654
##              Petal.Width
## Sepal.Length   0.8179411
## Sepal.Width   -0.3661259
## Petal.Length   0.9628654
## Petal.Width    1.0000000
library(corrgram)
## 
## Attaching package: 'corrgram'
## The following object is masked from 'package:lattice':
## 
##     panel.fill
corrgram(iris)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" style="display: block; margin: auto;" />

#### 2.添加线条

#### 3.增加绘图窗口

#### 4.增加一条曲线

* 多项式回归

#### 5.添加点

#### 6.添加图例

#### 7.添加文字

#### 8.精确定位

#### 9.图形保存

### 图形定制

#### 1.字符、文本

#### 2.符号和线条

#### 3.颜色

#### 4.图形尺寸和边界尺寸

#### 5.标题

#### 6.坐标轴

#### 7.参考线

#### 8.图例

#### 9.添加多边形

#### 10.平滑散点

#### 11.绘制显式表达式的函数

* 放大曲线的一部分

#### 12.文本标注

#### 13.数学标注

#### 14.图形的组合

### 三维图形


## 二、ggplot2作图

### 基本图类型

### 分组

### 刻面

### 添加光滑曲线

### 外观修改

#### 1.坐标轴

#### 2.图例

#### 3.标尺

#### 4.主题

#### 5.多重图

### 图形保存

> 持续更新 。。。
