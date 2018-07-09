---
title: nodejs 调试
date: 2018-07-09 16:06:31
tags:
---

nodejs的调试方式有很多，可以看[官方文档](https://nodejs.org/en/docs/guides/debugging-getting-started/)，我英文不好，只研究了几种个人比较常用的。
+ console.log
+ chrome devtools
+ vs code launch
+ vs code attach

<!--more-->

## console.log
console.log 对于任何一个web者来说都不陌生，不详细说明

### 优点
+ 简单

### 缺点
+ 简单

## chrome devtools
使用chrome浏览器的devtools调试，具体方式是使用`node`启动应用后，用chrome浏览器打开应用的随便一个访问链接，如
``` text
http://127.0.0.1:2333/
```
f12打开chrome devtools，devtools的左上角会出现一个<span style="color: green">绿色</span>的六边形按钮，点击它，就会出现一个新的简化版的devtools，里面目前有5个tab项，选择`Sources`，本地的代码源文件会在左侧展现，找到需要调试的文件，打个端点，就可以愉快的debugging了，调试方法和普通的浏览器web应用类似。

## vs code launch
我最喜欢的编辑器是vs code，本身提供了调试功能，主要又两种调试模式，首先是`launch`，
使用方法是，在调试tab中`添加配置`，选择`nodejs`，默认就是attach方式的，配置大概如下
``` json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "启动程序",
      "program": "${workspaceFolder}\\index.js"
    }
  ]
}
```
`program`配置项就是`nodejs`项目的入口文件，本例中我的入口文件是`index.js`
通过这种方式直接启动调试，不需要在另外用`node`运行`index.js`了，
直接在编辑器的行号旁边打断点即可，编辑器上方会有调试工具条，快捷方式和`chrome devtools`类似

## vs code attach
attach就是附加到另个端口监听，配置文件如下(attach部分)：
``` json
{
  // 使用 IntelliSense 了解相关属性。
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "attach",
      "name": "Attach",
      "port": 9229
    },
    {
      "type": "node",
      "request": "launch",
      "name": "启动程序",
      "program": "${workspaceFolder}\\index.js"
    }
  ]
}
```
attach方式，需要事先启动`nodejs`应用，使用参数`node --inspect[=host:port] index.js `，默认`host`是`127.0.0.1`，默认`port`是`9229`(对应的是attach方式附加到的端口)，如我的启动参数
``` bash
node --inspect index.js
```
一般开发的时候都是使用`npm scripts`，的方式启动的，在`package.json`的`scripts`中配置：
``` json
"scripts": {
  "debug": "node --inspect index.js"
}
```
我开发的时候会使用`nodemon`和`cross-env`，所以启动方法变成下面的了：
``` json
"scripts": {
  "debug": "cross-env NODE_ENV=local nodemon --inspect index.js --ships=4 --config nodemon.json",
  "local": "cross-env NODE_ENV=local nodemon index.js --ships=4 --config nodemon.json"
}
```