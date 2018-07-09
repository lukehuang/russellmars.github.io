title: 使用swiftype实现hexo的站内搜索
date: 2015-02-26 16:38:10
tags: [hexo, search, 优化]
categories: hexo
---
hexo博客属于静态博客，因为自己没有数据库，添加站内搜索功能就只能使用第三方的插件了，百度，谷歌等其实都是可以实现的，只是效果不是很理想，我从网上也看过很多的资料，在这里我推荐使用swiftype做站内搜索（本人使用的是pacman的主题，其他的主题稍有不同，可酌情参考）（网页爬虫）。
<!--more-->
废话不多说，直接开始吧
一. 首先去[swiftype](https://swiftype.com/)注册一个账号，然后会提示你输入你的网站的名称，以及输入一个搜索引擎名称（随便填）。完成之后，swiftype会爬你的网站，获得你网站的所有页面。当然这不是我们关心的。之后你会到达一个页面，如图![安装页面](/image/2015/02/hexo-search/hexo-search-install.jpg)
二. 然后在安装下图的配置填写表单，并点击下方的save，注意替换成你自己的域名![配置](/image/2015/02/hexo-search/hexo-search-config.jpg)
三. 点击左侧的`swiftype install code`，此时会生成一段代码，这段代码后面会有用，先copy下来。
四. pacman主题下的\_config.yml文件下末尾添加如下代码，并将google\_cse中的enable的值改成false
```
swift_search:
enable: true
```
五. 回到hexo的source目录中，新建一个search的文件夹，然后在里面新建一个index.md的文件，文件中输入如下内容
```
layout: search
title: search
---
```
六. 再回到pacman\layout\\\_partial目录中，打开header.ejs文件，在
```
<% if   (theme.google_cse&&theme.google_cse.enable){ %>
<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
    <label>Search</label>
    <input type="text" id="search" autocomplete="off" name="q" maxlength="20" placeholder="<%= __('search') %>" />
</form>
```
和
```
<% } else { %>
<form class="search" action="//google.com/search" method="get" accept-charset="utf-8">
    <label>Search</label>
    <input type="text" id="search" name="q" autocomplete="off" maxlength="20" placeholder="<%= __('search') %>" />
    <input type="hidden" name="q" value="site:<%- config.url.replace(/^https?:\/\//, '') %>">
</form>
```
之间加入如下代码
```
<% }else if (theme.swift_search&&theme.swift_search.enable){ %>
<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
    <label>Search</label>
    <input type="text" id="st-search-input" autocomplete="off" maxlength="20" placeholder="<%= __('search') %>" />
</form>
```

七. 打开当前文件夹中的search.ejs，将里面的内容情况，并加入如下内容(注意最下方的script标签中的内容换成你之前在swiftype中得到的代码)
```
<% if(theme.swift_search.enable) { %>
<div id="st-results-container" style="width:80%">正在加载搜索结果，请稍等。</div>
<style>.st-result-text {
background: #fafafa;
display: block;
border-left: 0.5em solid #ccc;
-webkit-transition: border-left 0.45s;
-moz-transition: border-left 0.45s;
-o-transition: border-left 0.45s;
-ms-transition: border-left 0.45s;
transition: border-left 0.45s;
padding: 0.5em;
}
@media only screen and (min-width: 768px) {
.st-result-text {
padding: 1em;
}
}
.st-result-text:hover {
border-left: 0.5em solid #ea6753;
}
.st-result-text h3 a{
color: #2ca6cb;
line-height: 1.5;
font-size: 22px;
}
.st-snippet em {
font-weight: bold;
color: #ea6753;
}</style>
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v1/st.js','_st');

  _st('install','obSA9H9BoiM58jQ6Vp4V');
</script>
<% } %>
```

八. 打开footer.ejs,再次将之前得到的代码copy到最后面
```
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v1/st.js','_st');

  _st('install','obSA9H9BoiM58jQ6Vp4V');
</script>
```

这样就完成了，最后你只需将将你的hexo博客重新hexo g,hexo d即可。

### 参考文献
+   [利用swiftype为hexo添加站内搜索](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype.html)