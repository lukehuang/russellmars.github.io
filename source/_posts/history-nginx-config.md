---
title: html5 history 路由模式下 nginx配置
date: 2017-07-11 11:08:08
tags: [html5, history, nginx]
---
现代化的web应用中多数采用 SPA（Single Page Application）的方式来编写网站或者应用，当你的 SPA 的路由方式使用的是html5的history模式的时候，需要服务器端做相应的配置，nginx 配置如下：
``` conf
server {
  location /erp-farm-admin {
    alias /home/erp-farm-admin;
    index index.html index.htm;

    if ($request_filename ~ .*.(html|htm)$) {
      add_header Cache-Control no-cache,no-store,must-revalidate;
    }

    if ($request_filename ~ .*.(js|css|jpe?g|png|gif)$) {
      add_header Cache-Control public,max-age=31536000;
      add_header expires 30d;
    }

    try_files $uri $uri/ /erp-farm-admin/index.html;
  }

}

```
假设我们有一个叫blog的项目，访问路径大概是`http://domain.com/FE/blog/`，而我们的blog项目的部署路径在`/data/static/FE/blog`下。加上这段配置，项目的路由就与普通的页面访问一致了。