---
title: 禁止系统字体缩放
date: 2018-07-23 16:57:38
tags: [web]
---

禁止系统字体缩放对页面的影响

### android
需要给webview设置
``` java
webview.getSettings().setTextZoom(100)
```

### ios
无需设置webview，只需要给body设置样式：
``` css
body {
  -webkit-text-size-adjust: none !important;
}
```
