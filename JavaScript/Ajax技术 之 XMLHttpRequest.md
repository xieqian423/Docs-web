## ajax原理
 Asynchronous JavaScript + XML，能够以异步方式从服务器取得更多信息，意味着用户单击后，可以不必刷新页面也能取得新数据。
也就是说，可以使用 XHR 对象取得新数据，然后再通过 DOM 将新数据插入到页面中。
ajax技术的核心是XMLHttpRequest

### XMLHttpRequest

#### 属性
 1. **readyState**  表示XMLHttpRequest对象的状态：

    	0   未初始化，对象已创建，未调用open；
    	1   启动，open方法成功调用，但send方法未调用；
    	2   发送，send方法已经调用，尚未接收到数据；
    	3   接收，正在接受数据。Http响应头信息已经接受，但尚未接收完成；
    	4   完成，即响应数据接收完成，数据可以在客户端使用。

 2. **onreadystatechange**

	请求状态改变的事件触发器（readyState值变化时会触发这个事件）。

 3. **responseText**	服务器响应的文本内容
 4. **responseXML**		服务器响应的XML内容,可以解析为DOM对象
 5. **Status**		服务器返回的http状态码。

	200表示“成功”，404表示“未找到”，500表示“服务器内部错误”等。

 6. **statusText**	服务器返回状态的文本信息（如，OK，No Content）。

#### 方法：

 1. open(method, url, asynchronous [, user, password]):初始化准备发送到服务器上的请求。method是HTTP方法，不区分大小写；url是请求发送的相对或绝对URL；asynchronous表示请求是否异步；user和password提供身份验证

 2. setRequestHeader(name, value):设置HTTP报头

 3. send(body):对服务器请求进行初始化。参数body包含请求的主体部分，对于POST请求为键值对字符串；对于GET请求，为null

 4. getResponseHeader(headerName):返回headName对应的报头值

 5. getAllResponseHeaders(): 返回一个字符串，包含响应中服务器发送的全部HTTP报头。每个报头都是一个用冒号分隔开的名/值对，并且使用一个回车/换行来分隔报头行
 
 6.abort():取消异步HTTP请求


 **注意**
  1. 必须在调用 open() 之前指定 onreadystatechange事件处理程序才能确保跨浏览器兼容性
  2. setRequestHeader() 方法可以设置自定义的请求头部信息。要成功发送请求头部信息，必须在open()与send()方法之间调用 setRequestHeader() ，不要使用浏览器正常发送的字段名称，否则有可能会影响服务器的响应。有的浏览器允许开发人员重写默认的头部信息，但有的不允许。


http://blog.csdn.net/liujiahan629629/article/details/17126727


### get post区别

#### GET 
 1. 一般用于获取/查询资源信，
 2. 请求的数据会附在URL之后。
 3. 提交的数据最多只能是1024字节。GET是通过URL提交数据，HTTP协议规范没有对URL长度进行限制。这个限制是特定的浏览器及服务器对它的限制。

#### POST 
 1. 表示可能修改变服务器上的资源的请求。
 2. 把提交的数据则放置在是HTTP包的包体中进行传输。
 3. POST是没有大小限制的，HTTP协议规范也没有进行大小限制

#### POST的安全性要比GET的安全性高。
  通过GET提交数据，用户名和密码将明文出现在URL上：

  （1）页面有可能被浏览器缓存，其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了

  （2）用GET提交数据还可能会造成Cross-site request forgery攻击。


### 请求头和响应头

    Accept ：浏览器能够处理的内容类型。
    Accept-Charset ：浏览器能够显示的字符集。
    Accept-Encoding ：浏览器能够处理的压缩编码。
    Accept-Language ：浏览器当前设置的语言。
    Connection ：浏览器与服务器之间连接的类型。
    Cookie ：当前页面设置的任何 Cookie。
    Host ：发出请求的页面所在的域 。
    Referer ：发出请求的页面的 URI。注意，HTTP 规范将这个头部字段拼写错了，而为保证与规范一致，也只能将错就错了。 （这个英文单词的正确拼法应该是 referrer。 ）
    User-Agent ：浏览器的用户代理字符串。

以上信息是各浏览器基本都会发送的请求头。








