title: hexo博客优化
date: 2015-02-09 10:39:55
tags: [hexo, 优化]
categories: hexo
---
弄了这么多，想必大家还是很好奇，hexo博客不也就能写写东西么，还能干嘛。其实这个问题，在之前我也是有提过的。我的答案就是自由博客，我的理解就是，你想干嘛就干嘛。这篇文章主要说说hexo博客的一些优化，或者是美观，个性化。
<!--more-->

## 一、修改默认日期显示格式
在hexo博客中，默认的日期显示格式是英文的。像这个样子
```
Feb 9 2015
```
如果你想改成下面这个样子的，那你就就要按照我接下来说的去做了
```
2015年02月9日
```
在你的博客的根目录下找到`_config.yml`文件，打开并找到`date_format`的关键字，样子大概如下
```
date_format: MMM D YYYY
```
你需要把它修改成这样就好了
```
date_format: YYYY年MM月D日
```

## 二、在导航栏中添加关于
在根目录中打开`git bash`，输入如下命令
```
hexo new page "about"
```
这个命令会这`source`目录下生成`about/index.md`的文件夹和文件。编辑这个`index.md`文件，里面的内容随便写，也就是你的关于页面的内容，写法就跟写平常的博客一样。
然后在在`themes/light/_config.yml`中，找到`menu`关键字，大概如下
```
menu:
  Home: /
  Archives: /archives
```
把上面的内容替换如下：
```
menu:
  首页: /
  存档: /archives
  关于: /about
```
## 三、添加友情链接
在`themes/light/layout/_widget`中新建一个文件，名字叫做`link.ejs`,在里面输入如下内容。
``` html
<div class="widget tag"> 
   <h3 class="title">友情链接</h3> 
   <ul class="entry"> 
    <li><a href="http://blog.csdn.net/hzhyiiyy" title="hzhyiyy">恒者亨也的博客</a></li> 
   </ul> 
</div>
```
在文件`themes/light/_config.yml`中找到`widgets`，大概如下
```
widgets:
- search
- category
- tag
```
把`link`添加进来即可
```
widgets:
- search
- category
- tag
- link
```

## 四、添加其他的widget
大家可以看到，我的博客里还有微博show，网易云之类的widget，其实做法和第三步中的添加友情链接是一个道理。我就一起说了。
首先，在`themes/light/layout/_widget`中新建连个文件`weibo.ejs`,`music.ejs`
`weibo.ejs`里要添加的内容去[这里](http://app.weibo.com/tool/weiboshow)找。
`music.ejs`里添加的内容去[这里](http://music.163.com/)，找一个你喜欢的单曲或者歌单，或者自己的歌单都可以，你会发现有 *生成音乐外链* 的字眼,点击进去你就明白了，
![音乐外链](/image/2015/02/hexo-optimizing/hexo-optimizing-music.jpg)

`weibo.ejs,music.ejs`的内容都编辑好了以后，还是需要去文件`themes/light/_config.yml`中配置，如下
```
widgets:
- search
- category
- tag
- link
- music
- weibo
```

## 五、添加“多说”评论
一篇博客必不可少需要有评论来相互交流，第三方评论插件，我也是听别人介绍说“多说”还不错，我也是使用的这个插件。
### 1.获得“多说”代码
首先，去[多说](http://duoshuo.com/)的官网少注册一个账号，然后会获得以下代码
``` html
<!-- 多说评论框 start -->
	<div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="请替换成文章的标题" data-url="请替换成文章的网址"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"hzhyiyy"};
	(function() {
		var ds = document.createElement('script');
		ds.type = 'text/javascript';ds.async = true;
		ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
		ds.charset = 'UTF-8';
		(document.getElementsByTagName('head')[0] 
		 || document.getElementsByTagName('body')[0]).appendChild(ds);
	})();
	</script>
<!-- 多说公共JS代码 end -->
```
### 2.替换模板
在你的博客目录下找到文件`themes\light\layout\_partial\comment.ejs`（主题不同可能不一样）,将里面的代码全部替换成以下代码
```
<% if (page.comments){ %>  
<section id="comment">  
//第一步中在“多说”中获得的代码 
</section>  
<% } %>  
```
注意，还需修改其中的代码，如下
**将`data-thread-key="请将此处替换成文章在你的站点中的ID"` 替换成`data-thread-key="<%= page.path %>"`**
**将`data-title="请替换成文章的标题"` 替换成 `data-title="<%= page.title %>"`**
**将`data-url="请替换成文章的网址"` 替换成 `data-url="<%= page.permalink %>"`**
``` html
<% if (page.comments){ %>
<section id="comment">
  <!-- 多说评论框 start -->
  <div class="ds-thread" data-thread-key="<%= page.path %>" data-title="<%= page.title %>" data-url="<%= page.permalink %>"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"hzhyiyy"};
  (function() {
    var ds = document.createElement('script');
    ds.type = 'text/javascript';ds.async = true;
    ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
    ds.charset = 'UTF-8';
    (document.getElementsByTagName('head')[0] 
     || document.getElementsByTagName('body')[0]).appendChild(ds);
  })();
  </script>
<!-- 多说公共JS代码 end -->

</section>
<% } %>
```
### 3.添加配置
在主目录下的文件`_config.yml`中增加如下代码
```
#duoshuo
duoshuo_shortname: hzhyiyy
```
并且将该文件里面的url改成你的网站地址，如
```
url: http://hzhyiyy.github.com
```
或者是(由于我已经申请了域名，当然是用上面的url也ok)
```
url: http://hzhyiyy.com
```
*做完上述步骤，博客里基本就能使用评论功能了*

## 六、分享功能
hexo默认会自带分享的功能，但都是分享到国外的网站，国内的朋友当然是能分享到qq空间，微博等社交网站，所以这里稍微修改了下hexo的分享功能，步骤如下：
+   在文件夹`themes\light\layout\_partial\post`中新建文件`baidushare.ejs`.
+   去[百度分享](http://share.baidu.com/)注册，并获得你的分享代码，并copy到baidushare.ejs文件中.
+   在文件`themes\light\layout\_partial\article.ejs`中找到`<%- partial('post/share') %>`,并把它替换成`<%-partial('post/baidushare')%>`

## 七、换机器
如果你换电脑了，但是还在用hexo博客写东西，那只能是copy你的blog源代码了，我虽然还没用换个电脑写博客，但还是有考虑过这个问题的。我的做法是用百度云自动备份我的blog文件夹。当然也是可以提交到代码托管服务器的。

## 八、回到顶部按钮
一般情况下，主题里面是没有回到顶部的按钮的，所以自能是自己加上这段代码了。
在`themes\light\layout\_partial`中新建文件`totop.ejs`,内容如下：
``` html
<div id="totop" style="position:fixed;bottom:5em;right:1em;cursor: pointer;">
<a title="返回顶部"><img src="/img/scrollup.png"/></a>
</div>
```
然后在`themes\light\source\js`中新建文件`totop.js`,内容如下：
``` javascript
(function($) {
// When to show the scroll link
// higher number = scroll link appears further down the page
var upperLimit = 1000;
// Our scroll link element
var scrollElem = $('#totop');
// Scroll to top speed
var scrollSpeed = 500;
// Show and hide the scroll to top link based on scroll position
scrollElem.hide();
$(window).scroll(function () {
var scrollTop = $(document).scrollTop();
if ( scrollTop > upperLimit ) {
$(scrollElem).stop().fadeTo(300, 1); // fade back in
}else{
$(scrollElem).stop().fadeTo(300, 0); // fade out
}
});
// Scroll to top animation on click
$(scrollElem).click(function(){
$('html, body').animate({scrollTop:0}, scrollSpeed); return false;
});
})(jQuery);
```
最后，在文件 `themes\light\layout\_partial\after_footer.ejs` 的最后加上：
``` ejs
<%- partial('totop') %>
<script src="<%- config.root %>js/totop.js"></script>
```
图片在这里![totop](/image/picture/scrollup.png)
## 九、添加标签评论
如图所示
![biaoqing](/image/2015/02/hexo-optimizing/hexo-optimizing-biaoqing.jpg)
这组表情，我也是偶然间发现的，觉得蛮有意思就弄了下，弄好之后发现这个东西设计的不够合理，你用了就知道，手机体验不太好，而且看起来可以连点，实际上是不行的。
我就大概说说怎么弄的吧。
1. 去搜狐[畅言](http://changyan.sohu.com/login?ru=%2Flabs%2Femoji) 里注册一个账号。
2. 找到实验室里的评论表情这个选项。然后你会获得一段代码（每个人的不一样）。
3. `themes\light\layout\_partial\post`里新建一个文件`biaoqing.ejs`,里面的内容就是在第二步中获得的代码。
4. `themes\light\layout\_partial\article.ejs`中的合适位置，将`<%- partial('post/biaoqing') %>` 加上就好了，我把它放在了`<%- partial('post/category') %>`的前面
5. 其实这时候是还没后表情的，回到畅言实验室里，在你获得代码的那个地方有个配置信息。看一下你就明白了，选择表情就好了，配置宽度，还有把追加评论去掉。

## 十、给页面添加一个时间
这个我也是从网上看到的是个html5 canvas的特效，就搬过来了。
+   `themes\light\layout\_widget`中新建`time.ejs`文件，内容如下：
``` html
<div class="widget tag"> 
   <div id="zzsc"></div>
</div>
```
+   `themes\light\source\js`下新建`zzsc.js`，内容如下(可根据自己的实际情况修改)：
``` javascript
$(function(){
$('#zzsc').html('<canvas id="canvas"></canvas>');
var WINDOW_WIDTH = 270;
    var WINDOW_HEIGHT = 100;
    var RADIUS = 2.2; //球半径
    var NUMBER_GAP = 3; //数字之间的间隙
    var u=0.65; //碰撞能量损耗系数
    var context; //Canvas绘制上下文
    var balls = []; //存储彩色的小球
    const colors = ["#33B5E5","#0099CC","#AA66CC","#9933CC","#99CC00","#669900","#FFBB33","#FF8800","#FF4444","#CC0000"]; //彩色小球的颜色
    var currentNums = []; //屏幕显示的8个字符
    var digit =
                [
                    [
                        [0,0,1,1,1,0,0],
                        [0,1,1,0,1,1,0],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,0,1,1,0],
                        [0,0,1,1,1,0,0]
                    ],//0
                    [
                        [0,0,0,1,1,0,0],
                        [0,1,1,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [1,1,1,1,1,1,1]
                    ],//1
                    [
                        [0,1,1,1,1,1,0],
                        [1,1,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,0,0],
                        [0,0,1,1,0,0,0],
                        [0,1,1,0,0,0,0],
                        [1,1,0,0,0,0,0],
                        [1,1,0,0,0,1,1],
                        [1,1,1,1,1,1,1]
                    ],//2
                    [
                        [1,1,1,1,1,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,0,0],
                        [0,0,1,1,1,0,0],
                        [0,0,0,0,1,1,0],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,1,1,0]
                    ],//3
                    [
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,1,0],
                        [0,0,1,1,1,1,0],
                        [0,1,1,0,1,1,0],
                        [1,1,0,0,1,1,0],
                        [1,1,1,1,1,1,1],
                        [0,0,0,0,1,1,0],
                        [0,0,0,0,1,1,0],
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,1,1]
                    ],//4
                    [
                        [1,1,1,1,1,1,1],
                        [1,1,0,0,0,0,0],
                        [1,1,0,0,0,0,0],
                        [1,1,1,1,1,1,0],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,1,1,0]
                    ],//5
                    [
                        [0,0,0,0,1,1,0],
                        [0,0,1,1,0,0,0],
                        [0,1,1,0,0,0,0],
                        [1,1,0,0,0,0,0],
                        [1,1,0,1,1,1,0],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,1,1,0]
                    ],//6
                    [
                        [1,1,1,1,1,1,1],
                        [1,1,0,0,0,1,1],
                        [0,0,0,0,1,1,0],
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,0,0],
                        [0,0,0,1,1,0,0],
                        [0,0,1,1,0,0,0],
                        [0,0,1,1,0,0,0],
                        [0,0,1,1,0,0,0],
                        [0,0,1,1,0,0,0]
                    ],//7
                    [
                        [0,1,1,1,1,1,0],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,1,1,0],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,1,1,0]
                    ],//8
                    [
                        [0,1,1,1,1,1,0],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [1,1,0,0,0,1,1],
                        [0,1,1,1,0,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,0,1,1],
                        [0,0,0,0,1,1,0],
                        [0,0,0,1,1,0,0],
                        [0,1,1,0,0,0,0]
                    ],//9
                    [
                        [0,0,0,0],
                        [0,0,0,0],
                        [0,1,1,0],
                        [0,1,1,0],
                        [0,0,0,0],
                        [0,0,0,0],
                        [0,1,1,0],
                        [0,1,1,0],
                        [0,0,0,0],
                        [0,0,0,0]
                    ]//:
                ];

    function drawDatetime(cxt){
      var nums = [];

      context.fillStyle="#005eac"
      var date = new Date();
      var offsetX = 25, offsetY = 15;
      var hours = date.getHours();
      var num1 = Math.floor(hours/10);
      var num2 = hours%10;
      nums.push({num: num1});
      nums.push({num: num2});
      nums.push({num: 10}); //冒号
      var minutes = date.getMinutes();
      var num1 = Math.floor(minutes/10);
      var num2 = minutes%10;
      nums.push({num: num1});
      nums.push({num: num2});
      nums.push({num: 10}); //冒号
      var seconds = date.getSeconds();
      var num1 = Math.floor(seconds/10);
      var num2 = seconds%10;
      nums.push({num: num1});
      nums.push({num: num2});

      for(var x = 0;x<nums.length;x++){
        nums[x].offsetX = offsetX;
        offsetX = drawSingleNumber(offsetX,offsetY, nums[x].num,cxt);
        //两个数字连一块，应该间隔一些距离
        if(x<nums.length-1){
          if((nums[x].num!=10) &&(nums[x+1].num!=10)){
            offsetX+=NUMBER_GAP;
          }
        }
      }

      //说明这是初始化
      if(currentNums.length ==0){
        currentNums = nums;
      }else{
        //进行比较
        for(var index = 0;index<currentNums.length;index++){
          if(currentNums[index].num!=nums[index].num){
            //不一样时，添加彩色小球
            addBalls(nums[index]);
            currentNums[index].num=nums[index].num;
          }
        }
      }
      renderBalls(cxt);
      updateBalls();

      return date;
    }

    function addBalls (item) {
      var num = item.num;
      var numMatrix = digit[num];
      for(var y = 0;y<numMatrix.length;y++){
        for(var x = 0;x<numMatrix[y].length;x++){
          if(numMatrix[y][x]==1){
            var ball={
              offsetX:item.offsetX+RADIUS+RADIUS*2*x,
              offsetY:30+RADIUS+RADIUS*2*y,
              color:colors[Math.floor(Math.random()*colors.length)],
              g:1.5+Math.random(),
              vx:Math.pow(-1, Math.ceil(Math.random()*10))*4+Math.random(),
              vy:-5
            }
            balls.push(ball);
          }
        }
      }
    }

    function renderBalls(cxt){
      for(var index = 0;index<balls.length;index++){
        cxt.beginPath();
        cxt.fillStyle=balls[index].color;
        cxt.arc(balls[index].offsetX, balls[index].offsetY, RADIUS, 0, 2*Math.PI);
        cxt.fill();
      }
    }

    function updateBalls () {
      var i =0;
      for(var index = 0;index<balls.length;index++){
        var ball = balls[index];
        ball.offsetX += ball.vx;
        ball.offsetY += ball.vy;
        ball.vy+=ball.g;
        if(ball.offsetY > (WINDOW_HEIGHT-RADIUS)){
          ball.offsetY= WINDOW_HEIGHT-RADIUS;
          ball.vy=-ball.vy*u;
        }
        if(ball.offsetX>RADIUS&&ball.offsetX<(WINDOW_WIDTH-RADIUS)){

          balls[i]=balls[index];
          i++;
        }
      }
      //去除出边界的球
      for(;i<balls.length;i++){
        balls.pop();
      }
    }
    function drawSingleNumber(offsetX, offsetY, num, cxt){
      var numMatrix = digit[num];
      for(var y = 0;y<numMatrix.length;y++){
        for(var x = 0;x<numMatrix[y].length;x++){
          if(numMatrix[y][x]==1){
            cxt.beginPath();
            cxt.arc(offsetX+RADIUS+RADIUS*2*x,offsetY+RADIUS+RADIUS*2*y,RADIUS,0,2*Math.PI);
            cxt.fill();
          }
        }
      }
      cxt.beginPath();
      offsetX += numMatrix[0].length*RADIUS*2;
      return offsetX;
    }

    var canvas = document.getElementById("canvas");
    canvas.width=WINDOW_WIDTH;
    canvas.height=WINDOW_HEIGHT;
    context = canvas.getContext("2d");

    //记录当前绘制的时刻
    var currentDate = new Date();

    setInterval(function(){
      //清空整个Canvas，重新绘制内容
      context.clearRect(0, 0, context.canvas.width, context.canvas.height);
      drawDatetime(context);
    }, 50)
});
```
+   `themes\light\_config.yml`中的`widgets:`下，加入如下内容：
```
- time
```


## 参考文献
+   [hexo](http://hexo.io/)
+   [hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/)
+   [hexo博客优化](http://blog.csdn.net/itmyhome1990/article/details/43489361)
+   [hexo中使用多说评论系统](http://blog.csdn.net/itmyhome1990/article/details/43536171)
+   [Hexo博客优化：添加返回顶部功能](http://wuchong.me/blog/2014/01/08/hexo-scrollup/)