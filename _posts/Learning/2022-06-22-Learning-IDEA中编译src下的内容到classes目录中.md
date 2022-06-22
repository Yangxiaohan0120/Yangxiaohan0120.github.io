---
layout:     post 
title:      "IDEA中编译src下的内容到classes目录中"
subtitle:   "IDEA编译"
date:       2022-06-22 10:00
author:     "Xiaohan"
header-img: "img/post-twopointers-linkedlist.jpg"
category: Learning 
catalog: true 
tags:
    - 编译
    - IDE
---

在我们使用IDEA开发的时候，往往需要用到getResource函数，去获取类中的一些资源

```Java
dataFile = XmlDataSource.class.getResource("/painting.xml").getPath();
```

比如我们使用getResource函数去获取根目录中的painting.xml中的编写好的内容。

但是当我们将文件放入src目录下，却总是报NullPointerException，找不到路径，找不到文件。

这是什么原因呢？

> 编译

但是我们知道Java是一种介于解释型语言和编译型语言之间的面相对象语言，需要先编译成class文件之后，再交给JVM执行。

所以我们所需要的painting.xml需要也编译到classes目录下，才可以从所谓的“根目录”下找到这个内容。

> IDEA的编译

但是IDEA只能将src目录下的.java文件编译到classes目录下，因此我们需要再pom.xml文件中进行设置

```Java
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes><include>**/*.ftl</include></includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes><include>**/*.xml</include></includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
    </build>
```

将java包下的内容设置为资源，包括所有的xml，ftl文件等，这样就可以在运行的时候被编译到classes目录下，也就可以从根目录找到了