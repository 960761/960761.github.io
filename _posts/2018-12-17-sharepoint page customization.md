sharepoint page customization

根据survey 和 GTRA 两个项目总结而来。

# sharepoint 相关

1. 若要一系列的网页都被定制，则需要在其使用的模板页上面添加js, css文件，方法为在head中正常添加js,css文件；  
有时候会因为浏览器缓存等原因，使得js，css文件修改后仍然加载旧文件的情况，这种情况下，最有效的方法是将文件重命名。

2. 若要某一个特定网页进行定制，只需在此网页.aspx文件中添加js,css文件，添加的位置需要注意，一般为 PlaceHolderAdditionalPageHead content placeholder里面，有时需要根据具体定制内容确定添加位置。

3. 在外部js 文件中利用JSOM访问list item值，参考以下链接，直接复制过来代码稍加修改就可以使用：  
https://docs.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/hh185007(v=office.14)

4.	要注意，添加的控件div里面的内容需要从List 中获取，这些操作要放到onSucceed()函数中，因为这些获取List的操作是异步的，所以即便放到下面也不能确保一定是在获取到之后，只能放到Onsucced里面才可以确保是在正确获取了List内容后再进行相关操作。
为了确保读取List操作是在sp.js; sp.runtime.js等依赖js加载后执行，可以调用以下函数，注意这个函数是会调用function的，所以不需要再次运行。  
ExecuteOrDelayUntilScriptLoaded(function, "sp.js");


5. 针对sharepoint UI 的定制和普通页面有些不同，因为sharepoint中的页面不是普通的html页面，而是master page + .aspx得来的，所以先要通过网页F12源码查找到要修改的元素，然后使用html中各种方法进行定位i，最后再进行定制操作。

6. 客户端修改asp 控件，在form提交时会出现验证问题，这是由于验证功能EnableEventValidation =  true引起的，
解决这个问题的方法，网上找到的一般两大类：  
第一，	修改这个配置为False，这会降低安全性，而且在这里也是行不通的，这个标志位在.aspx中的 page指令中也是不可以修改的，只能到web.config文件中修改；  
第二，	在code behind file  xx.cs文件中进行代码修改，因为获取不到newForm.aspx的code behind file，所以这类方法也是行不通的。  

    最后采取的方法是，将asp的dropdown控件替换成纯粹的html select控件，因为这些html控件的值是不会自动保存到list item中的，所以还需要一个中介，这里就再增添两个text field，list中真正的也是这个text field，text field的值则由自定义的dropdown selected value来控制。这样就实现了由自定义的drop down control 控制list中的text field的值的功能。

7.	使用selected control时，在IE上面有一个bug，就是点击select 所在行空白处时会自动默认选择第一项，而这种默认选择又不会激发 select onchange  event事件，导致级联菜单没有效果。尝试覆盖td / tr onclick事件也不起效果，最后只好在select 上面添加一条空白选项放在第一项，绕过去这个bug。这也提示了一点，解决问题思路非常非常重要，比如这个问题，之前一直想着怎么解决，但是却没有想过通过添加空白项绕过去。

8.	checkVP 拉取信息异步的问题，最后使用check 里面结合setTimeout调用自身来解决，通过检测目标值是否存在，不存在的话延迟一秒再调用自身，要注意的是一定记得设置一个最长时限，否则会一直互相调用，类似死循环，这里使用计数器设置最长时长为5s，否则就不再验证，继续下面的操作。



9. 在需要从List中根据某些条件查询时，使用caml query时，caml语句可以使用下面这个工具生成，但要记着添加<view>标签。
Caml generator:


# 纯JS功能

1. 如果js文件有语法错误，那个有可能会导致整个js文件都加载不进去，所以如果发现js文件没有被引用进去发挥作用，那么有可能是js文件中代码错误，注释掉有可能出错的代码进行调试。

2. 删除dropdown all items  
selecControl.options.length = 0;  实现删除dropdown 所有选项

3. JS获取table特定行列
~~~
   var valueTd=document.getElementById("tbl").rows[1].cells[2];
   var val=valueTd.innerHTML ;
~~~
4. 使用JS添加table td/cell
~~~
    var cell=document.createElement("td");  
    cell.id=i+"/"+j;
    cell.appendChild(document.createTextNode("第"+cell.id+"列"));  
    row.appendChild(cell);
~~~

5. JS获取当前页面url  
   window.location.href (设置或获取整个 URL 为字符串)  
   详细参考： https://www.jianshu.com/p/073f79c5e438

6.	jquery元素定位  
使用jquery实现级联定位，引用变量时如何使用，比如parentEle，定位为 $(“#” + parentEle.id + “ #childID”)。

7.	IE8中checkbox  
IE8中的checkbox没有Onchange 事件，解决方法是使用  onpropertyChange事件，或者click中再调用blur()才会激发onchange  
详细可参考： http://msnvip.iteye.com/blog/350835

8.	IE8中的hidden  
Hidden在IE8中不起作用，需要使用display来实现。Display=”none”; 让其隐藏，display = “”让其显示。

9.	js修改inline style  
有些属性命名会有些小差异，比如border-bottom为 borderBottom，  
全部的对应关系请参考  
https://webcache.googleusercontent.com/search?q=cache:FdmMRo6dUQYJ:https://blog.csdn.net/sinolzeng/article/details/41594913%2B&cd=3&hl=en&ct=clnk&gl=us

10.	jquery中的.after()和.before()  
在使用.after(“xxx”), .before(“xxx”),时，不是针对ele，也即，若使用var eles = $(“xxx”); 那么就使用eles.after(“xxx”); 而不是eles[0]来调用，这一点一定要清楚，可以通过查看，若为 object(p)类型，也即object集合，或者有context属性则可以使用。


11. 绑定事件处理函数  
   既可以在原来的默认上面添加，使用addEventListener(not IE), or attachEvent(IE)，也可以先使用onclick = null, onclick = function myOwnFtn(){xxx}; 来完全覆盖。

   - 使用jquery进行绑定
   
    $('#btn_submit').click(function(){

    });


   - 使用原生js绑定  
    （注意：Internet Explorer 8 及更早IE版本不支持 addEventListener() 方法，Opera 7.0 及 Opera 更早版本也不支持。 这类浏览器版本要使用 attachEvent() 方法来添加事件）

    document.getElementById("#btn_submit").addEventListener(‘click’, function(){}, false);

补充：addEventListener的第三个参数是用于决定事件模型的，当父元素和子元素都绑定了事件时，这个参数决定先触发哪个事件，false为冒泡事件模型：即子元素绑定的事件先响应，父元素绑定的事件后相应，true问捕获事件模型，与冒泡事件模型执行顺序相反，如：

~~~
<div id="test_div">
    <button type="button" value ="测试事件顺序" name="测试事件顺序" id="test_button">测试事件顺序</button>
 </div>

document.getElementById('test_div').addEventListener('click', function () {console.log('div');},true)
document.getElementById('test_button').addEventListener('click', function(){console.log('test1');},false);
~~~
这个例子的事件模型是捕获模型，会先执行div的事件再执行button的事件，这里有个需要注意的地方：决定事件模型的是父元素绑定事件时传的第三个参数，如上例中button绑定事件时传的第三个参数是不起作用的，除非它又包含了子元素。

jQuery绑定为追加绑定，绑定多少方法就执行多少；  
原声js绑定为覆盖绑定。


12. form submit实现方式

Html页面中的form提交时有两种方式，  
第一种是 input  type=”submit” ，点击时， form onsubmit事件会被触发；
第二种是 Input type=”button”，一般的按钮，点击时，调用其onclick函数。
http://webcache.googleusercontent.com/search?q=cache:HwOBT9NSVg4J:wsj123.iteye.com/blog/2398047+&cd=3&hl=en&ct=clnk&gl=us  

onsubmit属性的触发时机是在form用input:submit这样的button提交时才会触发，否则不会触发。如果是用一个普通input:button，则在onclick属性中指定一个javascript函数，在这个函数里面再执行form的submit()函数，而不是onsubmit属性。 


13. Form submit button默认实现  
   在asp.net 2.0中，微软使用了WebForm_DoPostBackWithOptions函数代替了以前版本所使用的__doPostBack函数。但也不是所有的情况都有替代，只有满足以下条件的时候，WebForm_DoPostBackWithOptions才会出现。     
   1,Button控件设置了PostBackUrl属性。  
   2,未对控件设置CausesValidation="false"属性，按钮启用了验证机制，所以自动启用了WebForm_DoPostBackWithOptions函数进行数据验证。  
   具体的html 示例代码如下：
   ~~~
   ＜input type="submit" name="Button2" value="Postback to Second Page" onclick="javascript:WebForm_DoPostBackWithOptions(new WebForm_PostBackOptions("Button2", "", false, "", "SecondPage.aspx", false, false))" id="Button2" /＞ 
   ~~~
   定制中，经常会用到重写submit button onclick的情况，这个时候就需要自己调用 WebForm_DoPostBackWithOptions()，这个是异步函数。

   默认的Onclick 一般为以下： 
   ~~~
    if(!PreSaveItem()){
    return false;
    } 
    else{
       WebForm_DoPostBackWithOptions(new WebForm_PostBackOptions(name, "", true, "", "", false, true));        
    } 
   ~~~
