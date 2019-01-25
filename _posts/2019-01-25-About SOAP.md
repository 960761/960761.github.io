
## SOAP理解
简单对象访问协议（Simple Object Access Protocol）是一种标准化的通讯规范，主要用于Web服务（web service）中。

用一个简单的例子来说明 SOAP 使用过程，一个 SOAP 消息可以发送到一个具有 Web Service 功能的 Web 站点，例如，一个含有房价信息的数据库，消息的参数中标明这是一个查询消息，此站点将返回一个 XML 格式的信息，其中包含了查询结果（价格，位置，特点，或者其他信息）。由于数据是用一种标准化的可分析的结构来传递的，所以可以直接被第三方站点所利用。

## 学习连接
xml SOAP简介：https://www.w3schools.com/xml/xml_soap.asp  

SOAP 官网：http://www.soapuser.com/basics1.html

## 工作流程
SOAP 中有client 和 server两部分，server即SOAP server，  
SOAP server 用来接收 SOAP requests，返回send SOAP responses.  

从SOAP server得到的response为XML 格式的，大多数时候我们不需要XML格式，因此，需要用某种方式将XML格式的数据进行解析并展示出来，这种解析并展示的 工作并不属于SOAP server的工作。

一般流程为，soap server被另外的another server使用，another server发送一个soap request 给soap server, 然后another server会解析soap response，通常，another server指http server。

示例：

我想获取城市a明天的天气，我打开forcast.com网站并输入城市名，forcast.com本身并没有存储相关内容，于是它向一个soap server发送soap message，soap server就会返回包含所需天气信息的soap response，forcast.com接受到之后对response进行处理，获取自己需要的信息展示给“我”。
这里的“我”就是 client， forcast.com就是“another server”。

## 使用javascript发送soap

soap message基本格式：

~~~
<?xml version="1.0"?>

<soap:Envelope
 xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"
 soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">

<soap:Header>
 ...
</soap:Header>

<soap:Body>
 ...
   <soap:Fault>
   ...
   </soap:Fault>
</soap:Body>

</soap:Envelope> 
~~~

使用js构建soap
~~~
<html>
<head>
    <title>SOAP JavaScript Client Test</title>
    <script type="text/javascript">
        function soap() {
            var xmlhttp = new XMLHttpRequest();
            xmlhttp.open('POST', 'https://somesoapurl.com/', true);
            // build SOAP request
            var sr =
                '<?xml version="1.0" encoding="utf-8"?>' +
                '<soapenv:Envelope ' + 
                    'xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ' +
                    'xmlns:api="http://127.0.0.1/Integrics/Enswitch/API" ' +
                    'xmlns:xsd="http://www.w3.org/2001/XMLSchema" ' +
                    'xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">' +
                    '<soapenv:Body>' +
                        '<api:some_api_call soapenv:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">' +
                            '<username xsi:type="xsd:string">login_username</username>' +
                            '<password xsi:type="xsd:string">password</password>' +
                        '</api:some_api_call>' +
                    '</soapenv:Body>' +
                '</soapenv:Envelope>';
            xmlhttp.onreadystatechange = function () {
                if (xmlhttp.readyState == 4) {
                    if (xmlhttp.status == 200) {
                        alert('done. use firebug/console to see network response');
                    }
                }
            }
            // Send the POST request
            xmlhttp.setRequestHeader('Content-Type', 'text/xml');
            xmlhttp.send(sr);
            // send request
            // ...
        }
    </script>
</head>
<body>
    <form name="Demo" action="" method="post">
        <div>
            <input type="button" value="Soap" onclick="soap();" />
        </div>
    </form>
</body>
</html> 

~~~
