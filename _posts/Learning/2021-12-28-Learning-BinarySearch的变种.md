---
layout:     post
title:      "Base r 中的一些基本操作"
subtitle:   "test page"
date:       2022-02-07 10:31
author:     "Xiaohan"
header-img: "img/code001.jpg"
category: Learning
catalog: true
output:
  html_document:
    keep_md: yes
tags:
    - Binary search
    - 数据结构
---

# Fig 

## Test

```{r}
library(ggplot2)
library(tidyverse)
ggplot(mpg, aes(hwy, cty)) +
    geom_point(aes(color = cyl)) + geom_smooth(method ="lm")
```