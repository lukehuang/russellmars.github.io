title: hexo博客搭建
date: 2015-02-06 14:34:03
tags: [hexo, 博客]
categories: hexo
---
之前有介绍了hexo，下面我们就来说说怎么搭建一个hexo博客。
<!--more-->
## 准备
### 安装node.js
本人的系统是win7 64位的，所以我去[官网](http://nodejs.org/download/)上下载的是64位版本的node.js（Windows Installer）, 当然小伙伴们可以根据自己的情况下载。下载好以后，一直next安装就好了，会自动添加到环境变量，以及安装npm的，这些都不用担心的。
### 安装git
我下载的是[msysgit](http://msysgit.github.io/)，这个东西下起来蛮坑的，反正我叫别人帮我下的，然后传给我的。下载好以后也是安装就好了。
## 安装hexo
当git和node.js都安装好了以后，打开你的系统的命令提示符（cmd）
在命令提示符中输入
``` cmd
npm install -g hexo
```
npm就回自动帮你安装hexo，npm是安装node.js时一起安装的一个工具（node.js 包管理器），当然这些不必在意。
接下来就是正式搭建你的博客了。
进入任意盘(如d盘)，鼠标右键，你会发现有有3个git相关的选项，我们关注git bash就好了，点击git bash，一次输入如下命令
``` bash
hexo init blog
cd blog
npm install
hexo server
```
我解释一下这几条命令。首先执行hexo init blog 后，会在当前目录下新建一个blog的文件夹，里面有一些文件，这其实是初始化了 hexo 博客，但是别急，现在还什么都用不了，cd blog是进入刚才新建的blog文件夹(当然你也可以直接打开这个文件夹，然后在里面使用git bash)，然后是npm install，这个命令是安装hexo所需要的一些模块，安装了哪些模块呢，你可以找blog文件夹中找到package.json的文件，用记事本打开就能看到里面有些什么，你会发现一个单词dependencies，看到这你应该会明白了，dependencies里面的东西就是安装的依赖。好了，最后一条命令是hexo server 相当于把你的博客部署在本地了，你可以打开你的浏览器输入 http://localhost:4000/ 就可以查看效果了（ctrl + c可以停止），是不是很简单^_^
## 更换主题
hexo有许多的[主题](https://github.com/hexojs/hexo/wiki/Themes)，你可以点开每一个主题后面的demo查看效果，我使用的主题是[light](https://github.com/hexojs/hexo-theme-light),下载主题的命令如下（一定要在/blog/themes目录下打开git bash执行）
``` bash
git clone git://github.com/tommy351/hexo-theme-light.git light
```
当然也可以在blog目录下使用如下命令
``` bash
git clone https://github.com/hexojs/hexo-theme-light.git themes/light
```
用编辑器打开blog/_config.yml（当然在此之前，我想推荐你一款编辑器sublime text2，我现在已也一直用这个编辑器，性感，风骚的编辑器，支持多种语言，用了就知道，根本停不下来），找到theme，并将主题修改为light
```
theme: light
```
启动server（之前已经说过）
``` bash
hexo server
```
打开你的浏览器，就可以看到刚才更新的主题了
## 推送到github
### 注册[github](https://github.com/)账号
怎么注册的我就不说了，如果已有账号，可以跳过这一步。
### 创建repository
我的账号是hzhyiyy，创建的repository名字就一定是：账号名.github.io,如
```
hzhyiyy.github.io
```
### 部署
编辑blog/_config.yml,在里面找到deploy，并将其内容改为(修改你自己的repository)
```
deploy:
  type: github
  repository: https://github.com/hzhyiyy/hzhyiyy.github.io.git
  branch: master
```
然后在blog目录中在此打开git bash，依次输入如下命令
``` bash
hexo generate
hexo deploy
```
在deploy时会要求你输入github.com的用户名和密码，输入即可
成功之后在浏览器中输入
```
http://github用户名.github.io/
如
http://hzhyiyy.github.io/
```

下一篇文章将介绍hexo博客的书写方式