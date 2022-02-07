---
layout:     post
title:      "Base r 中的一些基本操作"
subtitle:   "test page"
date:       2022-02-07 10:31
author:     "Xiaohan"
header-img: "img/code001.jpg"
category: Learning
catalog: true
status: process
tags:
    - Binary search
    - 数据结构
---

# Fig 

## Test


```r
library(ggplot2)
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✔ tibble  3.1.6     ✔ dplyr   1.0.7
## ✔ tidyr   1.1.3     ✔ stringr 1.4.0
## ✔ readr   1.4.0     ✔ forcats 0.5.1
## ✔ purrr   0.3.4
```

```
## ── Conflicts ───────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
ggplot(mpg, aes(hwy, cty)) +
    geom_point(aes(color = cyl)) + geom_smooth(method ="lm")
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png)
