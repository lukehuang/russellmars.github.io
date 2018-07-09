title: 使用maven时出现的一些问题
date: 2015-03-31 17:46:09
tags: [java,maven]
categories: maven
---
最近学习maven的时候碰到了一些问题，到时都解决了，不过想记下来，给哪些碰到类似问题的同学提供一个思路。
<!--more-->

## 无法使用el表达式
用maven新建webapp项目后，当我在前台使用EL表达式的时候发现无法tomcat居然无法解析，如我从后台传来一个message，想直接在前台显示，它直接就给我显示${message}这一串文字。
```
${message}
```
后来发现可以在每个使用jsp的页面加上如下代码就可以了。
```
<%@ page isELIgnored ="false" %>  
```
但总体感觉这样不太优雅，于是看到了另一种方法。如下
maven新建的webapp项目的web.xml中其实是这样的
```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```
据说是2.3的dtd不支持el表达式吧，web.xml中的这部分改成如下(在webapps\manager\WEB-INF\web.xml中找到的)
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                      http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
  version="3.0"
  metadata-complete="true">

    <display-name>Archetype Created Web Application</display-name>
</web-app>
```
## 添加json-lib依赖的问题
去网上查询json-lib依赖时，一般给的都是
```
<dependency>
    <groupId>net.sf.json-lib</groupId>
    <artifactId>json-lib</artifactId>
    <version>2.4</version>
</dependency>
```
但是发现eclipse就下不下来这个依赖，后来知道，这个依赖其实和jdk版本有关，有json-lib-XX-jdk13.jar和json-lib-XX-jdk15.jar这两种版本，所以如果需要添加json-lib依赖，就需要指定classifier，如下：
```
<dependency>
    <groupId>net.sf.json-lib</groupId>
    <artifactId>json-lib</artifactId>
    <version>2.4</version>
    <classifier>jdk15</classifier>
</dependency>
```
