title: Java和websocket实现网页聊天室
date: 2015-03-31 16:18:45
tags: [java,html5,websocket]
categories: java
---
WebSocket protocol 是HTML5一种新的协议，它实现了浏览器与服务器全双工通信(full-duplex)。接下来将实现一个websocket的网页聊天室的demo，前端框架会使用amazeUI，后台使用Java，网页编辑器使用UMEditor。
<!--more-->
## 涉及知识
网页前端（HTML+CSS+JS）和Java

## 软件环境

1. tomcat7
2. jdk7
3. eclipse javaee
4. 现代浏览器（推荐chrome或者firefox）

## 效果截图
效果一：
![chrome截图](/image/2015/03/java-websocket-chatroom/java-websocket-chatroom-chrome.jpg)
效果二：
![firefox截图](/image/2015/03/java-websocket-chatroom/java-websocket-chatroom-firefox.jpg)

## 项目实战
### 新建项目
由于最近一直在学习maven，所以我就用maven新建了个webapp项目，pom.xml的依赖如下
``` xml
<dependency> 
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.4</version>
</dependency>

<dependency>
    <groupId>commons-beanutils</groupId>
    <artifactId>commons-beanutils</artifactId>
    <version>1.9.2</version>
</dependency>

<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.0</version>
</dependency>

<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>

<dependency>
    <groupId>net.sf.json-lib</groupId>
    <artifactId>json-lib</artifactId>
    <version>2.4</version>
    <classifier>jdk15</classifier>
</dependency>
```
依赖添加完成后，把项目发布到tomcat上，一个空白的项目就建好了。

### 前台代码
在这介绍一下amazeUI，我摘了一段[官网](http://amazeui.org/)上的介绍：Amaze UI 以移动优先（Mobile first）为理念，从小屏逐步扩展到大屏，最终实现所有屏幕适配，适应移动互联潮流。
首先从AmazeUI官网下载压缩包，然后解压把assets文件夹拷贝到src/main/webapp目录下，这样我们就能使用AmazeUI了。
UMEditor，它是一个富文本在线编辑器，能让我们的消息内容多姿多彩。
从UEditer官网下载Mini版的JSP版本压缩包，解压后把整个目录拷贝到src/main/webapp目录下，接下来就可以编写前端代码了，代码如下
``` html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>chatroom</title>

<!-- Set render engine for 360 browser -->
<meta name="renderer" content="webkit">

<!-- No Baidu Siteapp-->
<meta http-equiv="Cache-Control" content="no-siteapp" />

<link rel="alternate icon" href="assets/i/favicon.png">
<link rel="stylesheet" href="assets/css/amazeui.min.css">
<link rel="stylesheet" href="assets/css/app.css">

<!-- umeditor css -->
<link href="umeditor/themes/default/css/umeditor.css" rel="stylesheet">

<style>
.title {
    text-align: center;
}

.chat-content-container {
    height: 29rem;
    overflow-y: scroll;
    border: 1px solid silver;
}
</style>
</head>
<body>
    <!-- title start -->
    <div class="title">
        <div class="am-g am-g-fixed">
            <div class="am-u-sm-12">
                <h1 class="am-text-primary">聊天室</h1>
            </div>
        </div>
    </div>
    <!-- title end -->

    <!-- chat content start -->
    <div class="chat-content">
        <div class="am-g am-g-fixed chat-content-container">
            <div class="am-u-sm-12">
                <ul id="message-list" class="am-comments-list am-comments-list-flip"></ul>
            </div>
        </div>
    </div>
    <!-- chat content start -->

    <!-- message input start -->
    <div class="message-input am-margin-top">
        <div class="am-g am-g-fixed">
            <div class="am-u-sm-12">
                <form class="am-form">
                    <div class="am-form-group">
                        <script type="text/plain" id="myEditor" style="width: 100%;height: 8rem;"></script>
                    </div>
                </form>
            </div>
        </div>
        <div class="am-g am-g-fixed am-margin-top">
            <div class="am-u-sm-6">
                <div id="message-input-nickname" class="am-input-group am-input-group-primary">
                    <span class="am-input-group-label"><i class="am-icon-user"></i></span>
                    <input id="nickname" type="text" class="am-form-field" placeholder="Please enter nickname"/>
                </div>
            </div>
            <div class="am-u-sm-6">
                <button id="send" type="button" class="am-btn am-btn-primary">
                    <i class="am-icon-send"></i> Send
                </button>
            </div>
        </div>
    </div>
    <!-- message input end -->

    <!--[if (gte IE 9)|!(IE)]><!-->
    <script src="assets/js/jquery.min.js"></script>
    <!--<![endif]-->
    <!--[if lte IE 8 ]>
    <script src="http://libs.baidu.com/jquery/1.11.1/jquery.min.js"></script>
    <![endif]-->
    
    <!-- umeditor js -->
    <script charset="utf-8" src="umeditor/umeditor.config.js"></script>
    <script charset="utf-8" src="umeditor/umeditor.min.js"></script>
    <script src="umeditor/lang/zh-cn/zh-cn.js"></script>
    
    <script>
        $(function() {
            // 初始化消息输入框
            var um = UM.getEditor('myEditor');
            // 使昵称框获取焦点
            $('#nickname')[0].focus();
        });
    </script>
</body>
</html>
```

### 后台代码
前台的页面我们已经有了，现在还需要实现服务器段的代码。
值得注意的是，从JavaEE 7开始就统一了WebSocket的API，所以如果是之前的java版本可能需要第三方的jar包才能实现websocket，当然也可以自己实现一个（通往大神的必经之路）。
``` java
package com.hzhyiyy.chat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;
import net.sf.json.JSONObject;
/**
 * @author hzhy Mar 31, 2015
 * todo : 聊天服务器
 */
@ServerEndpoint("/chatroom")
public class ChatServer { 
    private static final SimpleDateFormat DATE_FORMAT = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    /**
     * todo : 初始化一个会话
     * @param session
     */
    @OnOpen
    public void onOpen(Session session){
    }
    @OnMessage
    public void onMessage(String message, Session session){
        //将消息格式化成json
        JSONObject jsonObj = JSONObject.fromObject(message);
        //在消息中添加发送时间
        jsonObj.put("date", DATE_FORMAT.format(new Date()));
        //遍历所有的消息
            //在消息中添加是否是当前会话的标志
            jsonObj.put("isSelf", openSession.equals(session));
            //发送json格式的消息
            openSession.getAsyncRemote().sendText(jsonObj.toString());
        }
    }
    /**
     * todo : 关闭一个会话时的操作
     */
    @OnClose
    public void onClose(){
    }
    /**
     * todo : 处理错误
     */
    @OnError
    public void onError(Throwable t){
    }
}
```
### 前后台交互
前台页面和后台处理都做完了，现在需要如何做到前后台交互，这就用到了websocket了。仙说一说这一个过程：新建一个WebSocket对象，然后就可以和服务器端进行交互，从浏览器发送消息给服务器端，同时要验证输入框的内容是否为空，然后接受服务端发送的消息，把它们动态地添加到聊天内容框中。
在
``` javascript
// 初始化消息输入框
var um = UM.getEditor('myEditor');
// 使昵称框获取焦点
$('#nickname')[0].focus();
```
之后添加如下代码
``` javascript
// 新建WebSocket对象，最后的/chatroom跟服务器端的@ServerEndpoint("/chatroom")对应
var socket = new WebSocket('ws://${pageContext.request.getServerName()}:${pageContext.request.getServerPort()}${pageContext.request.contextPath}/chatroom');
// 处理服务器端发送的数据
socket.onmessage = function(event) {
    addMessage(event.data);
};
// 点击Send按钮时的操作
$('#send').on('click', function() {
    var nickname = $('#nickname').val();
    if (!um.hasContents()) {    // 判断消息输入框是否为空
        // 消息输入框获取焦点
        um.focus();
        // 添加抖动效果
        $('.edui-container').addClass('am-animation-shake');
        setTimeout("$('.edui-container').removeClass('am-animation-shake')", 1000);
    } else if (nickname == '') {    // 判断昵称框是否为空
        //昵称框获取焦点
        $('#nickname')[0].focus();
        // 添加抖动效果
        $('#message-input-nickname').addClass('am-animation-shake');
        setTimeout("$('#message-input-nickname').removeClass('am-animation-shake')", 1000);
    } else {
        // 发送消息
        socket.send(JSON.stringify({
            content : um.getContent(),
            nickname : nickname
        }));
        // 清空消息输入框
        um.setContent('');
        // 消息输入框获取焦点
        um.focus();
    }
});

// 把消息添加到聊天内容中
function addMessage(message) {
    message = JSON.parse(message);
    var messageItem = '<li class="am-comment '
            + (message.isSelf ? 'am-comment-flip' : 'am-comment')
            + '">'
            + '<a href="javascript:void(0)" ><img src="assets/images/'
            + (message.isSelf ? 'self.png' : 'others.jpg')
            + '" alt="" class="am-comment-avatar" width="48" height="48"/></a>'
            + '<div class="am-comment-main"><header class="am-comment-hd"><div class="am-comment-meta">'
            + '<a href="javascript:void(0)" class="am-comment-author">'
            + message.nickname + '</a> <time>' + message.date
            + '</time></div></header>'
            + '<div class="am-comment-bd">' + message.content
            + '</div></div></li>';
    $(messageItem).appendTo('#message-list');
    // 把滚动条滚动到底部
    $(".chat-content-container").scrollTop($(".chat-content-container")[0].scrollHeight);
}
```
## 测试
到这里简单的网页聊天室就做完了，接下来就可以测试了，你可以开几个浏览器自己测试，也可以邀请局域网的小伙伴一起试一试。

## 源码下载
如有不理解的小伙伴可以自行下载[源码](https://github.com/hzhyiyy/websocket-chat)（需要自己新建项目）

## 参考文献
+   [Java和WebSocket开发网页聊天室](http://www.shiyanlou.com/courses/running/350)