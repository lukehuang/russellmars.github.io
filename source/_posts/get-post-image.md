---
title: 前端post的方式获取图片
date: 2018-08-07 16:07:47
tags: [web, javascript]
---

在前端开发中有时候不得不使用post的方式去获取一张图片，如何把响应的图片数据在页面上显示

``` javascript
// response是响应体
const uint8Array = new Uint8Array(response.data)
// imageBase64 则是图片的base64数据，可以放在img标签的src中直接用来显示
// 在ios 上方法的参数长度有限制，所以不能直接使用String.fromCharCode方法调用
// let imageBase64 = window.btoa(String.fromCharCode.apply(null, uint8Array))
let imageBase64 = window.btoa(uint8Array.reduce(function (data, byte) {
  return data + String.fromCharCode(byte)
}, ''))
// 如果图片显示不出来，尝试加入图片类型
imageBase64 = 'data:image/jpeg;base64,' + imageBase64
```