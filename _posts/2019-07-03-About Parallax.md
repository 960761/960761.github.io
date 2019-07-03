

## 视差滚动(parallax scrolling)

**定义**  

视差效果，原本是一个天文学术语，当我们观察星空时，离我们远的星星移动速度较慢，离我们近的星星移动速度则较快。当我们坐在车上向车窗外看时，也会有这样的感觉，远处的群山似乎没有在动，而近处的稻田却在飞速掠过。许多游戏中都使用视差效果来增加场景的立体感。说的简单点就是网页内的元素在滚动屏幕时发生的位置的变化，然而各个不同的元素位置变化的速度不同，导致网页内的元素有层次错落的错觉，这和我们人体的眼球效果很像。

parallax 是一种新型的web design trend  

是的前景图像和背景图像以不同的速率scroll 来营造3D 的效果

**缘起**

在研究AEM UI 时，在corporate and blog site中都用到了parallax的技术，所以开始学习这种技术。  

jquery中提供了实现  parallax的插件，但是推测这里没有使用 jquery提供的,而是自己实现的。 


在HTML/ 中有 parallax.js和parallax.css 文件，  
分析corporate .html source code，发现实现的关键是 parallaxcontainer class。  


**原理**

页面上很多的元素在相互独立地滚动着，如果我们来对其它分层的话，可以有两到三层 ：背景层，内容层，贴图层 
当滚动鼠标滚轮的时候，各图层以不同速度移动，形成视差的效果。这就是时差滚动的大致原理。  
原理是这样，但落实到技术细节上时，实现的方法却各种各样。


差异滚动的实现规则：  
•　　背景层的滚动(最慢)  
•　　贴图层(内容层和背景层之间的元素)的滚动(次慢)  
•　　内容层的滚动(可以和页面的滚动速度一致)  

让三个图层的滚动速度不一致，就做出了漂亮的差异滚动效果  



**实现**

直接去研究corporate 的 parallax.js 文件不太现实，可以先从如何实现parallax  scroll 功能入手，自己先去实现一下，然后再去看应该会容易入手一些。  

基本实现：
1. 在CSS中定义背景滚动方式的属性backgroud-attacthment

background-attachment -- 定义背景图片随滚动轴的移动方式

•取值: scroll | fixed | inherit •scroll: 默认值。背景图像会随着页面其余部分的滚动而移动。  
•fixed: 当页面的其余部分滚动时，背景图像不会移动。  
•inherit: 规定应该从父元素继承 background-attachment 属性的设置。  
•初始值: scroll  
•继承性: 否  
•适用于: 所有元素  


**当页面滚动到图片应该出现的位置，被设置了 background-attachment: fixed 的图片并不会继续跟随页面的滚动而跟随上下移动，而是相对于视口固定死了。**


附带w3c的链接：https://www.w3schools.com/howto/howto_css_parallax.asp

浏览器的支持性：测试了chrome,opera,safari,firefox,ie7-8都是可以的，所以就是说IE6下不行~  

在IE6下使用这个属性，需要把background-attachment:fixed放置于body或html当中，就是说你说在其它标签里面是没用。

2. 使用 transform: translate3d 实现滚动视差(不太明白，也没看出效果)

https://webcache.googleusercontent.com/search?q=cache:SjhHgzZMkaoJ:https://juejin.im/post/5b6d0756e51d4562b31ad23c+&cd=5&hl=zh-CN&ct=clnk&gl=hk

原理是：

a.我们给容器设置上 transform-style: preserve-3d 和 perspective: xpx，那么处于这个容器的子元素就将位于3D空间中，

b.再给子元素设置不同的 transform: translateZ()，这个时候，不同元素在 3D Z轴方向距离屏幕（我们的眼睛）的距离也就不一样

c.滚动滚动条，由于子元素设置了不同的 transform: translateZ()，那么他们滚动的上下距离 translateY 相对屏幕（我们的眼睛），也是不一样的，这就达到了滚动视差的效果。

**Demo Code**

01. background image fixed  
背景图不动的情况下，很容易实现
~~~
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>

    section {
        height: 100vh;
    }

    .g-img {
        background-image: url("wait.jpg");
        background-attachment: fixed;
        background-size: cover;
        background-position: center center;
    }

</style>
</head>
<body>
	<section class="g-word">Header</section>
	<section class="g-img">IMG1</section>
	<section class="g-word">Content1</section>
	<section class="g-img">IMG2</section>
	<section class="g-word">Content2</section>
	<section class="g-img">IMG3</section>
	<section class="g-word">Footer</section>
		
</body>
</html>

~~~

2. 背景图也会移动，但是速率比前景内容慢，这样就和corporate site的效果很相似了  

关键是window.scroll的实现，将下面代码的scroll去掉之后，就和上面 background fixed的情况一样了
~~~
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>

/*if parallax height is percentage, this must have*/
body, html{
  height: 100%;
}
.parallax {
  /* The image used */
  background-image: url("wait.jpg");

  /* Set a specific height */
  height: 600px; /*if 100%, body and html must 100%*/
  color: red;

  /* Create the parallax scrolling effect */
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

#bg2 {
  background-image: url("wait2.jpg");
}

.foreground{
  height: 800px;
  background-color: blue;
  font-size: 24px;
  color: red;
}

</style>
</head>
<body>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
	<p>Scroll Up and Down this page to see the parallax scrolling effect.</p>

	<div class="parallax" id="bg1">background</div>

	<div class = "foreground">
	Scroll Up and Down this page to see the parallax scrolling effect.
	This div is just here to enable scrolling.
	Tip: Try to remove the background-attachment property to remove the scrolling effect.
	</div>

	<div class="parallax" id="bg2">background</div>

	<div class = "foreground">
	Scroll Up and Down this page to see the parallax scrolling effect.
	This div is just here to enable scrolling.
	Tip: Try to remove the background-attachment property to remove the scrolling effect.
	</div>
	
	<script>
	  $(window).scroll(function(){
	    //alert( $(window).scrollTop() );
						
		var yPos = $(window).scrollTop()/2.0;			       
		var coords = '50% ' + '-' + yPos + 'px';
		$("#bg1").css({"background-position" : coords});
		
        var yPos2 = $(window).scrollTop()/4.0;;
		var coords2 = '50% ' + '-' + yPos2 + 'px';
		$("#bg2").css({"background-position" : coords2});
	  
	  });
	
	
	</script>

</body>
</html>


~~~


**about typescript**  
typescript 简单了解：  

typescript 之于 javascript 就像 less 之于 css；
typescript是javascript的一个超集，或者理解为一种预编译语言，javascript是弱类型，且不支持静态检查，typescript则支持类型和静态检查，而且支持 类，接口等概念，  

对于熟悉 面向对象编程的人可以使用typescript来写代码，  
typescript 代码文件为 .ts文件，这个文件是不可以直接使用的，需要转化为 .js文件来使用。这等同于.less不能直接用，要转化为 .css 使用一个道理。

**about parsys in AEM**  

parSys: paragraph system  
可以将parsys理解为 layout containter，在里面可以拖放components  


跨不同视口的响应行为是parsys和layout容器之间的主要区别。

使用parsys，您可以拖放组件，并且这些组件的位置在所有视口中保持不变。例如，为桌面视口水平放置的tile组件，并且您希望它在视口减小时垂直堆叠，只需使用段落系统，您需要CSS更改或像bootstrap这样的响应式框架来实现此目的。
使用布局容器，您可以获得响应功能OOTB，您可以在布局模式下为不同的视口定义组件的位置。


