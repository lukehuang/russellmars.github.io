title: 移动版的2048
date: 2015-03-12 21:52:44
tags: [game,编程]
categories: game
---
上一篇博客已经介绍过网页版的2048了，源代码也有提供，而这篇博客主要讲讲2048这个游戏的进阶。
<!--more-->
上一版的2048已经能完美的运行在pc版的浏览器中了，但是不支持在手机端的愉快的玩耍，这一个问题主要分两个方面解决，详细参考慕课网[教学视频](http://www.imooc.com/learn/76)，点击[mobile2048](http://hzhyiyy.com/game/mobile2048/)试玩。
+   屏幕适应问题
+   触摸滑动事件

## 屏幕适应问题
如果想要你的游戏既能够在pc中的浏览器中看起来很舒服，并且在各种流行的屏幕尺寸的手机上也也能完美的显示，就的考虑一个问题，屏幕自适应问题，也称响应式布局。
首先，当你想要适应各种屏幕尺寸的话，你所显示的游戏大小就不能是一个定死的常量了，而是一个根据屏幕而变化的变量。
我在`support2048.js`中加入了四个全局变量
``` javascript
documentWidth = window.screen.availWidth;
gridContainerWidth = 0.92 * documentWidth;
cellSideLength = 0.18 * documentWidth;
cellSpace = 0.04 * documentWidth;
```
documentWidth：网页文档宽度，差不多也就是你的手机屏幕的宽度。
gridContainerWidth：整个游戏区域的宽度。
cellSideLength：每个数字格子的宽度。
cellSpace：每个格子之间的间隙距离。

在我们所有的js的方法中的所有涉及到直接写像素值得地方都要注意是否需要修改成我们的变量了。我们从`main2048.js`开始
```
$(document).ready(function(){
    prepareForMobile();
    newgame();
});
```
增加一个函数
```
function prepareForMobile(){

    if(documentWidth > 500){
        gridContainerWidth = 500;
        cellSpace = 20;
        cellSideLength = 100;
    }

    $("#grid-container").css("width", gridContainerWidth - 2*cellSpace);
    $("#grid-container").css("height", gridContainerWidth - 2*cellSpace);
    $("#grid-container").css("padding", cellSpace);
    $("#grid-container").css("border-radius", 0.02 * gridContainerWidth);

    $(".grid-cell").css("width", cellSideLength);
    $(".grid-cell").css("height", cellSideLength);
    $(".grid-cell").css("border-radius", 0.02 * cellSideLength);
}
```

然后在init的方法中使用了如下语句
```
gridCell.css("top", getPosTop(i, j));
gridCell.css("left", getPosLeft(i, j));
```

故相应要修改其中的两个方法
```
function getPosTop(i, j){
    return cellSpace + i*(cellSideLength + cellSpace);
}

function getPosLeft(i, j){
    return cellSpace + j*(cellSideLength + cellSpace);
}

```

还有`updateBoardView()`方法也有很多需要修改的地方
```
function updateBoardView(){
    $(".number-cell").remove();
    for(var i = 0; i < 4; i++){
        for(var j = 0; j < 4; j++){
            $("#grid-container").append('<div class="number-cell" id="number-cell-' + i+ '-' + j + '"></div>');
            var theNumberCell = $("#number-cell-"+ i + "-" + j);

            if(board[i][j] == 0){
                theNumberCell.css("width", "0px");
                theNumberCell.css("height", "0px");
                theNumberCell.css("top", getPosTop(i, j) + cellSideLength/2);
                theNumberCell.css("left", getPosLeft(i, j) + cellSideLength/2);
                theNumberCell.css("font-size", 0.6 * cellSideLength + "px");
            } else {
                theNumberCell.css("width", cellSideLength);
                theNumberCell.css("height", cellSideLength);
                theNumberCell.css("top", getPosTop(i, j));
                theNumberCell.css("left", getPosLeft(i, j));
                theNumberCell.css("background-color", getNumberBackgroundColor(board[i][j]));
                theNumberCell.css("color", getNumberColor(board[i][j]));
                theNumberCell.text(getNumberText(board[i][j]));
                theNumberCell.css("font-size", getNumberFontSize(board[i][j]) + "px");
            }

            hasConflicted[i][j] = false;
        }
    }

    $(".number-cell").css("line-height", cellSideLength + "px");
    $(".number-cell").css("border-radius", 0.02 * cellSideLength);
}
```

我在里面还单独添加了一个方法getNumberFontSize(number),用于不同数字时的文字大小显示，我把它放在了support2048.js中了，具体如下：
```
function getNumberFontSize(number){
    switch (number){
        case 2 : return 0.6 * cellSideLength; break;
        case 4 : return 0.6 * cellSideLength; break;
        case 8 : return 0.6 * cellSideLength; break;
        case 16 : return 0.5 * cellSideLength; break;
        case 32 : return 0.5 * cellSideLength; break;
        case 64 : return 0.5 * cellSideLength; break;
        case 128 : return 0.4 * cellSideLength; break;
        case 256 : return 0.4 * cellSideLength; break;
        case 512 : return 0.4 * cellSideLength; break;
        case 1024 : return 0.35 * cellSideLength; break;
        case 2048 : return 0.35 * cellSideLength; break;
        case 4096 : return 0.35 * cellSideLength; break;
        case 8192 : return 0.35 * cellSideLength; break;
    }

    return 0.3 * cellSideLength;
}
```
然后就是显示动画效果的方法了
```
function showNumberWithAnimation(i, j, randNumber){
    var numberCell = $("#number-cell-" + i + "-" + j);
    numberCell.css("background-color", getNumberBackgroundColor(randNumber));
    numberCell.css("color", getNumberColor(randNumber));
    numberCell.text(getNumberText(randNumber));

    numberCell.animate({
        width: cellSideLength,
        height: cellSideLength,
        top: getPosTop(i, j),
        left: getPosLeft(i, j)
    }, 100);
}

function showMoveAnimation(fromx, fromy, tox, toy){
    var numCell = $("#number-cell-"+fromx+"-"+fromy);
    numCell.animate({
        top : getPosTop(tox, toy),
        left : getPosLeft(tox, toy)
    },200);
}
```
屏幕适应的大体上就那么多需要修改了，当然还有些细节，可能我没有注意到，在文章最下方我会给出源码。

## 触摸滑动事件
在pc上的浏览器当然可以使用键盘操作游戏，但是在手机端就没有键盘了，所以就需要添加滑动的监听事件。
恰好原生的js就支持touch事件，这里我们主要用到touchstart和touchend事件，当然还有一个touchmove事件，顾名思义，这三个事件分别代表触摸开始，触摸结束，以及滑动时。
首先我们先定义4个全局的变量，分别代表触摸开始的x，y坐标，和触摸结束的x，y坐标。
```
var startx = 0;
var starty = 0;
var endx = 0;
var endy = 0;
```

然后就是对touchstart和touchend事件的监听
``` javascript
document.addEventListener("touchstart", function(event){
    startx = event.touches[0].pageX;
    starty = event.touches[0].pageY;
});
document.addEventListener("touchmove", function(event){
    //据说是防止一个android某版本的一个bug，需要监听touchmove，并使用如下语句。
    event.preventDefault();
});
/*监听touchStart事件*/
document.addEventListener("touchend", function(event){
    endx = event.changedTouches[0].pageX;
    endy = event.changedTouches[0].pageY;
    var deltax = endx - startx;
    var deltay = endy - starty;
    //当触摸的跨度大于屏幕宽度的0.2时才算是一次滑动
    if(Math.abs(deltax) < 0.2 * documentWidth && Math.abs(deltay) < 0.2 * documentWidth){
        return;
    }
    var canMove = false;
    if(Math.abs(deltax) >= Math.abs(deltay)){  //x
        if(deltax > 0){  //move right
            if(moveRight()){
                canMove = true;
            }
        } else { //move left
            if(moveLeft()){
                canMove = true;
            }
        }
    } else {  //y
        if(deltay > 0){  //move down
            if(moveDown()){
                canMove = true;
            }
        } else {  //move up
            if(moveUp()){
                canMove = true;
            }
        }
    }
    if(canMove){
        setTimeout("generateOneNumber()", 210);
        setTimeout("isgameover()", 300);
    }
});
```
需要修改的地方大概就那么多，当然可能有些地方在hmtl和css中我都做了些微调，有些没有照顾到的地方，还请见谅。下面我附上我的新改进的2048的源码

## 源码下载
源代码我已经放在我的github中了，下载请点击[mobile2048](https://github.com/hzhyiyy/mobile2048)


