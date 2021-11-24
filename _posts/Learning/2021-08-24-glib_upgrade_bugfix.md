---
layout:     post
title:      "关于glibc库升级带来的relocation error问题"
subtitle:   "glibc"
date:       2021-08-24 15:50
author:     "Xiaohan"
header-img: "img/Learning-mysql.jpg"
category: Learning
catalog: true
tags:
    - Mysql
    - Bug
    - root
    - password
---

> 经验贴

## 事故起因

为了安装perl 库的一个包不小心升级了整个系统的glib库导致了relocation error

![](001.png)
![](002.png)

## 解决过程

解决过程中参考了这几位大神的答案总算解决了，下面是解决方案。

> 原因是升级了glibc的库后导致了原来的程序不可以使用，因此降级。

* 创建临时软连接，使得ls命令可以使用：

根据每个人不同的老版本可以选择不同的版本号

```shell
export LD_PRELOAD=/lib64/libc-2.17.so
```
* 查看现在的连接和所有的版本号

```shell
ls -ltr
```

从2.17升级到了2.19因此改变软连接

```shell
sln /usr/lib64/libc-2.17.so /lib64/libc.so.6
```

* 还是报错并且是miniconda3

那就到miniconda3中的lib库里找寻

```shell
ls -ltr | grep "2.19"
```

![](003.png)

找到相关的2.19的库与软连接，可以从lib64从将对应的2.17版本拷贝过来，重新连接

```shell
cp /lib64/libcrypt-2.17.so ./
cp /lib64/libcidn-2.17.so ./
cp /lib64/libc-2.17.so ./
cp /lib64/libBrokenLocale-2.17.so ./
cp /lib64/libanl-2.17.so ./
cp /lib64/ld-2.17.so ./
cp /lib64/libutil-2.17.so ./
cp /lib64/librt-2.17.so ./
cp /lib64/libresolv-2.17.so ./
cp /lib64/libpthread-2.17.so ./
cp /lib64/libnss_nisplus-2.17.so ./
cp /lib64/libnss_nis-2.17.so ./
cp /lib64/libnss_hesiod-2.17.so ./
cp /lib64/libnss_files-2.17.so ./
cp /lib64/libnss_dns-2.17.so ./
cp /lib64/libnss_db-2.17.so ./
cp /lib64/libnss_compat-2.17.so ./
cp /lib64/libnsl-2.17.so ./
cp /lib64/libm-2.17.so ./

sln libanl-2.17.so		libanl.so.1
sln ld-2.17.so		ld-linux-x86-64.so.2
sln libdl-2.17.so		libdl.so.2
sln libc-2.17.so		libc.so.6
sln libcrypt-2.17.so		libcrypt.so.1
sln libcidn-2.17.so		libcidn.so.1
sln libnss_compat-2.17.so		libnss_compat.so.2
sln libnsl-2.17.so		libnsl.so.1
sln libnss_nis-2.17.so		libnss_nis.so.2
sln libnss_hesiod-2.17.so		libnss_hesiod.so.2
sln libnss_files-2.17.so		libnss_files.so.2
sln libnss_dns-2.17.so		libnss_dns.so.2
sln libnss_db-2.17.so		libnss_db.so.2
sln libresolv-2.17.so		libresolv.so.2
sln libpthread-2.17.so		libpthread.so.0
sln libnss_nisplus-2.17.so		libnss_nisplus.so.2
sln libutil-2.17.so		libutil.so.1
sln librt-2.17.so		librt.so.1
sln libm-2.17.so		libm.so.6

```

* 完成

## 不能瞎升级

还好不补救回来了，当时导致了服务器瘫痪，什么命令都用不了，先搜索了一下，在不关闭窗口的情况下才能解决。<br>
<br>
先登录了别人的账号查看不影响其他人，然后着手解决自己的问题，终于完成了！<br>
<br>
这块的知识还要补充～～<br>

> 希望本文可以对你有些帮助！
