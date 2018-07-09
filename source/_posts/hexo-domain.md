title: 购买域名，设置DNS
date: 2015-02-07 19:58:40
tags: hexo
categories: hexo
---
搭好博客了，一直想着有个自己的域名就好了，虽然github会给你一个二级域名，但是如果有一个自己专门的域名，应该会特别帅。故折腾了一个下午，并给自己的博客弄好了自己的域名。下面说说怎么购买域名的，以及设置DNS。
<!--more-->

##购买域名
在网上了解了下，大家也都推荐买域名去[狗爹](https://www.godaddy.com/),看了下，买一个.com的域名差不多需要$10/year，折合人民币65块钱的样子，我考虑的是买主机送域名的方案，这个也就$12/year，还送一个域名，也就多2刀，况且还可以玩玩主机什么的。好了下面说说怎么买域名的(直接购买域名的方式)
### 注册账号
先在godaddy里注册账号，如图

![注册账号](/image/2015/02/hexo-domain/hexo-domain-createAccount.jpg)

### 选域名并购买
注册号账号以后，在首页的文本框中输入你想买的域名，如：hellohzhy，然后点击后面的按钮search就好了，一会页面会出现search结果，会有各种后缀的域名供你选择，我用的是.com的，点击相应域名后的select，然后continue to cart，这样你选择的域名就会出现这你的购物车当中了，如图

![选择域名](/image/2015/02/hexo-domain/hexo-domain-selectDomain.jpg)

按着指导一直点击差不多是下一步的按钮就好了。值得注意的是，我们一般是一年一年的买，而默认是两年的，修改下就好了

![修改cart](/image/2015/02/hexo-domain/hexo-domain-checkoutCart.jpg)

然后就填写个人信息

![填写信息](/image/2015/02/hexo-domain/hexo-domain-billingInfo.jpg)

最后选择选择支付方式就好了，我用的是支付宝的，话说国外网站也支持支付宝，蛮厉害的，赞一个 ^_^

##设置DNS
买后域名后，就要设置dns了，在狗爹的网站中进入你的account就能看到如下信息，点击launch

![accountInfo](/image/2015/02/hexo-domain/hexo-domain-setDns.jpg)

在域名列表中找到你想设置的域名，然后点击后面的下拉列表，选择set Nameservers，进入设置

![launchdomain](/image/2015/02/hexo-domain/hexo-domain-launchDomain.jpg)

add nameserver中，添加两个DNSPod的DNS短地址，然后save就好了。
```
f1g1ns1.dnspod.net
f1g1ns2.dnspod.net
```
![addNameserver](/image/2015/02/hexo-domain/hexo-domain-addNameserver.jpg)

dns的设置就完成了。
##添加域名与ip地址的映射
设置好dns后，你需要再设置域名与ip地址的映射。
首先，去[dnspod.cn](https://www.dnspod.cn/)注册账号,然后在我的域名中添加你申请的域名，如图

![adddomain](/image/2015/02/hexo-domain/hexo-domain-adddomain.jpg)

然后添加记录，即你想用这个域名映射的ip地址，选择记录类型时，A代表使用ip地址，CNAME代表已有的另一个域名，我选择的是cname，在记录值中填写对应的ip或者域名，然后保存就好了。

![adddomain](/image/2015/02/hexo-domain/hexo-domain-addrecord.jpg)

接下来就是最后一步了，进入你的博客的文件夹的目录，找到 source 文件夹，创建名为 CNAME 的文件，然后添加你的域名就好了
```
hzhyiyy.com
```
如果想使用www.hzhyiyy.com形式，CNAME文件中相应地变为www.hzhyiyy.com即可。
这样就好了，部署一下你网站，然后在浏览器里输入hzhyiyy.com试试看
有可能要等待一段时间才能生效，所以不要着急 ^_^