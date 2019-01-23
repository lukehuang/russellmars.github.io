---
title: iphone x 的input顶起页面后隐藏不回弹
date: 2019-01-23 13:57:32
tags:
---
在微信的ios版本里，iphone x/iphone xs 等机型在键盘顶起页面后，收起键盘页面不会回弹，但是轻轻摸一下页面，又会正常的。在safari里是没有问题的，微信的webview里有这个bug，一开始还以为是自己的布局有问题，一顿操作，发现最简单的html流式布局就会出现这个bug，猜测这个应该是微信的bug，操作了一下，有两种方法可以解决
``` html
<input type="text" onblur="handleInputOnBlur(event)" placeholder="说点什么吧">
<script>
function handleInputOnBlur (event) {
  setTimeout(() => {
    window.scrollTo(0, document.body.scrollTop + 1);
    document.body.scrollTop >= 1 && window.scrollTo(0, document.body.scrollTop - 1);
  }, 10)
}
</script>
``` 
或者，
``` html
<input type="text" onblur="handleInputOnBlur(event)" placeholder="说点什么吧">
<script>
function handleInputOnBlur (event) {
  var count = 0;
  var screenHeight = window.screen.height;
  var blurInterval = setInterval(function() {
    document.body.style.minHeight = (screenHeight -= 10) + "px"
    if (count >= 10) {
      clearInterval(blurInterval);
      document.body.style.minHeight = ''
    }
    count++;
  }, 10);
}
</script>
``` 
具体用那种要看具体的页面布局，请自行尝试。