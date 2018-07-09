title: html + css 布局-对齐与居中
date: 2015-10-29 17:22:05
tags: [html,css,layout]
categories: layout
---
近段时间,由后端开发转到了前端开发,学了些html + css 布局的东西, 其中要说到居中的问题,居中分为水平居中和垂直居中.
<!-- more -->

## 水平居中
水平居中相对比较简单.有两种方法可以实现

### 文字水平居中
如果是一段文字需要水平居中的话,可以使用css 的text-align属性:取值有left/right/center, 分别代表左对齐,右对齐,居中对齐,如:
``` css
p {
    /* 居中对齐 */
    text-align:center
}
```

### div居中对齐
div的居中对齐也好做,只需设置一个div的左右margin 为auto即可,如
``` css
div {
    margin: 0 auto;
    width: 500px;
}
```
div的宽度是必须要设置的,如果不设置div默认是占整行宽度.

## 垂直居中
垂直居中相对于水平居中来说会比较麻烦些, 总的来说有如下几种方法:

### 单行文字垂直居中
对于只有单行文字的内容, 需要将文字水平居中,可以使用line-height属性,将line-height属性设置成父容器的height值即可. 如果不设置文本的父容器高度,父容器的高度将会撑开到line-height的值.
```
/* css */
span {
    line-height: 400px;    
}
/* html */
<span>hello world</span>
```
文字hello world将在span里面居中对齐, 并且span的高度将撑开成400px.

### 垂直居中图片1
垂直居中某一张图片可以在图片的前面加上一个span元素,并设置span元素的样式如下, 即可使图片居中.如果还需要将图片水平居中,可以设置box的line-height为box的高度即可
```
/* css */
.box {
    width: 500px;
    height: 300px;
    background-color: #aaaaaa;
}
.box span {
    height: inherit;
    display: inline-block;
    vertical-align: middle;   
}
.box img {
    vertical-align: middle;
}

/* html */
<div class="box"><span></span><img src="hello.png" alt="hello"></div>
```
### 垂直居中图片2
设置父容器的line-height的值为其height的值,并且设置img的vertical-align的值为middle即可.
```
/* css */
.box {
    width: 500px;
    height: 300px;
    line-height: 300px;
    background-color: #aaaaaa;
}
.box img {
    vertical-align: middle;
}

/* html */
<div class="box"><img src="hello.png" alt="hello"></div>
```
### div在父容器中垂直居中
设置父容器相对定位, 子容器的css代码如下
```
.box {
    position: relative;
    width:400px;
    height: 400px;
}
.sub {
    width: 50%;
    height: 200px;
    overflow: auto; //内容高度超过容器高度显示滚动条
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
}
```

### div在视区(屏幕)内垂直居中
如果想要一个一直在屏幕的正中间,则可以使用如下代码
```
.box {
    width: 200px;
    height: 200px;
    overflow: auto;
    margin: auto;
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 999;
}
```

### div在父容器居中高度自适应

```
/* css */
.box {
    position: relative;
    width:400px;
    height: 400px;
    background-color: #dd00dd;
}
.sub {
    background-color: #dddddd;
    width: 50%;
    height:auto;
    overflow: auto;
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    display: table;
}
/* html */
<div class="box">
    <div class="sub"></div>
</div>
```

### div在父容器中垂直居中2

```
/* css */
.box {
    width:400px;
    height: 400px;
    background-color: #dd00dd;
    position:relative;
}
.sub {
    background-color: #dddddd;
    width: 300px;  
    height: 200px;  
    padding: 20px;  
    position: absolute;  
    top: 50%; 
    left: 50%;  
    margin-left: -170px; /* (width + padding)/2 */  
    margin-top: -120px; /* (height + padding)/2 */  
}
/* html */
<div class="box">
    <div class="sub"></div>
</div>
```

### div在视区(屏幕)内垂直居中2

```
/* css */
.box {
    background-color: #dddddd;
    width: 300px;  
    height: 200px;  
    padding: 20px;  
    position: absolute;  
    top: 50%; 
    left: 50%;  
    margin-left: -170px; /* (width + padding)/2 */  
    margin-top: -120px; /* (height + padding)/2 */  
}
/* html */
<div class="box"></div>
```

### div在视区(屏幕)内垂直居中3
利用css3的变形
```
/* css */
.box {
    background-color: #dddddd;
    width: 50%;  
    height:200px;
    margin: auto;  
    position: absolute;  
    top: 50%; left: 50%;  
    -webkit-transform: translate(-50%,-50%);  
    -ms-transform: translate(-50%,-50%);  
    transform: translate(-50%,-50%); 
}
/* html */
<div class="box"></div>
```

