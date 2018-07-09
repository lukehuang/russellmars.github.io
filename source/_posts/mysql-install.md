---
title: mysql 免安装版
date: 2018-06-26 21:45:40
tags: [mysql]
---
mysql 免安装版本的安装使用教程
<!--more-->

### 下载
去到mysql的[下载页面](https://dev.mysql.com/downloads/)，找到并打开 `MySQL Community Server (GPL)
`的下载连接，在other downloads 里找到`
Windows (x86, 64-bit), ZIP Archive`，并下载。

### 解压
将下载以后的zip文件解压到某个目录，如`D:\software\mysql`，把`bin`目录添加到环境变量，如`D:\software\mysql\bin`

### my.ini 文件
在`D:\software\mysql`下新建`my.ini`文件，内容如下
``` ini
[mysqld]
# basedir代表自己MySQL的安装根目录
basedir = D:\\software\\mysql

character-set-server=utf8mb4

# datadir代表自己MySQL的数据库保存的目录，如果没有在MySQL安装的根目录下新建一个data文件夹 
datadir = D:\\database\\mysql\\data

# 允许最大连接数
max_connections=200

# port代表端口号
port = 3306

sql_mode=ONLY_FULL_GROUP_BY,NO_AUTO_VALUE_ON_ZERO,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,PIPES_AS_CONCAT,ANSI_QUOTES

[mysql]
default-character-set=utf8mb4
```
配置项可根据个人的不同修改，并新建datadir目录

### 注册成windows服务
在此之前要确保当前的mysql服务已经停止
``` bash
### 如果之前有设置密码则需要-p，输入密码
mysqladmin -u root [-p] shutdown
```
然后注册windows服务：
``` bash
mysqld --install

# 或者详细的注册，包括服务名，和配置文件陆军
mysqld --install MySQL --defaults-file=C:\mysql\my.ini
```
删除windows服务如下
``` bash
mysqld --remove
```
启动服务用
``` bash
net start MySQL
```
测试是否启动成功
```
mysql -u root [-p]
```

### 其他
1. [官方安装文档](https://dev.mysql.com/doc/refman/8.0/en/windows-install-archive.html)
2. 如果安装服务的时候报`msvcr120.dll`文件丢失，则需要下载安装[Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/zh-CN/download/details.aspx?id=40784)