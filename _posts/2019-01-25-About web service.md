
## 参考链接：
 https://zhuanlan.zhihu.com/p/27475153


## web service 理解

概念：WebService是为跨平台访问提供的一种通信机制。平台可以理解为不同的业务系统（不是操作系统）。
简单地说, 也就是服务器如何向客户端提供服务。

WebService就像一个服务提供商，SOAP就像双方签的合同，约束双方按规律和标准来办事。WSDL就像一个说明书，你能给别人提供说明服务。UUDI就像是公司在工商局注册，说明公司经营什么内容，方便别人查询。



## 相关术语

1）XML：扩展型标记语言

2）SOAP：简单对象访问协议，是一个xml调用规范，它支持http,https,smtp通信协议。

3）WSDL：web描述性语言。wsdl文件是一个xml文档。用于说明一组SOAP消息以及如何交换这些消息。

4）UDDI：通用描述、发现与集成服务。

## WebService特点：

1）自包含的。意思是客户端不需要包含任何附加的软件，只要客户端支持Http和XML。

2）自我描述的。客户端和服务端除了消息的格式和内容外不需要知道任务其他数据。

3）夸平台和夸语言。无论是java，c++都可以通过通信。

4）是基于开发和标准的。xml和http是WebService的主要技术，而xml和http早就成为行业内的标准。

5）可以组合的。可以通过各种WebService服务组合成负责的系统。

6）松散耦合的。像一些分布式中间件DCOM等，客户端和服务端是紧耦合的，当服务端挂后，客户端会崩溃。

7）提供编程访问的能力。这个好理解，只要是接口一定是提供了编程访问能力。

8）通过网络进行发布，查找和使用。

## WebService发布方式

1)JWS发布

2）Axis2发布

3）CXF发布

4）ksoap2-andriod发布。


## web service 使用
webService的常用的方法有:  
1.RPC（远程过程调用协议 ）所谓的远程过程调用 (面向方法)  

2.SOAP（简单对象访问协议） 所谓的面向服务的架构(面向消息)

简单对象访问协议（Simple Object Access Protocol）是一种标准化的通讯规范，主要用于Web服务（web service）中。  
用一个简单的例子来说明 SOAP 使用过程，一个 SOAP 消息可以发送到一个具有 Web Service 功能的 Web 站点，例如，一个含有房价信息的数据库，消息的参数中标明这是一个查询消息，此站点将返回一个 XML 格式的信息，其中包含了查询结果（价格，位置，特点，或者其他信息）。由于数据是用一种标准化的可分析的结构来传递的，所以可以直接被第三方站点所利用。

3.REST（表象化状态转变）Representational state transfer (面向资源)

表征状态转移（Representational State Transfer），采用Web 服务使用标准的 HTTP 方法 (GET/PUT/POST/DELETE) 将所有 Web 系统的服务抽象为资源，REST从资源的角度来观察整个网络，分布在各处的资源由URI确定，而客户端的应用通过URI来获取资源的表征。Http协议所抽象的get,post,put,delete就好比数据库中最基本的增删改查，而互联网上的各种资源就好比数据库中的记录（可能这么比喻不是很好），对于各种资源的操作最后总是能抽象成为这四种基本操作，在定义了定位资源的规则以后，对于资源的操作通过标准的Http协议就可以实现，开发者也会受益于这种轻量级的协议。

REST是一种软件架构风格而非协议也非规范，是一种针对网络应用的开发方式，可以降低开发的复杂性，提高系统的可伸缩性。RESTful 为一种架构设计风格，提供设计原则和约束条件，符合这种风格的应用程序或设计就是RESTful服务或架构。