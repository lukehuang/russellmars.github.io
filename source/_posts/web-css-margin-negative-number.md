---
title: margin 负值的应用
date: 2019-02-22 17:11:52
tags: ['css', 'web']
---
margin负值平常用的比较少，最近写一个盒子上流式布局，加上一些边距，使用一般的css比较难实现，后来用来margin负值，轻松解决，这里把一些margin负值的实际应用做一个总结，包括居中问题，等宽布局，等高布局，圣杯布局，双飞燕布局等。
<!--more-->
## 水平垂直居中

``` html
<div class="container1">
  <div class="box1"></div>
</div>
```
``` css
.container1 {
  position: relative;
  width: 200px;
  height: 200px;
  background: yellow;
}
.box1 {
  position: absolute;
  width: 50px;
  height: 50px;
  top: 50%;
  left: 50%;
  margin-top: -25px;
  margin-left: -25px;
  /* left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto; */
  background: red;
}
```
![1](/image/2019/02/web-css-margin-negative-number/1.png)

## 子项在盒子上流式布局
``` html
<div class="list">
  <div class="list-wrapper">
    <div class="item">项目1</div>
    <div class="item">项目2</div>
    <div class="item">项目3</div>
    <div class="item">项目4</div>
    <div class="item">项目5</div>
    <div class="item">项目6</div>
    <div class="item">项目7</div>
    <div class="item">项目8</div>
    <div class="item">项目9</div>
    <div class="item">项目10</div>
    <div class="item">项目11</div>
  </div>
</div>
```
``` css
.list {
    margin-top: 0.1rem;
    /* width: inherit; */
    width: 361px;
    overflow: hidden;
    background: green;
  }
  .list-wrapper {
    margin-right: -10px;
    margin-top: -10px;
  }
  .item {
    display: inline-block;
    padding: 5px 12px;
    background: red;
    font-size: 14px;
    color: #212121;
    border-radius: 1rem;
    margin-right: 10px;
    margin-top: 10px;
  }
```
![2](/image/2019/02/web-css-margin-negative-number/2.png)

## 多列等宽布局
``` html
<div class="container">
  <div class="wrapper">
    <div class="box">
      <div class="sbox">box1</div>
    </div>
    <div class="box">
      <div class="sbox">box2</div>
    </div>
    <div class="box">
      <div class="sbox">box3</div>
    </div>
    <div class="box">
      <div class="sbox">box4</div>
    </div>
  </div>
</div>
```
``` css
.container {
  background: green;
  height: 200px;
  overflow: hidden;
}
.wrapper {
  margin-right: -10px;
  height: 100%;
}
.box {
  float: left;
  height: 100%;
  width: 25%;
}
.sbox {
  height: 100%;
  margin-right: 10px;
  background: red;
}
```
![3](/image/2019/02/web-css-margin-negative-number/3.png)

## 多列等高布局
``` html
<div class="container">
  <div class="wrapper">
    <div class="box">hello world</div>
    <div class="box">Lorem ipsum dolor sit amet consectetur adipisicing elit. Eum voluptas necessitatibus distinctio.</div>
    <div class="box">Lorem ipsum dolor sit amet consectetur, adipisicing elit. Veritatis ipsa, accusantium error ipsum sint maxime fugit ea. Ipsum nobis unde natus vero tempore, dignissimos odio?</div>
  </div>
</div>
```
``` css
.container {
  width: 400px;
  background: green;
}
.wrapper {
  overflow: hidden;
  margin-right: -20px;
}
.box {
  background: red;
  float: left;
  width: 120px;
  padding-bottom: 9999px;
  margin-bottom: -9999px;
  margin-right: 20px;
}
```
![4](/image/2019/02/web-css-margin-negative-number/4.png)

## 圣杯布局
``` html
<div class="container">
  <div class="main">Lorem ipsum dolor sit, amet consectetur adipisicing elit. Ea aspernatur sunt veniam ipsum repellendus quo officia exercitationem a asperiores, aliquam rerum. Odit molestias quibusdam ad earum molestiae maiores. Iste, enim?</div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```
``` css
.container {
  padding-left: 200px;
  padding-right: 300px;
  overflow: hidden;
}
.main {
  width: 100%;
  background: red;
  float: left;
}
.left {
  position:relative;
  left: -200px;
  width: 200px;
  float: left;
  margin-left: -100%;
  background: green;
}
.right {
  position:relative;
  right: -300px;
  width: 300px;
  float: left;
  margin-left: -300px;
  background: yellow;
}
```
![5](/image/2019/02/web-css-margin-negative-number/5.png)

## 圣杯布局等高
``` html
<div class="container">
    <div class="main">Lorem ipsum dolor sit, amet consectetur adipisicing elit. Ea aspernatur sunt veniam ipsum repellendus quo officia exercitationem a asperiores, aliquam rerum. Odit molestias quibusdam ad earum molestiae maiores. Iste, enim?</div>
    <div class="left">left</div>
    <div class="right">right</div>
  </div>
```
``` css
.container {
  padding-left: 200px;
  padding-right: 300px;
  overflow: hidden;
}
.main, .left, .right {
  padding-bottom: 9999px;
  margin-bottom: -9999px;
}
.main {
  width: 100%;
  background: red;
  float: left;
}
.left {
  position:relative;
  left: -200px;
  width: 200px;
  float: left;
  margin-left: -100%;
  background: green;
}
.right {
  position:relative;
  right: -300px;
  width: 300px;
  float: left;
  margin-left: -300px;
  background: yellow;
}
```
![6](/image/2019/02/web-css-margin-negative-number/6.png)

## 双飞翼布局
``` html
<div class="container">
  <div class="middle">
    <div class="main">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Natus quo aliquid ipsam sed officia ducimus aspernatur quia repellat, harum, modi quisquam omnis a nobis laboriosam consectetur aperiam eveniet officiis assumenda?</div>
  </div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```
``` css
.container {
  overflow: hidden;
}
.middle {
  float: left;
}
.main {
  margin-left: 100px;
  margin-right: 200px;
  background: red;
}
.left {
  float: left;
  width: 100px;
  margin-left: -100%;
  background: green;
}
.right {
  float: left;
  width: 200px;
  margin-left: -200px;
  background: yellow;
}
```
![7](/image/2019/02/web-css-margin-negative-number/7.png)

## 等高双飞翼布局
``` html
<div class="container">
  <div class="middle">
    <div class="main">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Natus quo aliquid ipsam sed officia ducimus aspernatur quia repellat, harum, modi quisquam omnis a nobis laboriosam consectetur aperiam eveniet officiis assumenda?</div>
  </div>
  <div class="left">left</div>
  <div class="right">right</div>
</div>
```
``` css
.container {
  overflow: hidden;
}
.middle, .left, .right {
  padding-bottom: 9999px;
  margin-bottom: -9999px;
}
.middle {
  float: left;
}
.main {
  margin-left: 100px;
  margin-right: 200px;
  background: red;
}
.left {
  float: left;
  width: 100px;
  margin-left: -100%;
  background: green;
}
.right {
  float: left;
  width: 200px;
  margin-left: -200px;
  background: yellow;
}
```
![8](/image/2019/02/web-css-margin-negative-number/8.png)