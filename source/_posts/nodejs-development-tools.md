---
title: nodejs 开发工具
date: 2017-07-07 22:40:03
tags: [nodejs, vscode]
---
### nodemon
nodejs正常开发的过程中，每次修改修改代码都必须手动重启服务器，这是很麻烦的操作，于是就出来了很多自动重启的工具，其中 [nodemon](https://github.com/remy/nodemon/) 是我用过比较好用的，用法比较简单，首先安装nodemon
<!--more-->
``` bash
# 安装项目的开发依赖
yarn add nodemon -D
# 或者安装全局依赖
yarn global add nodemon
```
如果是安装项目开发依赖的话，可以配置package.json 文件的scripts项，用nodemon 来代替node 执行nodejs程序即可。
``` json
"scripts": {
  "start": "nodemon bin/www"
}
```
如果是全局安装的nodemon，还可以直接在终端运行`nodemon bin/www`

### pm2
[pm2](http://pm2.keymetrics.io/docs/usage/quick-start/) 是一个带有负载均衡功能的Node应用的进程管理器。
当你要把你的独立代码利用全部的服务器上的所有CPU，并保证进程永远都活着，0秒的重载， PM2是完美的。
上面这段话是摘至豆瓣的关于pm2的介绍，pm2用于部署nodejs项目，守护nodejs进程用的，保证nodejs能一直运行用的。
``` bash
# 安装
yarn global add pm2

# 运行
pm2 start bin/www

# 查看运行状态
pm2 list

# 追踪资源运行情况
pm2 monit

# 查看应用部署详情
pm2 describe appId

# 查看日志
pm2 logs

# 重启应用
pm2 restart appName

# 停止应用
pm2 stop appName
```

其中`pm2 start bin/www --watch`可以达到上面nodemon 模块重启服务的效果

### vscode
不得不说 vscode 神了，我现在的开发基本离不开这个东西了，vscode 自带的调试功能配合上 nodemon 极大的提升了nodejs项目的开发速度。
打开vscode的调试面板，如果没有配置，可以先点击配置，会出现选择环境的对话框，vscode 就会帮助我们生成调试配置文件![step2](/image/2017/07/nodejs-development/vscode-debug.png)，大概如下
```
{
  // Use IntelliSense to learn about possible Node.js debug attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "启动程序",
      "program": "${workspaceRoot}\\bin\\www"
    },
    {
      "type": "node",
      "request": "attach",
      "name": "附加到端口",
      "address": "localhost",
      "port": 5858
    }
  ]
}
```
这里有两个配置，一个是启动程序，另一个是附加到端口。选择一个配置项，然后开始调试即可开始调试。
如果使用启动程序的话，vscode 会用 debug 模式启动我们程序，也就上例的`bin/www`，然后我们可以在代码中设置断点等调试操作。
如果用的是附加到端口的配置项的话，需要我们已经用 debug 模式启动 nodejs 程序：
``` bash
nodemon --inspect=5858 bin/www
# or
node --inspect=5858 bin/www
```
也可以配置一个nodejs 的scripts
``` json
"scripts": {
  "debug": "nodemon --inspect=5858 bin/www"
}
```
然后运行开始调试，就可以愉快的调试nodejs项目了，其中--inspect=port 中的 端口号必须与附加到端口配置项的port相同。
然后我们就可以很愉快的开始nodejs 项目了。

最后附上一份简单的package.json 文件以做参考
``` json
{
  "name": "wash",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "nodemon bin/www",
    "debug": "nodemon --inspect=5858 bin/www",    
    "pm2": "pm2 start bin/www",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "axios": "^0.16.1",
    "debug": "^2.6.3",
    "handlebars": "^4.0.6",
    "jade": "~1.11.0",
    "koa": "^2.2.0",
    "koa-bodyparser": "^3.2.0",
    "koa-convert": "^1.2.0",
    "koa-json": "^2.0.2",
    "koa-logger": "^2.0.1",
    "koa-onerror": "^1.2.1",
    "koa-router": "^7.0.1",
    "koa-static": "^3.0.0",
    "koa-views": "^5.2.1",
    "md5": "^2.2.1"
  },
  "devDependencies": {
    "nodemon": "^1.11.0"
  }
}

```