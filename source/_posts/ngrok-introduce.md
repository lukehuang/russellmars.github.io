title: ngrok，将本机ip映射成公网域名工具
date: 2015-09-28 11:17:41
tags: [web,ngrok]
categories: web
---
推荐一款神器：ngrok。ngrok 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。简单来说可以内网映射到公网上面，这样就可以在公网上访问你的本地网络服务。
<!--more-->
有这样一个场景，一个开发者需要开发微信公众号，但是微信的调试是需要连接公网的ip的，也就是说，我们需要部署到服务器，然后查看各种日志，才能做一些调试。而又这个东西以后，我们可以将自己电脑的ip映射成一个公网的域名，并启动一个tomcat，然后直接就能调试。

使用方法，下载ngrok后，打开cmd，定位到ngrok.exe的目录中
方法一：
```
ngrok 8080
```
然后cmd中会出现一些信息，给你一个随机的域名，这个域名已经监控了你的8080端口，也就是说我如果能正常访问`http://localhost:8080/weixin/msg`，那么就可以直接用`http://xxxx.ngrok.com/weixin/msg`，这里给你分配的域名是随机的，意味着，你每次打开调试都需要修改微信开发的配置。
下面看方法二：
```
ngrok -config ngrok.cfg -subdomain hzhyiyy 8080
```
跟方法一一样，但是，这个会绑定一个固定的域名，上面这段命令就是绑定了hzhyiyy.ngrok.com这个域名，我们可以通过`http://hzhyiyy.ngrok.com/weixin/msg`来访问到本地的服务（tomcat），ngrok.cfg是一个配置文件，在`http://www.tunnel.mobi/`下载，放到ngrok.exe的同一目录中即可。