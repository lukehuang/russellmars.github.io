title: angularjs 应用
date: 2016-02-16 13:58:56
tags: [javascript, angularjs]
categories: javascript
---
最近一直在接触angularjs框架，也用这个东西做了一个项目出来，当然纯粹是赶鸭子上架给逼出来的，之前其实也只是接触过knockoutjs这类mvvm框架，以为同样是mvvm框架，上手起来会很快，结果各种踩坑，说多了都是泪。下面我带大家搞一个完整的angularjs项目，以供大家学习参考。
<!--more-->
## 准备
### 前提知识
+   JavaScript
+   html
+   angularjs

### 工具
+   nodejs
+   git
+   yoeman
+   grunt
+   bower
+   编辑器（建议使用哪个sublime或者webstorm）

上面说的这些东西都要了解下。

## step 1
首先，需要安装nodejs环境，以及git，具体安装步骤这里不再详细叙述，网上有教程。安装以后需要安装一些npm模块，打开终端，执行以下命令：
```
npm install -g grunt-cli bower yo generator-karma generator-angular
```
这样大部分的工具以及生成器就安装好了。
## step2
建立一个目录用于存放项目，并cd进入这个目录
```
mkdir pub && cd pub
```
运行yo angular，生成我们项目原型框架，app-name是可选的，默认使用我们的根目录的名字，这个app-name会应用到整个项目中。
```
yo angular [app-name]
```
yoeman会询问我们几个问题，按照我们的实际需要填写就好了，就会帮我们生成项目的必须文件，并下载npm和bower依赖。等待一段时间就差不多了。
这样我们的项目框架就差不多搭建好了，可以在这个项目的代码中做我们的业务逻辑了。
现在我们运行`grunt serve`就能看到效果。
## step3
我们做项目时也可能会用到route,service，directive，controller，filter，view等，我们可以手动生成这些文件，然后将这些文件添加到index.html中，但是在生成器中这些东西都是可以帮我们自动生成的，并添加相应代码，我们只需要写逻辑就好了，如：
```
yo angular:route myroute
yo angular:controller user
yo angular:directive myDirective
yo angular:filter myFilter
yo angular:view user
yo angular:service myService
```
## step4
到现在我们的目录结构差不多是这个样子![project-structure](/image/2016/02/angularjs-application/project-structure.jpg)
1. .tmp是临时文件夹，运行grunt serve时，会自动产生。
2. app是我们主要关注的文件夹，里面都是我们自己需要些的文件。并且我们之后需要新增的文件几乎都这这个文件夹下。
3. bower_components是项目bower依赖模块存放的目录。
4. node_modules是项目npm依赖模块，主要是项目开发阶段中所需要的一些模块，打包是基本不会在了。
5. test是测试文件的存放目录
6. .bowerrc文件是bower的配置文件，这里配置了bower依赖的存放目录，如果需要手动生成这个文件需要用命令行工具
7. .editorconfig是一个编辑器的配置文件，很多编辑器都支持这种配置，可以统一项目开发的代码格式化风格。
8. bower.json是bower依赖的配置文件
9. Gruntfile.json是grunt的配置文件，grunt的所有任务都在这里配置。
10. package.json是项目的配置文件，nodejs相关。

app目录下的文件如下![app-structure](/image/2016/02/angularjs-application/app-structure.jpg)
看名字基本都知道各个文件夹以及文件是干嘛的，index.html就是单页应用的那个单页，
基本上项目的目录结构都讲了。后面会将angularjs项目是怎么开发的。
## step5
单页应用中，页面的跳转是靠路由来控制，angularjs自带了路由功能，需添加路由模块ngRoute，在app.js中配置路由，项目依赖的模块建议最好在app.js中配置，其他的controller，service等中就可以忽略这个模块依赖参数了。