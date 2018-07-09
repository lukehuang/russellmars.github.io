title: ehcache集群配置
date: 2015-09-01 11:54:26
tags: [java,ehcache]
categories: java
---
EhCache Server 方式的缓存
EhCache Server 是一个独立的缓存服务器，其内部使用 EhCache做为缓存系统，可利用前面提到的两种方式进行内部集群。对外提供编程语言无关的基于 HTTP 的 RESTful 或者是 SOAP 的数据缓存操作接口。
<!--more-->
下面是 EhCache Server 提供的对缓存数据进行操作的方法：
```
OPTIONS /{cache}}
```
获取某个缓存的可用操作的信息。
```
HEAD /{cache}/{element}
```
获取缓存中某个元素的 HTTP 头信息，例如：
```
curl --head  http://localhost:8080/ehcache/rest/sampleCache2/2
```
EhCache Server 返回的信息如下：
```
HTTP/1.1 200 OK 
X-Powered-By: Servlet/2.5 
Server: GlassFish/v3 
Last-Modified: Sun, 27 Jul 2008 08:08:49 GMT 
ETag: "1217146129490"
Content-Type: text/plain; charset=iso-8859-1 
Content-Length: 157 
Date: Sun, 27 Jul 2008 08:17:09 GMT
```
```
GET /{cache}/{element}
```
读取缓存中某个数据的值。
```
PUT /{cache}/{element}
```
写缓存。

由于这些操作都是基于 HTTP 协议的，因此你可以在任何一种编程语言中使用它，例如 Perl、PHP 和 Ruby 等等。

EhCache Server 同时也提供强大的安全机制、监控功能。在数据存储方面，最大的 Ehcache 单实例在内存中可以缓存 20GB。最大的磁盘可以缓存 100GB。通过将节点整合在一起，这样缓存数据就可以跨越节点，以此获得更大的容量。将缓存 20GB 的 50 个节点整合在一起就是 1TB 了。

## 参考文献
+ [EhCache缓存系统在集成环境中的使用详解](http://www.codeceo.com/article/ehcache-integrated-usage.html)
+ [EhCache官网说明](http://ehcache.org/documentation/2.8/modules/cache-server)