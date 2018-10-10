
VB和VB script  

Visual Basic简称VB，是一种由 Microsoft开发的可视化程序设计语言。  
VBScript是Visual Basic Script的简称，即 Visual Basic 脚本语言，有时也被缩写为VBS，它是一种微软环境下的轻量级的解释型语言。  
Visual Basic Script 是 Visual Basic 语言的一个子集合，是以 VB语言为基础发展出来的一种简单型的脚本语言，  
VB语言多用于Windows环境下的编程，而VBS多用于ASP环境下的网页编程,主要运用在利用ADODB控件访问数据库,以作动态网站。  

### VB
Visual Basic是Microsoft在1991年首次发布的第三代事件驱动编程语言。最终版本是Visual Basic 6. VB 是为初学者设计的一种易学的编程语言，它可以让我们轻松地开发视窗应用程序。  
Visual Basic 是一个视觉和事件驱动的程序语言。在 Visual Basic里，用户可点击任何一对象，每个对象都需要设计一套程序来应对这些动作。  
因此， 一个Visual Basic程序里包含了许多子程序，每个子程序都有自己的程序代码，每个程序都可独立操作，也可联合起来进行操作。  

变量声明：
~~~
Dim password As String
~~~
流程控制：
~~~
If  条件 式 Then  
VB 陈述句
Else  
VB 陈述句
End If
Select Case  表达式
   Case 数值1 
         VB 语句
   Case 数值2 
        VB 语句
   Case 数值3 
        VB 语句 
        . 
        . 
   Case Else 
        VB 语句
End Select 
~~~

Msgbox的作用是显示一个弹出式消息框，并提示用户点击一个命令按钮以继续进行下一个任务。相当于javascript 中的alert();  
Msgbox的结构如下：  
~~~
          yourMsg=MsgBox(Prompt, Style Value, Title) 
~~~
 Msgbox 中的第一个参数 Prompt 是用来显示在消息框中的信讯息。   
 参数 Style Value 是确定什么类型的命令按钮出现在消息框中  
 参数Title 显示留言板上的标题  

InputBox （ ）函数将显示一个消息框以便用户可以输入一个值或一个信息。  

自定义函数：  
~~~
Public  Function functionName (Arg As dataType,..........) As dataType 
或
Private  Function functionName (Arg As dataType,..........) As dataType 
~~~

数组声明：  
~~~
Dim arrayName(subs) As dataType
~~~

错误处理：
~~~
On Error GoTo program_label
~~~
其中program_label是 用于处理用户犯下误差时的一段代码。一旦发现误差，程序将跳转到误差处理program_label部分。

### VB script
轻量级的脚本语言, 大小写不敏感  
只有在一定的宿主环境下才能够正确发挥作用，比如IE， IIS，Windows Scripting Host (WSH)等  
和javascript 类似，是由microsoft在1996年发明，因此目前只有IE browser支持，最新版本是5.8，  
Since there is no development environment available by default, debugging is difficult.  
VBScript engine would be shipped as part of all Microsoft Windows and IIS by default.  
凡是以’开头的都被认为是注释。


