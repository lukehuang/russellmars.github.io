---
title: nodejs-install
date: 2018-06-30 22:12:07
tags: [nodejs]
---
nodejs 的安装方法
+ windows 安装
+ linux 里的两种安装方式

<!--more-->

### windows
去官网里[下载](https://nodejs.org/en/download/)安装文件，然后安装就可以了，安装步骤都是next就可以了
### linux
#### 压缩包下载安装
首先查看自己的系统是32位还是64位的
``` sh
uname -m
# 32位：Xi686
# 64位：x86_64
```
找到对应的安装包，右键复制下载链接，然后
``` sh
# 进入到用户目录
cd ~
# 获取安装包
wget https://nodejs.org/dist/v8.11.3/node-v8.11.3-linux-x64.tar.xz
# 解压
tar xf node-v8.11.3-linux-x64.tar.xz
# 建立软链接，就算已经安装好了
ln -s ~/node-v8.11.3-linux-x64/bin/node /usr/local/bin/node
ln -s ~/node-v8.11.3-linux-x64/bin/npm /usr/local/bin/npm
ln -s ~/node-v8.11.3-linux-x64/bin/npx /usr/local/bin/npx
# 查看nodejs的版本
node -v
```
#### package-manager 安装
官网的下载页面提供了[Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/)的地址，例如我这里是centos7.0_64的系统，在页面里找到相应的安装方式既可，只需两条命令
```
sudo yum -y install nodejs
curl --silent --location https://rpm.nodesource.com/setup_10.x | sudo bash -
```
还有install build tools 的可选安装
```
sudo yum install gcc-c++ make
```
我自己比较喜欢package manager的方式安装，比较简单