

## IIS，Apache, Tomcat

**共同点**：  
都可以直接用作Web服务器

**区别**：  
iis和apache二者只能作web服务器，而tomcat除做web服务器外，还可以作应用服务器。
所谓应用服务器(App Server)，这里主要是为Java EE的Web应用提供一个运行的容器。

- Apache

免费，世界使用排名第一的Web服务器。它可以运行在几乎所有广泛使用的计算机平台上。Apache的特点是简单、速度快、性能稳定，并可做代理服务器来使用。它的成功之处主要在于它的源代码开放、有一支开放的开发队伍、支持跨平台的应用（可以运行在几乎所有的Unix、Windows、Linux系统平台上）以及它的可移植性等方面

在Web服务器中，Apache是纯粹的Web服务器，经常与Tomcat配对使用。它对HTML页面具有强大的解释能力，但是不能解释嵌入页面内的服务器端脚本代码。 

- Tomcat 
 
免费，一个免费的开放源代码的Web应用服务器。由于有Sun 的参与和支持，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现。因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web应用服务器。

早期的Tomcat是一个嵌入Apache内的JSP/Servlet解释引擎Apache+Tomcat就相当于IIS+ASP。后来的Tomcat已不再嵌入Apache内，Tomcat进程独立于Apache进程运行。 而且，Tomcat已经是一个独立的Servlet和JSP容器，业务逻辑层代码和界面交互层代码可以分离了。因此，有人把Tomcat叫做轻量级应用服务器。 

- IIS 
 
IIS是微软的Internet Information Server的简称。主要是用来提供Web服务的，当然是针对它自己的产品asp的。  
微软早期的IIS，就是一个纯粹的Web服务器。后来，它嵌入了ASP引擎，可以解释VBScript和JScript服务器端代码，这时，它就可以兼作应用服务器。当然，它与J2EE应用服务器根本无法相比，但是，从功能上说，从原理上说，它勉强可以称之为应用服务器。确切地说，它是兼有一点应用服务器功能的Web服务器。 


综上：  
Apache是纯粹的web服务器，而Tomcat和IIS因为具有了解释执行服务器端代码的能力，可以称作为轻量级应用服务器或带有服务器功能的Web服务器。  
对于处于中间位置的Tomcat，它可以配合纯Web服务器Apache一起使用，也可以作为应用服务器的辅助与应用服务器一起部署。   
Weblogic、WebSphere因为能提供强大的J2EE功能，毫无疑问是绝对的应用服务器。 

## Tomcat与apache 

Tomcat提供一个支持Servlet和JSP运行的容器。Servlet和JSP能根据实时需要，产生动态网页内容。而对于Web服务器来说，Apache仅仅支持静态网页，对于支持动态网页就会显得无能为力；Tomcat则既能为动态网页服务，同时也能为静态网页提供支持。尽管它没有通常的Web服务器快、功能也不如Web服务器丰富，但是Tomcat逐渐为支持静态内容不断扩充。大多数的Web服务器都是用底层语言编写如C，利用了相应平台的特征，因此用纯Java编写的Tomcat执行速度不可能与它们相提并论。 

一般来说，大的站点都是将Tomcat与Apache的结合，Apache负责接受所有来自客户端的HTTP请求，然后将Servlets和JSP的请求转发给Tomcat来处理。Tomcat完成处理后，将响应传回给Apache，最后Apache将响应返回给客户端。 

而且为了提高性能，可以一台apache连接多台tomcat实现负载平衡。 

如果客户端请求的是静态页面，则只需要Apache服务器响应请求  
如果客户端请求动态页面，则是Tomcat服务器响应请求  
换句话说，apache是一辆卡车，上面可以装一些东西如html等。但是不能装水，要装水必须要有容器（桶），而这个桶也可以不放在卡车上。 


## web服务器 && 应用服务器 

严格意义上Web服务器只负责处理HTTP协议，只能发送静态页面的内容。
而JSP，ASP，PHP等动态内容需要通过CGI、FastCGI、ISAPI等接口交给其他程序去处理。这个其他程序就是应用服务器。

应用服务器一般也支持HTTP协议，因此界限没这么清晰。但是应用服务器的HTTP协议部分仅仅是支持，一般不会做特别优化，所以很少有见Tomcat直接暴露给外面，而是和Nginx、Apache等配合，只让Tomcat处理JSP和Servlet部分。


Web服务器的基本功能就是提供Web信息浏览服务。它只需支持HTTP协议、HTML文档格式及URL。与客户端的网络浏览器配合。因为Web服务器主要支持的协议就是HTTP，所以通常情况下HTTP服务器和WEB服务器是相等的(有没有支持除HTTP之外的协议的web服务器，作者没有考证过)，说的是一回事。 


通俗的讲，Web服务器传送页面使浏览器可以浏览，应用程序服务器提供的是客户端应用程序可以调用(call)的方法(methods)。确切一点，你可以说:Web服务器专门处理HTTP请求(request)，但是应用程序服务器是通过很多协议来为应用程序提供商业逻辑。 

以Java EE为例，Web服务器主要是处理静态页面处理和作为Servlet容器，解释和执行servlet/JSP，而应用服务器是运行业务逻辑的，主要是EJB、 JNDI和JMX API等J2EE API方面的，还包含事务处理、数据库连接等功能，所以在企业级应用中，应用服务器提供的功能比WEB服务器强大的多。 

以这样的定义，IIS、Apache、Tomcat都可以属于Web服务器，Weblogic、WebSphere都属于应用服务器。 


**关于WEB服务器、应用程序服务器的更详细区别**

- Web服务器(Web Server)

　　Web服务器可以解析(handles)HTTP协议。当Web服务器接收到一个HTTP请求(request)，会返回一个HTTP响应 (response)，例如送回一个HTML页面。为了处理一个请求(request)，Web服务器可以响应(response)一个静态页面或图片，进行页面跳转(redirect)，或者把动态响应(dynamic response)的产生委托(delegate)给一些其它的程序例如CGI脚本，JSP(JavaServer Pages)脚本，servlets，ASP(Active Server Pages)脚本，服务器端(server-side)JavaScript，或者一些其它的服务器端(server-side)技术。无论它们(译者注:脚本)的目的如何，这些服务器端(server-side)的程序通常产生一个HTML的响应(response)来让浏览器可以浏览。 

　　要知道，Web服务器的代理模型(delegation model)非常简单。当一个请求(request)被送到Web服务器里来时，它只单纯的把请求(request)传递给可以很好的处理请求 (request)的程序(服务器端脚本)。Web服务器仅仅提供一个可以执行服务器端(server-side)程序和返回(程序所产生的)响应(response)的环境，而不会超出职能范围。服务器端(server-side)程序通常具有事务处理(transaction processing)，数据库连接(database connectivity)和消息(messaging)等功能。 

- 应用程序服务器(The Application Server)

　　作为应用程序服务器，它通过各种协议，可以包括HTTP，把商业逻辑暴露给(expose)客户端应用程序。应用程序服务器提供访问商业逻辑的途径以供客户端应用程序使用。应用程序使用此商业逻辑就象你调用对象的一个方法 (或过程语言中的一个函数)一样。 

　　应用程序服务器的客户端(包含有图形用户界面GUI)可能会运行在一台PC、一个Web服务器或者甚至是其它的应用程序服务器上。在应用程序服务器与其客户端之间来回穿梭(traveling)的信息不仅仅局限于简单的显示标记。相反，这种信息就是程序逻辑(program logic)。正是由于这种逻辑取得了(takes)数据和方法调用(calls)的形式而不是静态HTML，所以客户端才可以随心所欲的使用这种被暴露的商业逻辑。 

　　在大多数情形下，应用程序服务器是通过组件 (component) 的应用程序接口(API)把商业逻辑暴露(expose)给客户端应用程序的，例如基于J2EE(Java 2 Platform, Enterprise Edition)应用程序服务器的EJB(Enterprise JavaBean)组件模型。此外，应用程序服务器可以管理自己的资源，例如看大门的工作(gate-keeping duties)包括安全(security)，事务处理(transaction processing)，资源池(resource pooling)，和消息(messaging)。
