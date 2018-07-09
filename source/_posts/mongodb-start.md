title: mongodb初接触
date: 2015-07-30 21:00:24
tags: mongodb
categories: mongodb
---
最近，因为项目需求，需要使用到mongodb，不是项目还没有开始，要我自学mongodb，所以自己搞了几天后，发现还是有些收获，也是打算记录下来，以备不时之需。
<!--more-->
## 前言
首先mongodb是什么东西，优缺点什么的，我就不说了，关系型数据库大家都用过吧。这里要说的mongodb呢，有3个元素：数据库，集合，文档。数据库对应表，文档对应行。大概是这么对应的，但是具体还是有很大的差别的，大家用了就知道。

## 下载和安装
下载的话一般是去[mongodb官网](https://www.mongodb.org/downloads)上去下载，根据自己的系统，下载一个最新的版本就行了，一般来说偶数为“稳定版”（如：1.6.X，1.8.X），奇数为“开发版”（如：1.7.X，1.9.X)，并且如果下载的事32位的版本的话，只能存放2g的数据，所以64位的系统比较适合mongodb吧。
安装的话基本就是next就好了，可以修改mongodb的安装目录的，我的安装目录是
`D:\software\MongoDB\Server\3.4`
## 启动
### 数据库文件夹
启动mongodb是需要指定一个数据库文件存放的目录的，所以我们需要新建一个数据库存放的目录(如：D:\mongodb\data)。
### 启动mongodb
打开我们的命令行，来到安装目录下的bin文件夹中，我们输入命令
`mongod --dbpath=D:\mongodb\data`.
### 检查是否成功
一般来说我的的mongodb就已经启成功了，如果一定要检查以下是否启动成功的话，我们可以打开浏览器，输入网址`http://localhost:27017/`，如果有信息，就算启动成功了。
### 安装为windows服务
首先新建一个config文件 `D:\software\MongoDB\Server\3.4\mongod.config`，内容如下
```
dbpath=D:\mongodb\data\db
logpath=D:\mongodb\data\log\mongodb.log
```
然后有两种方式安装

+ 用mongod安装服务
  首先以管理员身份运行cmd，cd到mongo的安装目录下的bin目录下，然后运行
  ```
  mongod --config D:\software\MongoDB\Server\3.4\mongod.config --install
  ```
+ 使用sc安装服务
  首先以管理员身份运行cmd
  ```
  sc create MongoDB binPath= "D:\software\MongoDB\Server\3.4\bin\mongod.exe --service --config=D:\software\MongoDB\Server\3.4\mongod.config"
  ```

## 简单入门
一般来说，对一个数据库的操作就是_增删改查_，首先我们也是用命令行进行一些简单的操作，首先在打开一个cmd，然后来到mongodb的bin目录，输入命令`mongo`，这样就连上了mongodb自带的test数据库了，然后会进入mongo的shell命令模式。
### insert
输入如下命令，可插入一个文档
`db.user.insert({name:"mars",age:30})`
### update
输入如下命令，可修改文档，第一个参数是查询条件，第二个参数是修改后的文档
`db.user.update({name:"mars"},{name:"mars",age:35})`
### remove
输入如下命令，可删除文档
`db.user.remove({name:"mars"})`
### find
输入如下命令，可查找文档
`db.user.find({name:"mars"})`

## 总结
上面的步骤都做完了的话，对mongodb也有一个大概的了解了，想深入学习的话，还是要下些功夫的。