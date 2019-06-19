
## 模板引擎

模板原理：  
模板的诞生是为了将显示与数据分离，模板技术多种多样，但其本质是将模板文件和数据通过模板引擎生成最终的HTML代码。  

模板技术并不是什么神秘技术，干的是拼接字符串的体力活。模板引擎就是利用正则表达式识别模板标识，并利用数据替换其中的标识符。比如：

Hello, <%= name%>

数据是{name: '木的树'}，那么通过模板引擎解析后，我们希望得到Hello, 木的树。  
模板的前半部分是普通字符串，后半部分是模板标识，我们需要将其中的标识符替换为表达式。

模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。 
模板引擎不属于特定技术领域，它是跨领域跨平台的概念。在Asp下有模板引擎，在PHP下也有模板引擎，在C#下也有，甚至JavaScript、WinForm开发都会用到模板引擎技术。


比如我们需要在页面渲染一个列表：
~~~
<li>111</li>
<li>222</li>
<li>333</li>
~~~

列表中的数据是动态获取的一个数组data=['111','222','333']。
如果不用模板引擎，直接用代码写，需要循环data，然后使用字符串拼接每一个li的数据：

~~~
html += "<li>" + data[i] + "</li>";
~~~

这就导致html 和 js 代码混杂在了一起，后期维护和阅读都不方便，模板引擎就可以用来将html和js进行解耦。把代码逻辑直接写在一个html里面，只要更换数据源，就能输出不同的页面代码了。



## Handlebars 模板引擎

官方文档： http://handlebarsjs.com/

https://www.jianshu.com/p/c9c459c40250

https://webcache.googleusercontent.com/search?q=cache:QxA0wjWzScIJ:https://blog.csdn.net/m0_37836194/article/details/79041403+&cd=4&hl=zh-CN&ct=clnk&gl=hk




使用handlebars一般来说都是为了解决从后台拿数据，并实现后台数据前端渲染的问题。

是js的一个语义模板库，通过view和data的分离来快速构建Web模板。采用“Logic-less template”（无逻辑模板）的思路，Handlebars兼容Mustache，你可以在Handlebars中导入Mustache模板。handlebars支持的浏览器及运行环境有：IE6+ Chrome Firefox Safari5+ Operall11+以及node.js


 handlebars 相当于一个js库，如同jquery一样，可以从http://handlebarsjs.com 中下载handlebars.js库，然后和引用Jquery库一样通过script标签将其添加引用。 

 Handlebars的基本语法极其简单,使用{{value}}将数据包装起来即可,Handlebars会自动匹配响应的数值和对象。

## Handlebars 使用


 一个简单的示例：  


**第一，引入Hiberbars**

像普通的js库一样引用
~~~
<script src="./js/handlebars-1.0.0.beta.6.js"></script>
~~~

**第二，定义template**

template  和 普通的html相同， 只是增加了 {{xxx}}，这里面的内容需要动态数据接口来确定。
 
通过一个script将需要的模板包裹起来，在script标签中填入type和id。  
type类型可以是除text/javascript以外的任何MIME类型,推荐type="text/template",更加语义化，防止被当成一般的javascript被解析。id为了后期编译时找到该模板。
~~~
<script id="entry-template" type="text/x-handlebars-template">
  <div class="entry">
    <h1>{{title}}</h1>
    <div class="body">
      {{body}}
    </div>
  </div>
</script>

~~~


 **第三，编译template，生成目标html**

 ~~~
//get template by ID, like other html elements 
var source   = document.getElementById("entry-template").innerHTML;

//compile
var template = Handlebars.compile(source);

//pass data into the template
var context = {title: "My New Post", body: "This is my first post!"};
var html    = template(context);

得到的html为： 

<div class="entry">
  <h1>My New Post</h1>
  <div class="body">
    This is my first post!
  </div>
</div>
 ~~~
可以将其插入页面当中显示：

document.getElementById("divID").innerHTML = html；

实用示例：

~~~

html 内容： 

<!--模板. -->  
<!--需要数据的地方，用{{}}括起来.-->  
<script id="address-template" type="text/x-handlebars-template">  
   <p>You can find me in {{city}}. My address is {{number}} {{street}}.</p>  
</script>  
   
<!--新的内容在这里展示-->  
<div class="content-placeholder"></div>  

javascript代码：

$(function () {

   // 抓取模板  
   var theTemplateScript = $("#address-template").html();  
   
   // 编译模板  
   var theTemplate = Handlebars.compile(theTemplateScript);  
   
   // 定义数据  
   var context={  
      "city": "London",  
      "street": "Baker Street",  
      "number": "221B"  
    };  
    
    // 把数据传送到模板  
    var theCompiledHtml = theTemplate(context);  
    
    // 更新到页面显示  
    $('.content-placeholder').html(theCompiledHtml);  
  });  

~~~

实例参考：

 https://webcache.googleusercontent.com/search?q=cache:X6pxofzTyVUJ:https://www.cnblogs.com/diligenceday/p/4105229.html+&cd=3&hl=zh-CN&ct=clnk&gl=hk


 https://webcache.googleusercontent.com/search?q=cache:ZRmpdGcQTLQJ:https://wm4n.github.io/%25E4%25BD%25BF%25E7%2594%25A8-Handlebars-js-%25E8%25BC%2595%25E9%25AC%2586%25E5%2581%259A-HTML-Template-%25E4%25B8%2580/+&cd=2&hl=zh-CN&ct=clnk&gl=hk




## html中使用handlebars

在html中使用handlebars两种方式（类比javascript）： 

一，直接嵌入  
二，使用.hbs文件外部链接


在html 中嵌入使用handlebars.js：以下代码可以拿来直接执行
~~~
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>test</title>
</head>
<body>

<script src="http://libs.baidu.com/jquery/1.9.0/jquery.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/handlebars.js/2.0.0/handlebars.js"></script> 

<div id="div1"></div>

<script id="entry-template" type="text/x-handlebars-template">    
    <div class="entry">
      <h1>{{title}}</h1>
      <div class="body">
        {{body}}
      </div>
    </div>
</script>

<script>
    //JS代码
    var source   = $("#entry-template").html();
    var template = Handlebars.compile(source);
    
    var context = {title: "标题", body: "我是字符串!"}
    var html    = template(context);
    document.getElementById("div1").innerHTML = html;
</script>

</body>
</html>

~~~


.hbs文件是什么？  
类比js，将template code拿出来存为 .hbs文件；将js code拿出来存为 .js文件；

.hbs文件中的内容就是 
~~~
<script id="entry-template" type="text/x-handlebars-template"> 
~~~
里面定义的template的内容，所以在.hbs中看不到这些标签，就类似于.js中看不到script标签一样。


.hbs文件是如何被使用到.html中去的？（not clear yet）


那么.js是通过 script 标签添加引用，.hbs文件如何添加引用呢？？ 目前.hbs没有试用成功。


若将模板代码写成单独的.hbs文件时，需要在Js 中使用ajax请求该文件；  

handlebars模板是可以预编译的，需要提前安装一些handlebars引擎相关工具，预编译后.hbs文件会变成一个 .js文件，可以当成普通的.js文件通过script标签在html文件中引用，但是在html中具体使用模板的格式还不太清楚，直接{{xxx}}使用吗？ 
 
参考： https://webcache.googleusercontent.com/search?q=cache:qCG5u8Di_iAJ:https://codeday.me/bug/20180721/198443.html+&cd=1&hl=zh-CN&ct=clnk&gl=hk

handlebars官网中也提到，在Prod上面是不建议使用Js compile模板的，还是使用预编译的方式。


## others

 http://webcache.googleusercontent.com/search?q=cache:ipr_TlWS070J:web.jobbole.com/85948/+&cd=6&hl=zh-CN&ct=clnk&gl=hk


express中的handlebars引擎是这么生成页面的：
~~~

 /* layout.hbs
 * 主模板，所有的的页面都将替换"{{{body}}}"，"{{}}"相当于占位符，由数据进行替换
 */
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    {{{body}}}
  </body>
</html>
 


/* index.hbs
 * 单个页面模板，这里以首页为例。"{{>}}"表示引用其他模板来替换，这里引用名为"partial"的模板
 */
<div>index</div>
{{>partial}}
 


/* partial.hbs
 * 一个分页文件，被其他模板引用，分页之间也可以互相引用。
 */
<div>123</div>
 


/* index.html
 * 当浏览器请求index.html时，经过handlebars模板引擎处理后生成的页面
 */
 <!DOCTYPE html>
 <html>
   <head>
     <title></title>
   </head>
   <body>
     <div>index</div>
     <div>123</div>
   </body>
 </html>

 
~~~


