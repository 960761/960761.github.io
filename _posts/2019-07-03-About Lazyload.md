

## lazyload

lazyload 指缓冲加载，即有些内容只有在你需要看到它时才会被加载并显示。

这里主要介绍jquery的lazyload 插件，用来缓冲加载图片。

如果一篇文章很长有很多图片的话，下载图片就需要很多时间，这款插件，会检测你的滚动情况，只有你要看到那个图片的时候，它才会从后台请求下载图片，然后显示出来。使用这个插件，可以在需要显示图片的时候，才下载图片，所以可以减少服务器的压力，避免不必要的资源下载。如果一个人不看下面的图片，那加载下面的图片就是一种浪费。

## jquery lazy load原理 
在 img 标签中添加新的属性，把 src 属性的值指向占位图片，也可以不再使用src属性，添加 data-original 属性，让其指向真正的图像地址。

## jquery lazy load使用  

第一步：加载相关文件。  

首先要加载jquery和这个插件。使用以下代码，加载这几个文件（使用CDN）：  

CDN：https://cdnjs.com/libraries/jquery

~~~
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.lazyload/1.9.1/jquery.lazyload.js"></script>
~~~

第二步：定义图片结构。

按照官方的建议，定义你的img结构：  

四大属性必须同时具备：class   data-original   width  height  

class用来定位需要使用lazyload的元素；   
data-original用来指向实际图片位置，  
width, height用来设置图片大小，  
另外src用来指向占位符图片位置，也可以不使用，不使用的话原图片未加载时是灰色的区域。

~~~
<img class="lazy" src="img/grey.gif" data-original="img/example.jpg" width="640" heigh="480">
~~~

第三步：触发这个插件，生效。

具体使用的语句为：

~~~
$("img.lazy").lazyload();
~~~

完整code:
~~~
<html lang="en">
   <head>
      <title>lazy load demo</title>
	  <style type="text/css">
	  </style>
   </head>
   
   <body>
      This is for lazyload demo:</br>
      <img src="1.png" width="750", height = "460"/>
	  <img src="1.png" width="750", height = "460"/>
	  
	  <img class="lazyload"   alt="lazyload" data-original="2lazy.png" width="750" height="360">
	  <img class="lazyload"   alt="lazyload" data-original="3lazy.png" width="750" height="360">
	  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
	  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.lazyload/1.9.1/jquery.lazyload.js"></script>
      <script>
           $("img.lazyload").lazyload({
            threshold: -200,
            effect: "fadeIn"
        });
      </script>
   </body>
</html>
~~~

## jquery lazy load高级用法  

lazyload()函数中还可以使用很多参数，就是lazyload的高级用法了，有以下常用的参数可选：  

**提前加载**：  

默认的情况是，当你滚动到图片位置的时候，插件开始加载。这样，用户可能首先看到的是一个空白图像，然后再缓慢出现。如果你想在用户滚动之前，提前加载这个图像，你可以配置一下参数。  

threshold: 200,    // 当距离图片还有200像素的时候，就开始加载图片。


**自定义显示效果**：  

默认的图片实现效果，就是没有效果，下载完成之后，直接显示出来。这样的用户体验并不好，你可以设置 effect 属性，来控制显示图片的效果。  

effect: "fadeIn",  // 控制显示图片的效果，fadeIn的效果淡入动画。 


**自定义触发事件**：

默认的触发事件是滚动，默认情况下，是要等到用户向下滚动并且图像出现在显示区域时才触发。你可以使用event属性设置你自己的加载事件，之后你可以自定义触发这个事件的条件，然后去加载图像。

event: "click",     


**加载不可见图像**： 

有部分图像是不可见的，我们对其加上类似 display：none 等属性的图像。默认的情况下，这个插件是不会加载隐藏的不可见图像。如果我们需要用它加载不可见图像，我们需要将 skip_invisible 设置为 false

$("img.lazy").lazyload({ skip_invisible : false });


## 浏览器不支持javascript时的降级处理

应对这个问题，我们需要引入 noscript 标签。大体思路如下：  
用 noscript 包含真实的图像位置，当浏览器不支持 Javascript，直接显示图像。对现有图像，隐藏处理，使用 show() 方法触发显示。这样，如果浏览器不支持 Javascript，我们自定义的 img 就不会出现，而显示 noscript 里面的图像。

具体实现代码如下：

~~~
<img src="img/grey.gif" data-original="img/example.jpg" width="640" heigh="480"> 
<noscript><img src="img/example.jpg" width="640" heigh="480"></noscript>

<script type="text/javascript"></script>
   $("img.lazy").show().lazyload();
</script>
~~~

**参考**  
https://webcache.googleusercontent.com/search?q=cache:NWv-tK1s4joJ:https://blog.wpjam.com/article/jquery-lazyload-js/+&cd=4&hl=zh-CN&ct=clnk&gl=hk
