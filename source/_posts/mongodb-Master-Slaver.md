title: mongodb的Master-Slaver集群的搭建
date: 2015-08-07 21:17:37
tags: mongodb
categories: mongodb
---
部署应用（模拟部署）（所有的命令都在mongodb文件的安装目录中的bin文件夹中执行，分别是c、d、e盘的文件夹）
## 安装mongodb  
在自己的电脑上，将mongodb的安装后（一开始是安装在d盘的）的文件复制两份，一份放在c盘，一份放在e盘
<!--more-->
## 开启一个master
```
mongodb --dbpath=d:\software\mongodb\db --master
```
--master指定为主数据库端口号默认为27017
## 开启两个slave
分别在c盘和e盘开启两个slave，--slave指定为备份数据库，--source为master的ip地址和端口号
```
mongod --dbpath=c:\mongodb\db --port=8888 --slave --source=127.0.0.1:27017
mongod --dbpath=e:\mongodb\db --port=5555 --slave --source=127.0.0.1:27017
```
## 连接客户端
分别在每个盘的mongodb的bin目录下执行，打开shell命令
```
mongo 127.0.0.1:27017
mongo 127.0.0.1:8888
mongo 127.0.0.1:5555
```
## 读写验证
在master的shell命令中插入两条数据
```
db.person.insert({name:”jack”,age:25})
db.person.insert({name:”mars”,age:30})
```
其他的两个shell的中执行查看：
```
db.person.find()
```
发现数据已经同步。
## 读写分离
通过驱动链接到mongodb时，默认master能读写，slave不能读也不能写，但是连接时通过显示的指定mongoClient.slaveOk()就可以读取这个slave的数据了。
## 总结
这种方式部署的mongodb数据库，并不够灵活，当master宕机以后，slave不能自动成为master，只能当成一个读取的数据库。并且官方已经不推荐这种集群部署了。
## 参考资料
+  [主从复制](http://www.cnblogs.com/huangxincheng/archive/2012/03/04/2379755.html)
