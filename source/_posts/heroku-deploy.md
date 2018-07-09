title: 将应用部署到heroku中
date: 2016-03-16 16:25:08
tags: study
categories: study
---
heroku是国外比较知名的云PaaS(平台即服务),我们可以部署我们的应用到heroku中,当然免费的heroku有许多的限制,所以如果发布项目时要慎重.
<!--more-->
## 准备
### 注册hero账号
去[heroku](https://www.heroku.com/)的官网注册,然后邮件激活,现在你应该能正常使用heroku的dashboard了.
### 创建应用
控制台中会告诉你heroku支持的编程语言,你可以看你需要是否有合适的,然后右上角有一个"+"号,可以创建应用,创建好了以后,你就可以将你本地代码部署到heroku中了.
## 部署
### 创建Procfile文件
在根目录下创建一个Procfile文件,这个文件很关键,文件的具体内容可以百度找,我这里列出我自己创建的应用的Procfile文件内容.
nodejs项目的:
```
web: node ./bin/www
```
java spring-boot项目的
```
web: java -Dserver.port=$PORT -jar target/blog-0.0.1-SNAPSHOT.jar
```
其实这个文件的目的主要就是指定用什么命令来运行什么文件
bin/www文件是我的nodejs项目的启动文件.
target/blog-0.0.1-SNAPSHOT.jar是我的spring-boot项目打包后的所在目录.
java项目如果要指定heroku中jdk的版本的话,可以在Procfile同级的目录下创建system.properties文件,内容可以如下:
```
java.runtime.version=1.7
```
### 安装toolbalt工具
toolbalt工具可以去[这里](https://toolbelt.heroku.com/)下载,安装的时候如果你的本机已经安装git了,可以选择安装,如果没安装git,会自动给你安装git.然后运行如下命令
```
$ heroku login
```
输入你的heroku的账户和密码
```
heroku keys:add ~/.ssh/*.pub
```
添加上传ssh到heroku
```
git remote add heroku git@heroku.com:yourAppName.git
```
添加远程仓库,yourAppName就是你之前创建的应用的名字,如果你之前没有创建的话,可以使用如下命令
```
heroku create
```
这样heroku会自动给你创建一个应用,但是应用的名字是随机给你生成的.
### push代码到hero库中
```
git push heroku master
```
如果之前的步骤都没有出错的话,执行上面的命令,你的代码就会部署到heroku中,并且会帮你开启你的应用.
### 访问你的应用
http://yourAppName.herokuapp.com就是你的应用的域名了,你可以用这个域名访问的应用.