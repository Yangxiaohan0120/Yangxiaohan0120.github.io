---
layout:     post
title:      "关于Mysql root 密码修改的bug"
subtitle:   ""
date:       2021-08-24 15:50
author:     "Xiaohan"
header-img: "img/Borges.jpg"
category: Learning
catalog: true
tags:
    - Mysql，Bug，root，password
---

> 经验贴

## 非root用户安装mysql
* 下载安装包
* 解压及编写配置文件
* 安装MySql
* 启动MySql
* 登录MySql

一、下载安装包<br>

https://dev.mysql.com/downloads/
<br>
下载对应的软件包

二、解压及编写配置文件

tar -zxvf mysql-8.0.26-linux-glibc2.12-x86_64
mv mysql-8.0.26-linux-glibc2.12-x86_64 mysql

配置文件如下：

```
#添加以下内容
[client]   
port=3345  
socket=/data1/home/xiaohan/myprogram/mysql/mysql.sock  
[mysqld]
#服务端口号
port=3345
#mysql安装根目录
basedir=/data1/home/xiaohan/myprogram/mysql
#mysql数据文件所在位置
datadir=/data1/home/xiaohan/myprogram/mysql/data
#mysql进程文件
pid-file=/data1/home/xiaohan/myprogram/mysql/mysql.pid
#设置socke文件所在目录
socket=/data1/home/xiaohan/myprogram/mysql/mysql.sock
#数据库错误日志文件
log_error=/data1/home/xiaohan/myprogram/mysql/error.log
server-id=100
#数据库默认字符集,主流字符集支持一些特殊表情符号（特殊表情符占用4个字节
character-set-server = utf8mb4
#数据库字符集对应一些排序等规则，注意要和character-set-server对应
collation-server = utf8mb4_general_ci
#设置client连接mysql时的字符集,防止乱码
init_connect='SET NAMES utf8mb4'
#是否对sql语句大小写敏感，1表示不敏感
lower_case_table_names = 1
max_allowed_packet = 100M
innodb_log_file_size=300M
```

port 部分可以根据服务器现有情况进行更改

三、安装MySql

在此可能由于默认端口被占用，从而无法安装，使用
```
netstat -nlpt
```
查看占用情况，使用未被使用的端口，在配置文件中修改。

```
## 进入文件夹
cd mysql
## 创建data文件夹
mkdir data
## 依赖配置文件安装mysql
bin/mysqld --defaults-file=/data1/home/xiaohan/myprogram/mysql/my.cnf\
 --initialize --user=xiaohan \
 --basedir=/data1/home/xiaohan/myprogram/mysql \
 --datadir=/data1/home/xiaohan/myprogram/mysql/data
```

四、启动Mysql

```
bin/mysqld_safe --defaults-file=/data1/home/xiaohan/myprogram/mysql/my.cnf\
 --user=xiaohan &
```


五、登录mysql

查看sock，pid 文件是否正常建立，查看root用户的随机生成密码

```
cat error.log | grep root@localhost
```

由于服务器已经安装了mysql因此会造成'/tmp/mysql.sock'占用，首次登录我们使用依赖于sock文件的方式登录。

```
bin/mysql -u root -p -S /data1/home/xiaohan/myprogram/mysql/mysql.sock
```

六、登录后修改初始密码

mysql为了保证安全性，只有在修改了初始密码之后才可以进行其他的操作。

mysql 8.0 版本之前修改密码
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('oper');
flush privileges;
```

mysql 8.0 版本之后修改密码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Root_12root';
```

之后可以进行其他操作，此次退出之后再登录需要使用新的密码。


## bug fix

关于退出登录后无法使用新修改后的密码登录问题

* user root用户中无localhost字段

查看用户信息

```
### 停止mysql
mysql stop
### 无密码启动
mysqld_safe –skip-grant-tables &
### 查看用户信息
use mysql
// 8.0 之前
select user,host,password from user where user='root';
// 8.0 之后
select user,host,authentication_string from user where user='root';
```
添加localhost 字段

```
update user set host='localhost' where user='root' and host='%';
```

* mysql登录项导向错误

我们常用的mysql登录语法为：

```
mysql -u root -p
```

但如果是非root用户的话，实际上应该调用自己安装的mysql,所以登录时定向导向即可：

```
/data1/home/xiaohan/myprogram/mysql/bin/mysql -u root -p
```

