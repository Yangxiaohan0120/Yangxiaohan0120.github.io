---
layout:     post
title:      "R codebase（三）基础作图与ggplot2"
subtitle:   "使用R处理数据的转换"
date:       2022-02-09 22:31
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
##                    mpg cyl disp  hp drat    wt  qsec
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02
## Valiant           18.1   6  225 105 2.76 3.460 20.22
##                   vs am gear carb
## Mazda RX4          0  1    4    4
## Mazda RX4 Wag      0  1    4    4
## Datsun 710         1  1    4    1
## Hornet 4 Drive     1  0    3    1
## Hornet Sportabout  0  0    3    2
## Valiant            1  0    3    1
heatmap(as.matrix(mtcars))
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />
* 地图 Map

```r
## devtools::install_github("rstudio/leaflet")
library(magrittr) 
## Warning: package 'magrittr' was built under R version
## 4.0.5
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
# scatter3d(Petal.Width~Petal.Length+Sepal.Length|Species, data=iris, fit="linear",residuals=TRUE, parallel=FALSE, bg="black", axis.scales=TRUE, grid=TRUE, ellipsoid=FALSE)

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
##              Sepal.Length Sepal.Width Petal.Length
## Sepal.Length    1.0000000  -0.1175698    0.8717538
## Sepal.Width    -0.1175698   1.0000000   -0.4284401
## Petal.Length    0.8717538  -0.4284401    1.0000000
## Petal.Width     0.8179411  -0.3661259    0.9628654
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

### 图形定制

#### 1.字符、文本

* 文字大小

|参数|描述|
|:---|:---|
|cex|相对于默认大小的缩放倍数|
|cex.axis|坐标轴刻度文字的缩放倍数|
|cex.lab|坐标轴标签的缩放倍数|
|cex.main|标题的缩放倍数|
|cex.sub|副标题的缩放倍数|

* 文字字体

|参数|描述|
|:---|:---|
|font|用于指定绘图所用的字体样式。1=常规，2=粗体，3=斜体，4=粗斜体，5=符号字体（以Adobe符号编码表示）|
|font.axis|坐标轴刻度字体样式|
|font.lab|坐标轴标签字体样式|
|font.main|标题的字体样式|
|font.sub|副标题的字体样式|
|ps|字体磅值，文本的最终大小为ps*cex|
|family|绘制文本时使用的字体族，serif(衬线),sans(无衬线),mono(等宽)|

#### 2.符号和线条

|参数|描述|
|:---|:---|
|pch|符号类型|
|cex|符号大小|
|lty|线条类型|
|lwd|线条宽度|

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/1.png)
![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/2.png)

```r
dose <- c(20,30,40,50,60)
drugA <- c(16,20,28,40,60)
plot(dose,drugA,type = "b",lty=3,lwd=3,pch=15,cex=2)
```

<img src="https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" style="display: block; margin: auto;" />

#### 3.颜色

|参数|描述|
|:---|:---|
|col|默认绘图颜色|
|col.axis|坐标轴刻度文字颜色|
|col.lab|坐标轴标签文字颜色|
|col.main|标题颜色|
|col.sub|副标题颜色|
|fg|图形的前景色|
|bg|图形的后景色|

* 使用颜色下标、名称，十六进制、RGB、或者HSV表示

col=1,col="white",col=#FFFFFF".col=rgb(1,1,1)和col=hsv(0,0,1)

* 函数


```r
## colors() 可返回所有可用的颜色名称
head(colors())
## [1] "white"         "aliceblue"     "antiquewhite" 
## [4] "antiquewhite1" "antiquewhite2" "antiquewhite3"

rainbow(10)
##  [1] "#FF0000" "#FF9900" "#CCFF00" "#33FF00"
##  [5] "#00FF66" "#00FFFF" "#0066FF" "#3300FF"
##  [9] "#CC00FF" "#FF0099"

heat.colors(10)
##  [1] "#FF0000" "#FF2400" "#FF4900" "#FF6D00"
##  [5] "#FF9200" "#FFB600" "#FFDB00" "#FFFF00"
##  [9] "#FFFF40" "#FFFFBF"

terrain.colors(10)
##  [1] "#00A600" "#2DB600" "#63C600" "#A0D600"
##  [5] "#E6E600" "#E8C32E" "#EBB25E" "#EDB48E"
##  [9] "#F0C9C0" "#F2F2F2"

topo.colors(10)
##  [1] "#4C00FF" "#0019FF" "#0080FF" "#00E5FF"
##  [5] "#00FF4D" "#4DFF00" "#E6FF00" "#FFFF00"
##  [9] "#FFDE59" "#FFE0B3"

cm.colors(10)
##  [1] "#80FFFF" "#99FFFF" "#B3FFFF" "#CCFFFF"
##  [5] "#E6FFFF" "#FFE6FF" "#FFCCFF" "#FFB3FF"
##  [9] "#FF99FF" "#FF80FF"
```

* R 包

RColorBrewer


```r
library(RColorBrewer)
brewer.pal.info ## 展示所有颜色
##          maxcolors category colorblind
## BrBG            11      div       TRUE
## PiYG            11      div       TRUE
## PRGn            11      div       TRUE
## PuOr            11      div       TRUE
## RdBu            11      div       TRUE
## RdGy            11      div      FALSE
## RdYlBu          11      div       TRUE
## RdYlGn          11      div      FALSE
## Spectral        11      div      FALSE
## Accent           8     qual      FALSE
## Dark2            8     qual       TRUE
## Paired          12     qual       TRUE
## Pastel1          9     qual      FALSE
## Pastel2          8     qual      FALSE
## Set1             9     qual      FALSE
## Set2             8     qual       TRUE
## Set3            12     qual      FALSE
## Blues            9      seq       TRUE
## BuGn             9      seq       TRUE
## BuPu             9      seq       TRUE
## GnBu             9      seq       TRUE
## Greens           9      seq       TRUE
## Greys            9      seq       TRUE
## Oranges          9      seq       TRUE
## OrRd             9      seq       TRUE
## PuBu             9      seq       TRUE
## PuBuGn           9      seq       TRUE
## PuRd             9      seq       TRUE
## Purples          9      seq       TRUE
## RdPu             9      seq       TRUE
## Reds             9      seq       TRUE
## YlGn             9      seq       TRUE
## YlGnBu           9      seq       TRUE
## YlOrBr           9      seq       TRUE
## YlOrRd           9      seq       TRUE
display.brewer.all() ## 打开调色板
```

![plot of chunk unnamed-chunk-15](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-15-1.png)

```r
mycolor <- brewer.pal(5,"Set1") ## 调用颜色
mycolor
## [1] "#E41A1C" "#377EB8" "#4DAF4A" "#984EA3" "#FF7F00"
```

#### 4.图形尺寸和边界尺寸

|参数|描述|
|:----|:-----|
|pin|以英寸表示图形尺寸|
|mai|以数值向量表示边界大小，“下，左，上，右”，单位英寸|
|mar|以数值向量表示边界大小，“下，左，上，右”，单位英分|

#### 5.标题

title()


```r
title(main="main title",sub= "sub title",xlab = "x-axis lable",ylab = "y-axis lable")
```

#### 6.坐标轴

axis()


```r
axis(side,at = ,labels = ,pos = ,lty = ,col = ,Las=,tck=, ……)
```

|||
|:---|:---|
|side|整数，表示绘制坐标轴的位置（1=下，2=左，3=上，4=右）|
|at|数值型向量，表示需要绘制刻度线的位置|
|labels|字符型向量，表示置于刻度线旁边的文字标签,(如果为Null，则默认使用at()中的值|
|pos|坐标轴线绘制位置的坐标(相交点的坐标)｜
|lty|线条类型|
|col|线条颜色与刻度线颜色|
|las|标签是否平行于（=0）或垂直于（=2）坐标轴（标签值较长的情况）|
|tck|刻度线长度，负数为外侧，正数为内侧，0表示禁用，1表示绘制网格线，默认-0.01|
|xlim|x轴的范围|
|ylim|y轴的范围|

axes=FALSE 禁用坐标轴， yaxt="n",xaxt="n" 分别禁用y轴和x轴


```r
## 举例

x <- c(1:10)
y <- x
z <- 10/x
opar <- par(no.readonly = T)

par(mar=c(5,4,4,8)+0.1)
plot(x,y,type="b",pch=21,col="red",yaxt="n",lty=3,ann=FALSE)
lines(x,z,type="b",pch=22,col="blue",lty=2)
axis(2,at=x,labels = x,col.axis="red",las=2)
axis(4,at=z,labels = round(z,digits = 2),col.axis="blue",las=2,cex.axis=0.7,tck=-0.01)
mtext("y=1/x",side=4,line=3,cex.lab=1,las=2,col="blue")
title("An Example of Creative Axes",xlab = "X Values",ylab = "Y = X")
```

![plot of chunk unnamed-chunk-18](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-18-1.png)

```r
par(opar)
```

#### 7.参考线

abline(h=yvalues,v=xvalues)为图形添加参考线


```r
plot(x,y,type="b",pch=21,col="red",lty=3)
abline(h=c(1.5,7))
abline(v=seq(1,10,2),lty=2,col="blue")
```

![plot of chunk unnamed-chunk-19](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-19-1.png)

#### 8.图例

legend(location,title,legend,...)

|选项|描述|
|:---|:---|
|location|1，指定坐标；2，使用关键字（bottom，topright 等），可同时使用inset=指定向图形内侧移动的大小|
|title|标题|
|legend|标签|

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/3.png)
![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/4.png)

#### 9.文本标注

text(location,"text to place",pos,...)
mtext("text to place",side,line=n,...)

|选项|描述|
|:---|:---|
|location|文本位置|
|pos|相对于位置的方位，1=下，2=左，3=上，4=右，使用offset=指定偏移量|
|side|文本的边，lines=内移或外移文本，adj=0文本左下对齐，adj=1右上对齐|


```r
attach(mtcars)
```

```
## The following object is masked from package:ggplot2:
## 
##     mpg
```

```r
plot(wt,mpg,
     main = "Mileage vs,. Car weight",
     xlab = "Weight",ylab = "Mileage",
     pch=18,col="blue")
text(wt,mpg,
     row.names(mtcars),
     cex=0.6,pos=4,col = "red")
```

![plot of chunk unnamed-chunk-20](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-20-1.png)

```r
detach(mtcars)
```

#### 10.数学标注

plotmath()添加数学符号

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/5.png)

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/6.png)

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/7.png)

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/8.png)

![](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/Rplot/9.png)

#### 11.图形的组合

* par()

使用参数mfrow=c(nrows,ncols) or mfcol=c(nrows,ncols)


```r
attach(mtcars)
```

```
## The following object is masked from package:ggplot2:
## 
##     mpg
```

```r
opar <- par(no.readonly = T)
par(mfrow=c(3,1))
hist(wt)
hist(mpg)
hist(disp)
```

![plot of chunk unnamed-chunk-21](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-21-1.png)

```r
par(opar)
detach(mtcars)
```

* layout()

layout(mat),使用mat指定图形位置（所在行）


```r
attach(mtcars)
```

```
## The following object is masked from package:ggplot2:
## 
##     mpg
```

```r
layout(matrix(c(1,1,2,3),2,2,byrow = T))
hist(wt)
hist(disp)
hist(mpg)
```

![plot of chunk unnamed-chunk-22](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-22-1.png)

```r
detach(mtcars)
```


#### 12.添加多边形

polygon()


```r
f <- function(x) return(1-exp(-x))
curve(f,0,2)
polygon(c(1.2,1.4,1.4,1.2),c(0,0,f(1.3),f(1.3)),col = "gray")
```

![plot of chunk unnamed-chunk-23](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-23-1.png)

#### 12.平滑散点

lowess() 与 loess() 函数


```r
datatest <- data.frame(x=mtcars$mpg,y=mtcars$disp)
plot(datatest)+
lines(lowess(datatest))
```

![plot of chunk unnamed-chunk-24](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-24-1.png)

```
## integer(0)
```

#### 13.绘制显式表达式的函数

对于具有一定函数关系的曲线图的绘制

1、将其中的数抽样进行plot


```r
g <- function(t) {return (t^2+1)^0.5 }
x <- seq(0,5,length=10000)
y <- g(x)
plot(x,y,type="l")
```

![plot of chunk unnamed-chunk-25](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-25-1.png)

2、直接绘制函数图像（add=T，在原有图形上添加）


```r
curve((x^2+1)^0.5,0,5)
```

![plot of chunk unnamed-chunk-26](https://raw.githubusercontent.com/Yangxiaohan0120/Yangxiaohan0120.github.io/main/figure/unnamed-chunk-26-1.png)

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
