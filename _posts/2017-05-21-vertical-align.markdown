---
layout: post
title:  "垂直居中"
date:   2017-05-21 14:39:18 +0800
categories: jekyll update
---
咋一看标题，可能你会觉得非常简单，因为这是极其常见的需求，但只有当你真正去实践过，才会发现这并没有想象中那么简单。长久以来，前端开发者们为此殚精竭虑，琢磨出了各种解决办法，本篇文章将利用现代css的威力，去解决这一难题。
先从先前所使用的方法及其弊病讲起。
### 1.表格布局法
{% highlight ruby %}
<div class="container">
    <div class="content">
       centered text
    </div>
</div>
.container{
    display: table;
    width: 100%;
height: 200px;
    background-color: yellowgreen;
}
.content{
   display: table-cell;
   text-align: center;
   vertical-align: middle;
}
{% endhighlight %}
利用了表格的显示模式，弊病是需要用到冗余的HTML元素
### 2.行内块法
{% highlight ruby %}
<div class="container">
    <div class="content">
       centered text
    </div>
</div>
.container{
    width: 100%;
    height: 200px;
    background-color: yellowgreen;
    white-space: nowrap;
    text-align: center;
}
.container:before {
   content: '';
   display: inline-block;
   height: 100%;
   vertical-align: middle;
   margin-right: -0.25em; /* Adjusts for spacing */
}
.content{
   display: inline-block;
   vertical-align: middle;
   width: 300px;;
}
{% endhighlight %}
利用了行内块元素的vertical-align: middle属性，弊病是也用到了冗余的HTML元素，而且hack的味道很浓
### 3.基于绝对定位 
{% highlight ruby %}
<div class="centered"></div>
.centered{
   position: absolute;
   top: 50%;
   left: 50%;
   margin-top: -3em; /* 6/2 = 3 */
   margin-left: -9em; /* 18/2 = 9 */
   width: 18em;
   height: 6em;
   background-color: yellowgreen;
 }	
 {% endhighlight %}
相信这是很多人使用过的方法，先把元素的左上角放置在正中心，然后利用负外边距向上向左移动自身的一半以使它居中。可以看出来，有一个很明显的弊病，就是元素的宽高必须固定。而在通常情况下，元素的宽高是由其内容决定的，也就是不固定的，那么有没有一种属性是相对于其自身作为基准的呢？可能你已经想到了，那就是css3属性中的translate属性，用上述思路，上面代码可以修改为
{% highlight ruby %}.centered{
   position: absolute;
top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: yellowgreen;
 }	
 {% endhighlight %}
很简洁，很完美有没有？但是，没有任何一种技巧是完美无缺的，上面这种方法也有一些弊病之处：
用了绝对定位，对整个布局影响有时过于强烈
如果需要居中的元素在高度上超过了视口，那他的顶部会被视口裁切掉
在某些浏览器中，这个方法可能会导致元素显示有一些模糊，这个问题可以用transform: preserve-3d来修复，但很难保证未来不会出问题
### 4.基于视口
假设我们不想使用绝对定位，仍用translate()来把这个元素以其自身宽高的一半为距离来进行移动，那么在缺少left和top的情况下我们怎样把元素的左上角移到正中央呢？可能你会想到用margin，但是margin的百分比是以父元素的宽度来作为基准的，即使对于margin-top和margin-bottom也是这样。好在我们只需基于视口居中，所以我们可以把margin-top的百分比改为以基于视口的高度作为标准。在css的单位里面有一个叫vh的单位，1vh表示视口高度的1%，那么我们可以更新一下我们的代码：
{% highlight ruby %}
.centered{
      margin-top: 50vh;
  margin-left: 50%;
  transform: translate(-50%, -50%);
  background-color: yellowgreen;
 }
 {% endhighlight %}
不难看出，这个技巧的最大弊病就是使用性有限，因为它只能基于视口居中
### 5.基于flexbox
如果有对弹性盒子模型不熟悉的同学可以参考阮一峰老师的这篇文章[http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)	
{% highlight ruby %}
<div class="centered">centered text</div>
.centered1{
   display: flex;
   align-items: center;
   background-color: yellowgreen;
}	
{% endhighlight %}
超级简洁有没有？仅仅几行代码就完美地实现了垂直居中，如果要实现水平居中，只需要加上justify-content: center就可以实现，可见现代css的强大威力，解决了多年来让前端工程师头疼的问题。
好了，现在你对什么情况下该使用何种垂直居中方式应该有了一个清晰的认识，在项目中你可以根据实际情况选择适合自己的解决方式。



