title: 布局练习1
date: 2015-10-30 15:38:25
tags: [layout,html,css]
categories: layout
---
布局练习系列第一篇, 利用position 布局, 可以实现div元素的各种摆放位置, 也是很灵活的, 下面我们将实现下列布局, 如图:
<!-- more -->
![目标布局](/image/2015/08/layout-exercise-01/layout-target.png)

代码如下:
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<style>
    .box {
        width: 500px;
        margin:0 auto;
        padding: 20px;
        background-color: #999d9c;
        height:auto;
    }
    .box1 {
        position:relative;
        height:auto;
    }
    .box2 {
        margin-top: 20px;
        position:relative;
    }
    .box .box1 .div1 {
        width:76%;
        display: inline-block;
        background-color: #009ad6;
    }
    .box .box1 .div2 {
        display: inline-block;
        width:20%;
        background-color: #dbce8f;
        position:absolute;
        top:0;
        right:0;
        height:100%;
    }
    .box .div3 {
        width:48%;
        display: inline-block;
        background-color: #ae5039;
    }
    .box .div4 {
        width:48%;
        display: inline-block;  
        background-color: #525f42;
        position:absolute;
        top:0;
        right:0;
    }

</style>
<body>
    <div class="box">
        <div class="box1">
            <div class="div1">
                div1
                <p></p>
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Consectetur ex asperiores quisquam voluptates laudantium porro suscipit laborum commodi, molestias atque laboriosam unde ducimus vero quibusdam, id ullam aspernatur facilis! Veniam.
            </div>
            <div class="div2">
                div2
            </div>
        </div>
        <div class="box2">
            <div class="div3">
                div3
            </div>
            <div class="div4">
                div4
            </div>
        </div>
    </div>
</body>
</html>
```
div1的高度,自适应div2的高度, 整个外层的div高度也是自适应内容的高度.

结果如下图:
![布局结果](/image/2015/08/layout-exercise-01/layout-result.png)