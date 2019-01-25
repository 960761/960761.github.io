
## 参考书籍
1.	《Learn powershell over a month’s lunches》  
Show you how to user powershell during lunch time, includes 28 chapter, and each chapter will take you about 40 minutes to read, the whole book will take you about a month to finish.  
Online reading:      https://webcache.googleusercontent.com/search?q=cache:Gf-BN2UprqcJ:https://www.manning.com/books/learn-windows-powershell-in-a-month-of-lunches-second-edition+&cd=1&hl=en&ct=clnk&gl=us


2.	《Powershell in action》  
It is more about the underlying principles of powershell, about why things are what they are. You can download ebook and source code.

3. video   
https://channel9.msdn.com/Series/GetStartedPowerShell3

4. community  
https://powershell.org/


## 知识点
1. Start-Transcript  
这个命令可以将输入的cmd 记录输出保存到.txt文件中。

2. sharepoint management shell
这个是用来操作share point的shell，可以认为是基于powershell 的工具。

## video notes
https://channel9.msdn.com/Series/GetStartedPowerShell3

2017.11.24 chaper1-2: powershell get started, about the help system, a very rich system
1.	Always run powershell in admin mode, 
2.	Right click the powershell console top bar ,choose properties, you can set up the layout of powershell.
3.	Make use of the powerful help system, if you do not know what command to use, you can guess the critical noun or verb, then search related commands. For example, if you want to get services, but do not know exact commands, then you can type: get-help “service” then, you will get all the commands containing “service”, then you can look for which one will may help, then get more info about the command, say, you think “Get Service” may help, then you can type: Get-help get-service, then you will see the full usage of the command. So, then most import thing is not to memorize, but to think and make full use of the help system.
4.	In order to keep your help updated, you can run “update-help –Force”(for v3) command to keep your help updated.
5.	Use Get-help get-service  - Detailed to get detailed usage about the command.
Get-help get-service –examples
Get-help get-service –full
Get-help get-service –online
Get-help get-service –showwindow(for v3)
6.	Choose sth you want to cut and paste, just choose, right click and move to the destination and right click again, then complete.
7.	Get-service -name bits, get-service -name bites, fte; get-service -name b*; get-service -name b*,c*;
Get-service –name bits
Get-service bits
Gsv bits
When work with console, you can use shortcut to work quickly, when with script, better verbose, better explain.
8.	Get used to using “Tab” key all the time you can.
9.	Ctrl+C to cancel a process;
10.	If you use a command which needs a para without a parameter, it will prompt the para to let you fill it, very smart.
11.	Get-help about*, you will get many  “about-xx”  files, these are help files, to explain the usage, very useful
 

2017.11.27 chapter 3-4: pipeline and objects  

12.	Pipeline  
Get-service | export-csv –Path c:\services.csv
Get-service | out-file c:\test.txt
13.	Whenever you are uncertain of what will happen when you operate an action, you can use “what if”, then it will tell you what will happen if you run the command line.
Get-service | stop-service whatif
Get-service | stop-service confirm
Get-service –displayname *bi* | stop-service –whatif
14.	Poweshell are based on objects, when you query for sth, the columns are the properties of the objects, for example:
 
     “handlles” column is one property of  the object “service”. Seems like only applies to v3?  
Where: use as a filter
 
 
    Best practice: Get-stuff | where –somecondition | sort | out-file
15.	How to see all the properties and methods of a powershell object?
With property, we can select or sort.
You can use get-member to look all the properties of an object, and select some kinds useful to you, you can show them, select, or sort , you can output them to csv, xml, html ,very useful.
Get-member or Alias gm 
 
    The following is a very important automation process: 
Output the newest error log to a web page and send it to someone to fix it.
 
 
16.	Use get-history to get all your history command lines.
User start-transcript to log all the command lines you have typed.
2017.11.28  chapter 5-6

17.	Pipeline deeper
By value: means object name
By property name: for example: get-service | stop-process
Get-service return services, but stop-process receive process, it seems like this  pipeline will not work, but , stop-process receive input by propertyName, that means, if get-service output object has name property, and stop-process input object has name property, then they will also hook up, this pipeline will also work.
 
 
18.	Pipeline as input:
A, Get-service | stop-service : by value
B, Get-service | stop-process : by property name
C, If the output property name is the same as the input property name, you can change.
For  example: get-adcomputer output has property computerName, but get-service input need property Name, if not change, this will not match, so need to make some change.
 
     If command does not receive pipeline input, use the command line’s output as the other command’s input,  if type do not match, then try this:
Get-wmiobject does not take any pipeline input, so cannot use pipeline, only use parameter.   
Like the below:  
In V2:
 
In V3, it can be more simple:
 
In V3, The more simple way: the second line  
First run get-adcomputer, then the output will be assigned to $_, then get the .Name property, then assign the .Name property to get-wmiObject as input.Done. Powerful.
 
19.	Compare  property and expandproperty:
 
 
20.	Remoting 
To use powershell remoting ,you have to turn on powershell remoting, for server 2012, it is turned on default.
One-to-one: Enter-possession computerNameYouWantToConnect
 
Mstsc: remote desk command

21.	Invoke-command –computerName xx,yy {command you want to run on the remote computer}
This can be used one-to-many
Process: make a remote connection to the remote computer, load the command , start the powershell on remote computer, run the command , and serialize the result from remote computer to the local computer.
 
22.	Get-computerFeature to get what are and what are not installed on this computer
If there is a cross, mean installed, if not, not installed.
 
If want to install sth, can use install-windowfeature,
 
23.	Powershell web access
This make powershell available by a browser.
For configuration ,you can refer to the video.
2017.11.29: chapter7-9
24.	For a ps1 file, double click will not run.
25.	Set-executionpolicy remotesigned, window server 2012 set execution policy to remotesigned by default.
26.	Variable $var = “hi”, after running a method ,need to refresh. Remember use $ before variable name in order to separate from command line.
27.	Write-warning, write-error, be careful to use write-host
 
28.	Sessions : Invoke-command can let you run command in remote computer, but can not keep powershell up, if you want to keep remote pc powershell up, you should use session.
$session = new-session –computername dc, this will create an all-up session to remote dc ,
 
29.	You can use powershell to remote install  web-server and deploy web sites for many servers at only one try. 
 
 
 
30.	Remote CMDLETs ,Import, can dynamically generate powershell cmdlet. So you can import cmdlets from other computers.
 
31.	Ctrl+R to transfer between ps and psISE.
Powershell  ISE is a very powerful script edit tool.
You can use  Ctrl+J to get some template.
You can use #xxx# to add comment, and these comment will show when you use get-help to check the script you have written.
[cmdletbinding] give some common parameters to the function you write.
 
 
32.	Module : .psm1 , you can create your own module, you can take module as a collection of commands or functions. You can refer to chapter 9 to see how to create your own module and put it in the right path.



## 《powershell in action》notes 

1.	Powershell is a wholey new shell language. It is designed to based on .NET object model.
2.	With OS win7 or server 2008R2, powershell is part of the system. It is placed in start -> All Programs -> Accessories -> windows powershell.
3.	Powershell ISE, you can press F8 to run the part of script you have selected to test.
4.	Always remember to use TAB to complete command, powershell is case insensitive.
5.	You can use > to direct your output to a file, and can use type to see what have been saved.
6.	You can use sort to sort by certain field :  xx | sort –property XX
You can use select to select a subrange of objects piped into it and a subrange of properties to display. Such as ; dir | sort –Descreaing | select –first 1 –property name
You can use sort-select pattern to select certain number of topmost files in a directory.
 
7.	Foreach : foreach cmdlet let you execute a block of statements for each object in the pipe line.
For example: $total = 0 , dir | foreach {$total += $_.length}, $total . to calculate the total length. 
8.	The core of powershell remoting is Invoke-Command, also icm, it can invoke a block of script in the current computer, or in a remote computer, or on a thousand of computers.
 
 
9.	Use Enter-PSSession to create one-to-one connection to a remote server.
10.	Parameter binder, notice the difference between parameter and argument. Parameter usually starts with  - and is used to control the behavior of the command. Argument is the value of the parameter, consumed by parameter. If you use --, then everything after -- will be treated as arguments .
11.	4 catagories of commands in powershell: cmdlet, function, script and native cmd.
12.	There are 2 kinds alias, first is mapped to cmd or unix shell, called transitional alias, such as get-childitem = dir = ls ; second is shortcut of their names, called convenient alias, such as gci = get childitem.
13.	Single quote and double quote: for single, putting a single quote around a series of characters causes them to be treated like a single string; for double quote, variables are expanded, that means the variable name will be replaced by the value of the variable, but with single quote, it will not be replaced , but treated as a string.
  
If you do not want variable to expand, you can use ` (not single quote ‘, but below ESC), and for special character you also need to use `, for example, you want to enter a new line, then you have to use `n.
 
 
The above table can only be used in double quote, in single quote, what you see is what you get.

14.	Use  Format-table or format-list, format-wide, format-custom(will give many many output) to structure the output.
  
15.	Output : by default, out-default, is send to the screen. Anything sent to out-null is discarded.  
Out-file: is sent to a file  
Out-string: formats its input and sends it as a string to the next cmdlet in the pipeline. The main reason of this cmdlet is for interoperation with existing APIs, and external commands that expect to deal with strings.  
Out-gridview: a new window opened with the output displayed with a grid control  
 All out* except out-string do not write anything to the pipeline.
16.	Powershell is type-based , the ability to use objects makes it powerful and different.
 
17.	Hashtable is very useful in powershell, you can use $user.keys, $user.values to fetch data.You can use assignment to add or change a value of hashtable.
18.	Hashtable is reference type, so if you assign $foo to $bar, actually, these two refer to the same object, so if you change $foo, $bar will change  accordingly. If you want to make a copy instead of the reference, you can use clone() method. Such as $bar = $foo.clone().
19.	If the array size is fixed , you cannot add new value to it by assigning value to index larger than athe length, you need to do it by using +.
Note: you didn’t add any elements to the original array after all, instead, you created a new, larger one.
20.	As with hashtables, array assignment is also done by reference. That means, if $a = $b, then either one change will affect the other one. But if you add a new value to $b, then the new $b is a new copy of the old array, not he reference any more, so now ,you change either one, the other one will not be affected.
 
21.	You can use @{} to create an empty array. If you want to define a one-element array, you cannot use $a=1, this will create a Int32 not an array, you should use $a =  ,1, then $a will be an object, or an array.
Chapter3.5 
Operators always can divide to 3 : numbers, strings, arrays.
22.	Arithmetic operators apply “left-hand” rule: the type of the left-hand operand determines the type of the overall operation. Such as : 2+”123” result: 125; “2” + 123 ,result : 2123
23.	When involves arrays or collections, the left is array or collection ,the operand on the right will be appended to that collection.  .Net array objects are of fixed size, + is accomplished by creating a new array of type object() and copying the elements from operands into this new array.
 
24.	For *, the only legal right-hand operand for * is a number. If the left is a string, then the result will repeat right-operand times.
 
25.	If you want to swap a and b, in powershell ,you can use $a,$b = $b,$a, very simple.
26.	For comparison operators: -eq, -gt, ect. For case-sensitivity issue: -eq have two: -ceq, -ieq, -ceq means case sensitive, -ieq means case-insensitive. By default, it is case insensitive. This applies for other comparison operators.
27.	As with assignment operators, the behavior of comparison operators is affected by the type of the left operand.
 
28.	Comparison operators with collections: if the left operand is an array or collection, the comparison operation will return the elements of that collection that math the right operand. All the comparison operators return the matching elements from the collection.
2 and “02” compare equally in a numeric context, “2” and “02” are different in a string context.
 
Chapter 4.4

