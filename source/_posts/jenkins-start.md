---
title: jenkins 之旅
date: 2018-04-03 20:32:14
tags: [jenkins, 持续集成]
---
持续集成之jenkins的安装使用步骤
<!--more-->

### 环境
自己的运行环境
+ 64位 windows 10
+ 64位 JDK 1.8.0_131
+ 64位 apache-tomcat-8.5.29
+ Jenkins 2.107.1 for war

### 准备
#### 下载相关软件
1. 下载[JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)并安装（自行百度搜索安装以及环境变量配置）
1. 下载[tomcat](https://tomcat.apache.org/download-80.cgi#8.5.29)并安装（解压即可）
1. 下载[jenkins](https://jenkins.io/download/) Generic Java package(.war)的版本
![下载tomcat](/image/2018/04/jenkins-start/p1.png)
![下载jenkins](/image/2018/04/jenkins-start/p2.png)

#### 启动tomcat
将下载下来的jenkins的war包复制到`tomcat-root/webapps`下
![复制jenkins的war包](/image/2018/04/jenkins-start/p3.png)
然后执行`tomcat-root/bin/startup.bat`文件，正常情况下我们就已经启动了tomcat了，并且jenkins也已经在tomcat运行了，接下来就是jenkins的安装过程。
![启动tomcat](/image/2018/04/jenkins-start/p4.png)

### 安装
正常启动tomcat以后，我们打开浏览器输入[localhost:8080](http://localhost:8080)，可以看到tomcat的管理页面，如果出现了tomcat管理页面，则说明启动成功，如下图
![启动tomcat](/image/2018/04/jenkins-start/01.png)
接下来我们输入[localhost:8080/jenkins](http://localhost:8080/jenkins)，来访问我们的jenkins应用，如下图
![打开jenkins页面](/image/2018/04/jenkins-start/02.png)
此时我们的jenkins还没有初始化，这里选择`按照推荐的插件`，然后jenkins就会帮我们下载并安装默认的插件
![下载安装推荐插件](/image/2018/04/jenkins-start/03.png)
下一步会提示我们创建一个管理员账户，我们填写自己的信息就好了
![填写管理员账户](/image/2018/04/jenkins-start/04.png)
现在我们就已经完成了jenkins的安装了
![安装完成](/image/2018/04/jenkins-start/04.png)
