# HTTP报文结构

1. 起始行
2. 首部

 首部包括：通用首部、请求首部/响应首部、实体首部、扩展首部

 * 通用首部

|首部|描述|
|-------    |--------------|
| Date      	 |构建报文的时间和日期
|Connection 	 |客户端和
|Transfer-Encoding|规定了传输报文主体时采用的编码方式
|Via 			|追踪客户端与服务器之间的请求响应和响应报文的传输途径。还可以避免请求回环的发生。
|Trailer		|说明在报文主体后记录了哪些首部字段，可以应用在HTTP1.1版本分块传输编码时使用。
|Cache-Control	|操作缓存的工作机制。
|pragma 		|报文指令。与http1.1之前的版本兼容

 * 请求首部

|首部|描述|
|-------    |--------------|
User-Agent	|产生请求的浏览器类型。
Accept		|客户端可识别的内容类型列表。
Host		|请求的主机名，允许多个域名同处一个IP地址，即虚拟主机。
Referer		|包含当前请求URI的文档的URL

 * Accept首部

|首部|描述|
|-------    	|--------------|
Accept 			|可处理的媒体类型及媒体类型的相对优先级
Accept-Charset	|支持的字符集及字符集的相对优先级
Accept-Encoding	|支持的内容编码及内容编码优先级顺序
Accept-Lanuage	|用户代理能够处理的自然语言集

 * 实体首部

  其中实体又包括实体首部和实体主体。

  参考[实体和编码](http://www.cnblogs.com/pzk7788/p/6881709.html)


3. 主体（实体主体或报文主体）

 可以包含文本和二进制，也可以为空


## request报文结构

1. 首行是Request-Line包括：请求方法，请求URI，协议版本，CRLF
2. 首行之后是若干行请求头，包括general-header，request-header或者entity-header，每个一行以CRLF结束
3. 请求头和消息实体之间有一个CRLF分隔
4. 根据实际请求需要可能包含一个消息实体

        GET /Protocols/rfc2616/rfc2616-sec5.html HTTP/1.1
        Host: www.w3.org
        Connection: keep-alive
        Cache-Control: max-age=0
        Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
        User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
        Referer: https://www.google.com.hk/
        Accept-Encoding: gzip,deflate,sdch
        Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
        Cookie: authorstyle=yes
        If-None-Match: "2cc8-3e3073913b100"
        If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT

        name=qiu&age=25

## response报文结构

1. 首行是状态行包括：HTTP版本，状态码，状态描述，后面跟一个CRLF
2. 首行之后是若干行响应头，包括：通用头部，响应头部，实体头部
3. 响应头部和响应实体之间用一个CRLF空行分隔
4. 最后是一个可能的消息实体

        HTTP/1.1 200 OK
        Date: Tue, 08 Jul 2014 05:28:43 GMT
        Server: Apache/2
        Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
        ETag: "40d7-3e3073913b100"
        Accept-Ranges: bytes
        Content-Length: 16599
        Cache-Control: max-age=21600
        Expires: Tue, 08 Jul 2014 11:28:43 GMT
        P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
        Content-Type: text/html; charset=iso-8859-1

        {"name": "qiu", "age": 25}


## 请求方法
1. GET 请求一份文档
2. POST 向服务器发送需要处理的处理
3. PUT 将请求的主体部分存储在服务器上

 让服务器用请求的主体创建一个由所请求的URL命名的新文档

4. DELETE 请求删除一份文档
    客户端无法保证删除一定执行，因为HTTP规范允许服务器不通知客户端的情况下撤销请求。

5. OPTIONS 请求服务器告知其支持的各种功能

 服务器支持的方法、或对默写特殊资源支持哪些方法
6. HEAD

 与GET方法类似，但是服务器只在响应中返回首部，不会返回实体的主体部分。
 * 不获取资源的情况下了解资源的情况，比如资源类型
 * 查看状态码，了解对象是否存在
 * 查看首部，测试资源是否被修改

7. TRACE  对传到服务器上的报文，可能经过的代理的追踪

一个请求可能经过防火墙、代理及网关或其他应用程序，这些节点可能会修改原始HTTP请求。
TRACE请求，目的服务器会弹回一条trace响应，并在主体中携带收到的原始请求报文，供客户端查看是否被修改及毁坏过。

并不是所有服务器都支持这些请求方法，服务器也可以实现自己的请求方法。

## 状态码

1. 1XX

2. 2XX
 * 200 请求成功

3. 3XX：重定向
 * 301 Moved Permanently
 * 302 Found： HTTP1.0的重定向，接受Location首部的重定向URL
 * 303 See Other
 * 304 Not Modified： 发起if-modified-since请求，未改变返回304
 * 307 Temporary Redirect

4. 4XX: 客户端错误
 * 401  Unauthorized: 输入用户名和密码
 * 403  Forbidden: 请求被拒绝，一般不想说明原因
 * 404  not found
 * 405  Method not Allowed: 请求方法不被允许，响应中会包含Allow首部，说明可以请求的方法
 * 408  Request timeout

5. 5XX: 服务端错误
 * 500 Internal Server Error:
 * 501 Not Implemented:
 * 502 Bad Gateway:
 * 503 Service Unavailable:
 * 504 Gateway Timeout:
 * 505 HTTP Version Not Supported:



