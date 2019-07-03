sharepoint page customization

根据survey 和 GTRA 两个项目总结而来
基于incident action  && contract renewal 项目

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
    
    
10. 页面上的button 点击时会默认刷新整个页面，如何阻止：  

为了达到页面不刷新的效果，可以在button的onclick属性里加 return  false;
代码如下：
~~~
//js代码
function onclick(){
    //your code here
     return false;
}
~~~

11. 调用share point webservice 创建list item  

使用的field name 是 internal name，需要到 list settting获取  
list setting -> Click on column name [Column (click to edit)] -> you will see    

https://xxxx/_layouts/FldEdit.aspx?List=%7BAFBE0273%2D69F4%2D48EC%2DB7A1%2DF9AF8483AF57%7D&Field=Issue%5Fx0020%5FSummary  

Field= 后面的值是 Internal name，将 %5F替换为_，其余不变。

在使用 camlQuery时，使用的也是 field的internal name。


12. people picker列赋值  
在给people picker 列赋值时，可以赋空，但不可以赋字符串，对于people picker类型的字段必须赋特定类型的值才可以，这个特定的类型就是 fieldUserValue，需要以下来创建  
SP.FieldUserValue.fromUser（）  
里面的参数必须是 corp\\e610374，不可以是Liu, xin  
所以需要从resolved的people picker来获取。  


13. 从 share point people picker中获取人员信息  

都需要首先手动check,获得 resolved value，然后才可以获取，有两种获取方式：
一种，使用share point webservice，   
在sp2013及以后，需要加载 clientpeoplepicker.js，然后使用
~~~

 function getEmailFromPeoplePicker(title) {
    //Get the people picker field
    var ppDiv = $("div[title='xx']")[0];
    //cast the object as type PeoplePicker
    var peoplePicker = SPClientPeoplePicker.SPClientPeoplePickerDict[ppDiv.id];
    //Get list of users from field (assuming 1 in this case)
    var userList = peoplePicker.GetAllUserInfo();
    var userInfo = userList[0];
    var userEmail;
 
    if(userInfo != null)
    {
        userEmail = userInfo.EntityData.Email;
    }
 
    return userEmail;
}
~~~
正常情况下， EntityData包括七种信息。  
具体参考：  
https://webcache.googleusercontent.com/search?q=cache:8GcCj7jjJPgJ:https://prairiedeveloper.com/2016/12/getting-user-data-from-people-picker-with-javascript/+&cd=9&hl=zh-CN&ct=clnk&gl=us

注意：在sp2013之前，2010中是没有 clientpeoplepicker.js的，只能使用下面这种方法。 

另一种是解析xml: 

在resolved后，里面会有一段xml，包含所有详细的信息，示例如下：  
~~~
"<ArrayOfDictionaryEntry xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\">
   <DictionaryEntry> <Key xsi:type=\"xsd:string\">SPUserID</Key><Value xsi:type=\"xsd:string\">2335</Value> </DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">AccountName</Key><Value xsi:type=\"xsd:string\">CORP\\a638836</Value></DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">Email</Key><Value xsi:type=\"xsd:string\">CFei2@StateStreet.com</Value></DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">Department</Key><Value xsi:type=\"xsd:string\">CTC Corp Systems Ops</Value></DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">SIPAddress</Key><Value xsi:type=\"xsd:string\">CFei2@StateStreet.com</Value></DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">PrincipalType</Key><Value xsi:type=\"xsd:string\">User</Value></DictionaryEntry>
  <DictionaryEntry><Key xsi:type=\"xsd:string\">Title</Key><Value xsi:type=\"xsd:string\">Contractor</Value></DictionaryEntry>
</ArrayOfDictionaryEntry>"
~~~

使用javascript解析这段xml语句来获取相关信息，使用js解析xml 可参考：  
http://www.w3school.com.cn/xmldom/dom_parser.asp

14. operate share point list with javascript Object Model:

Retrieve List Items Using JavaScript:
https://docs.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/hh185007%28v%3doffice.14%29


Common Programming Tasks in the JavaScript Object Model：
https://docs.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/hh185015%28v%3doffice.14%29


15. form 元素disable但是还需要被保存的实现    
    不可以简单使用disabled = "disabled" 来实现，因为disabled后，这个值就不会被保存了，如果是对checkbox or radio使用diabled，它们的 change handler也不再运行了。  

    使得元素不可编辑，通常有三种方式：  
    disable:  后台不可以接收传值  
    readonly: 后台可以接收传值，不可以编辑  
    unselectable: 不可以接收焦点，不可以选择，可以后台传值

    对share point form中几种常见元素：  

    input类型：   
    使用 unselectable = "on", 然后使用css 设置 unselectable = on的元素的背景色和前景色和 disable相似，实现 伪 disabled 效果；

    radio, checkbox ,dropdown：   
    两种方式：   
    第一，使用隐藏列，将这几种列设为diaable, 将它们的值保存到hidden column传给后台；
    第二，设置disabled，save时再enable，
          这种方式在点击 save时，因为disable = false，在UI上会有些变化，可能会引起用户体验不好，可以使用一个小技巧，在enable之后，将对应控件的背景色和字体颜色都设置成和disable时相同，造成一种 disable的假象。


    对于people picker 和 date类型：   
    在输入框部分可以使用unselectable，但是同时还要注意将右边的 name book or calendar隐藏 
    


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
   
14. CDN online js library:

https://cdnjs.com/libraries/jquery  

15. IE8中使用console.log 注意事项  
   
IE8只有在开启调试窗口(F12)的时候，console.log 才能出结果，不然就报错。这就会出现奇怪的现象，即F12一切正常，不进入F12时会出现错误。


console.log 原先是 Firefox 的"专利"，严格说是安装了 Firebugs 之后的 Firefox 所独有的调试"绝招"。 
 
IE浏览器下默认是不支持console.log，反而会因为这句代码报错，如果正式发布代码时忘记将console注释掉，就会出错误。

解决办法是声明该console对象的log函数为空函数： 
~~~
if(!window.console){
 window.console = {log : function(){}};
}
~~~
console.log()等调试代码应当从最终的产品代码中删除掉。


16. javascript 程序生成日志
   
第一，可以使用 log4javascript.js，但是这个是弹出网页，而不是保存为txt文件，所以不太理想。


参考：
http://log4javascript.org/docs/quickstart.html  

示例：
~~~
<!DOCTYPE html>
<html>
  <head>
    <title>test.html</title>
    <meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
    <meta http-equiv="description" content="this is my page">
    <meta http-equiv="content-type" content="text/html; charset=UTF-8"> <!-- 我引入的是本地的js  -->
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/log4javascript/1.4.9/log4javascript.js"></script>
	<script type="text/javascript">
	//因为log4javaScript不能准确定位，虽然每次写log.info时加入准确信息也可以但是麻烦
	//这里以title为标志用于定位页面 便于搜索
	    var log = log4javascript.getDefaultLogger(); 
		var pageName=document.title;
		log.trace(pageName,"我在这里加入了trace信息");
		log.debug(pageName,"我在这里加入了debug信息");
		log.info(pageName,"我在这里加入了info信息");
		log.warn(pageName,"我在这里加入了warn信息");
		log.error(pageName,"我在这里加入了error信息");
		log.fatal(pageName,"我在这里加入了fatal信息");
	</script>
  </head>
  
  <body>
    This is my HTML page. <br>
  </body>
</html>
~~~


第二，自己写log类或函数，网上找到一个 debugout.js，可以参考一下，

https://github.com/inorganik/debugout.js


https://webcache.googleusercontent.com/search?q=cache:1WxKrUNrWRUJ:https://www.helplib.com/GitHub/article_111981+&cd=9&hl=zh-CN&ct=clnk&gl=hk



17. 使用jquery修改元素属性  
    多个属性:  
    $("img").attr({ src: "test.jpg", alt: "Test Image" });  

    单个属性:  
    $("img").attr("src","test.jpg");


18. 判断输入为空字符   
    最好要 trim()一下，再判断。


19. share point page中插入js方法  
   第一，在.aspx 或者 .master 中使用 script and link 标签，分别插入.js, .css文件；    
   第二，插入 content editor web part中，将js, css全部写在一个.txt文件。


20.  checkbox, radio 事件处理  
    Checkbox, radio没有Onchange事件，而且即便有，也是在失去焦点时才触发，因此可以使用 onpropertychange事件处理。
