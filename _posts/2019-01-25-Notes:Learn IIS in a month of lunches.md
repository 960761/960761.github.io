Notes-learn IIS in a month of lunches

book: << learn windows iis in a month of lunches >>

**2018.3.7 day 1: chapter2, how to install IIS**

1. Have the firewall protection in place before deploy IIS is very important. Figure2.4
 
2. 安装IIS 有很多种途径，比较常用的有GUI and powershell.   
Microsoft将additional software分为roles and features, roles are services that affect the entire network, such as AD, features only impact the server, such as clustering. IIS has network impact, so it is a role.   
可以在server manager中查看有哪些roles and features安装了，有哪些可以选择安装。 IIS 是被当成role，所以安装时是在server manager中add  the role（web server(IIS)）即可。选择add role之后接下来会有a list service，你可以选择添加哪些，其中默认选择的是IIS正常运行所需的最小配置。
3. powershell 中, modules hold a collection of cmdlets that can be loaded and unloaded as need。Powershell中的Module也就是命令集，可以根据需要进行加载，可以使用Get-Module –ListAvailable来查看有哪些可以用的module。在使用之前要import，Import-Module moduleName
可以使用server manager module里面的 cmdlet : Add-WindowsFeature Web-server来安装IIS。你还可以使用这个命令来安装新的roles, features or services。
4. 安装IIS后，可以在本地computer browser上面使用http://localhost来检查是否安装成功，或者在其他机器上使用http://servername  or http://ip-address  实际操作中是通过查看http://localhost:8000/ 来检验的。  
   
通过查看log来发现问题，Get-EventLog –LogName System –Source IIS* -EntryType Error可以只查看IIS相关的error问题。  
也可以使用 Get-EventLog –LogName System –Source IIS* -EntryType Error | Export-Csv c:\IISErrors.csv来导出为csv文件。


**Day2: chapter3, the instruction of IIS manager and how to build website structure。**

5. In most cases, a web server is like a file server serving web pages like files to a network client across the internet, the client uses a browser to display and run those files like a useful application.   
在IIS里，每个web site包括两部分内容：一部分称为 website容器，用来存放在IIS manager里面设置的相关的配置值；另一部分则是保存在filesystem上面的网页本身。也即web site container + web page。分清几个概念： web server, web site , web page, web application.

6. IIS管理工具主要的有两个： IIS manager GUI 和 WebAdministration module for powershell。  
在IIS manager里面看到的只是web site configuration(container)，不是web page本身。使用powershell时，首先导入相关module。Import-Module WebAdministration， 可以使用Get-Command –Module WebAdministration 来获取可用的命令。IIS相关的命令名词部分都有web，可以用这个特点进行查找。
7. 作为IIS admin，主要任务是create the website container, open access to the outside world , test for performance and security。  
Web pages是一些files，存放在file system。  
有两种方法可以获取web page file，一种是直接定位到对应路径，default  web site默认路径为c:\inetpub，IIS安装时自动生成的；一种则是点击右边pane上面的explorer。第二种方式非常有用，因为有时候你不一定清楚web page路径是在哪里的。因为只有default web sites是在c:\inetpub\里的，另外添加的非default web site的web page所在的位置可以自己设定。Wwwroot里面保存有default web site的web pages，除此外还有一些error pages, logs等。  
最好不要修改default web page，这是一个很标准的测试工具，自定义的网页不能正常显示时，这个也应该是正常显示的。
8. The ASP and ASP.NET components provide the ability for a web page to display information about the website or web application that is hosting the page.   
ASP模块可以在web page中动态显示所在的web site, web server, application的信息。这些信息都是从 server variables中提取出来的。Server variables包含了有关web server的各种信息，可以查看也可以显示在web page上面，完整的server variables可以在这里看到：https://msdn.microsoft.com/en-us/library/ms524602(v=vs.90).aspx

创建一个page, 如果安装的是ASP，则命名为xx.asp，如果是ASP.NET，则命名为xx.aspx，在这个上面动态获取web server , site , application等相关信息，这个文件可以拷贝到人很一个web site里面，上面的内容会动态改变，非常方便实用。  

具体代码可以查看3420/c:/inetput/wwwroot/Testpage.aspx。大体如下：
~~~
<html>
Server Information<br>
------------------<br>
Server Name = <%= Request.ServerVariables("SERVER_NAME")%><br>
Server Protocol = <%= Request.ServerVariables("SERVER_PROTOCOL")%><br>
Server IP = <%= Request.ServerVariables("LOCAL_ADDR")%><br>
Server Port = <%= Request.ServerVariables("SERVER_PORT")%><br>
IIS Version = <%= Request.ServerVariables("SERVER_SOFTWARE")%><br>
<br>
Website Information<br>
-------------------<br>
Application Physical Path = <%= Request.ServerVariables("APPL_PHYSICAL_PATH")%> <br> #C
Application Path = <%= Request.ServerVariables("PATH_INFO")%><br>
Application Translated Path = <%= Request.ServerVariables("PATH_TRANSLATED")%><br>
<br>
</html>
~~~
 
9. 每个website and application 都有一个自己的default document list，在这个list里面的web page会被自动加载，所以放在default web site 里面的default.htm才会在request http:/localhost时自动加载。  
对于特定name and extensions的document(files)，IIS可以自动进行加载。这些自动加载的document就是位于default document list里面的。在中间pane中的IIS下面，点击default document，就可以看到所有自动加载的file，你可以将自己想要自动加载的file放到里面。只有放到最上面的那个file才会被自动加载。

10. 虽然需求的增加，web site的文件结构也会变得复杂，为了方便访问，file system也应该有一个清晰的布局。  
一般会包括用户下载的文件，图片文件，及新安装的web application文件，所以三种常用的folder包括： normal folder, virtual directory, web application folder。 

Normal folder，  
就是最普通的folder，和普通的创建方法一样，没有任何special IIS feature，所以很少用到；放在这种folder里面的files可以被任何browser访问，因此可以将downloadable content 放到这里，也可以将web pages放到这里，这种folder只允许local storage on the web server。创建时只需在相应的地方新建folder即可，可平常创建一样。

Virtual directories:  
Virtual folder可以有alias(a shortcut name) that will become part of URL。比如你可以创建一个virtual directory命名为this is where I store files，然后将alias命名为myFile，当访问的时候，URL就可以写成xxxx/myFile。创建时，右击web site> add virtual directory，填好alias and physical path即可，physical path即这个file的物理保存位置，可以是local path or net work path，要注意的是，virtual directory类似于一个映射，所以需要提前在physical path上面创建出来一个folder用于对应这个新建的virtual folder。它的用处就在于可以位于network share on another server，这对于multiple server  deployment非常有用。  

Web application folder:  
A web application is a collection of files that contain HTML, ASP, ASP.NET, or other code that a browser can execute。也即, web application本质上是一组文件的集合，在IIS里要通过将它们加到 application pools来区别对待。之所以将application 放到 application pool是为了防止其他application或IIS本身对它带来破坏。Application folder也可以有alias，类似于virtual directory，创建时和virtual folder很相似，比之多了一个application pool，其余都相同。因为涉及到不同的application pool，所以在连接这个folder 里面的内容时需要进行身份验证。

11. 若要允许外界访问某个web site，需要进行设置，大致为三个设置：首先开启防火墙端口；其次获取一个outside public IP address，最后配置DNS。公司服务器的防火墙是不可以自己配置的，所以这一块内容无法实践。  
开启防火墙比较好理解，获取公共IP  address，一般公司内部的IP地址是只适用于内部的，所以要想被外界访问，需要申请一个public address，理解为申请一个地址，之后就要将想让被访问的内容放到这个地址里面，也即将申请到的 IP 地址通过DNS和你的web server对应起来。  

**Day3: chapter4, manage application pool(create and recycle)
           Chapter 5, add more websites to servers**


12. Application pools provide isolation to each website on a server.

13. 新创建application pool时需要提供name, .Net work version , pipeline mode.   
Name最好是和其中host 的application/website的名字相同，便于跟踪，最好不要使用特殊字符；大部分application 都需要.Net work支持，但也不是全部，每一个application pool只能支持一种版本的.net，所以放在同一个application pool 里面的application一定要使用相同的.net work version, 至于具体哪个版本要参考application的说明，v2.0 for 2.0,3.0,3.5; v4.0 for v4.0 and above。  
Pipeline mode是针对IIS7及以上的版本的，用于加速ASP.NET application，一般选择integrated，如果有IIS6 application 则需要选择classic mode。使用powershell创建时不能只需name，.net framework version, mode需要另外的命令配置。

14. 当一些application进行了一些更新需要进行一些测试时，就可以创建一个app pool，然后将要测试的application 移到这些pool内进行测试，正常运行时再投入使用。移动时，先选中要移动的web site，点击右侧edit site> basic setting > select，选择目标application pool即可。

15. application pool recycling: reset themselves, cleaning up the fragmented memory and stalled processes of a misbehaving application and then restarting the application in a clean environment.   
Application pool recycle发生有三种情况，第一，手动on-demand；第二，IIS config改变的时候会recycle；第三，当监测到有unhealthy website or application时会自动recycle。

16. 每个application pool都有自己的worker process，可以把worker process想象成一个waiter in a restaurant，它负责接收request，将这个request递交给application，从application中收集response回馈给你。  
Open a web site，你会看到这个web site 所在的application pool work process 在task manager中，在recycle an application pool的时候，你会从task manager中看到，在old work process 结束之前会生成一个新的work process，也即new and old work process will overlap for a while。当一个application pool become slow or stop responding，可以尝试使用recycle可否解决问题。

17. 选中application pool，点击右边的recycling，可以对recycle进行设置，包括自动recycle的频率，次数，时间，log内容等都可以自定义。也可以使用powershell进行设置，稍微复杂一些，多用于批量配置，但是用来查看recycle event log非常方便：  
Get-Eventlog –LogName System –Source WAS
18. websites  include web application 尽量多的创建web site，而不是一个web site里面创建多个web  application，原因是，这样每个application 可以有自己的URL，URL是对应于website的。  
右击sites 或者右边action>add new website，第一部分，你需要，site name, application pool, web page physical location. 注意的是，这里的site name并非用来生成URL的，是为了organizational purpose。  Application pool会自动根据你的site name而定，同时创建同名的app pool。你也可以使用select选择一个不同的而非自动为你生成的app pool。Web page physical location是你存放application file的地方，比如default web site存放在c:\inetput\wwwroot。这个location or path也可以是network share location，这样可以用来部署到multiple web servers。

19. create a new website的第二部分，website must have different/unique names so that servers know where to send requests。  
这里的unique name被称为binding。 A protocol binding is a set of communication rules that define a path between two computers so they can communicate。如同人的名字由first, middle, last name组成，web site unique name(binding)由四部分组成，分别为：type, IP address, port, host name。  

20. Type: 又称为protocol，browser use to access it。通常为http or https, 有些还有FTP，WCF。    
IP address，你可以使用一个实际存在的或者一个虚拟的IP address来命名web site；  
Port: 通过端口来标识网站不推荐，首先URL不好记，其次需要开发firewall port；  
Host name: 涉及到DNS，  
目前看到的3420上面的web site都是通过port来区分的，没有一个是通过host name的，在完成task时，利用host name时打开网页失败，目前不知道原因在哪里，是操作错误还是domain的原因，或者公司政策问题，按照指示操作打开后出现以下提示：

This page cannot be displayed. Based on your corporate access policy, xxxxxx .
 
好像只要是使用host name的都需要DNS进行配置，但配置过后都是打不开的，提示如上。

**Day4: chapter 6: about web applications, chapter 7: security web sites and web application**

21. IIS use a group of XML files to store everything but the web pages for an application. 不建议直接修改XML文件来改配置，即便真要直接修改也一定要备份。  
Compression： 中间一列IIS> compression，有static, dynamic两种，前一种是默认打开的，后一种只有安装之后才可开启，dynamic 压缩会降低计算机性能。  
Default document:  前面已经介绍过。  
Directory Browsing : determines whether a consumer using a browser can see  the files inside a website, application, or virtual directory, installed but not enabled by default。  
WebPI(Web Platform installer) is a free download from Microsoft to install additional application support.可以使用它方便地下载安装配置。
22. 对于IIS6及以下版本的application需要使用 Compatibility Mode，但是有些old application即使使用这种MODE也是无法正常运行的，这种情况下只能在IIS下运行。
23. 有五种authentication 方式，其中有三种比较常用，分别为： anonymous access, windows authentication, basic authentication。
 
24. anonymous access被映射为了 built-in user account IUSR and the group account IIS_IUSRS。  
Windows authentication is the best method for internal websites that contain confidential or personalized information, such  as sharepoint。  
Basic authentication allows users from the internet to access private and confidential content by providing a valid username and password.

