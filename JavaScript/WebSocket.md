# WebSocket

## WebSocket通信机制
在一个单独持久连接上提供全双工、双向通信。
标准的 HTTP 服务器无法实现 Web Sockets，只有支持这种协议的专门服务器才能正常工作。在 JavaScript 中创建了 Web Socket 之后，会有一个 HTTP 请求发送
到浏览器以发起连接。在取得服务器响应后，建立的连接会使用 HTTP 升级从 HTTP 协议交换为 WebSocket 协议。

WebSockets协议（自定义协议）

 * 未加密的连接是 ws:// ；
 * 加密的连接是 wss://，会先通过HTTPS创建安全的连接，然后，该HTTPS连接升级为WebSocket连接，底层通信走的仍然是安全的SSL/TLS协议。

自定义协议

 * 优势：传送数据包小，非常适合移动端
 * 缺点：制定协议的时间比制定JS API的时间长

必须传入绝对路径，同源策略对WebSocket不起作用，通过它可以打开到任何站点的连接


## 属性和方法
1. readyState属性的值：
 * WebSocket.OPENING  (0)：正在建立连接。
 * WebSocket.OPEN  (1)：已经建立连接。
 * WebSocket.CLOSING  (2)：正在关闭连接。
 * WebSocket.CLOSE  (3)：已经关闭连接。
2. WebSocket对象的事件：
 * send：发送消息，只能接受纯文本参数，对于复杂数据要序列化为JSON字符串
 * open ：在成功建立连接时触发。
 * message：收到服务端消息时触发。返回数据保存在event.data 属性
 * error ：在发生错误时触发，连接不能持续。
 * close ：在连接关闭时触发。

    close事件对象event有三个属性：wasClean 、 code 和 reason 。

     * wasClean 是一个布尔值，表示连接是否已经明确地关闭;
     * code 是服务器返回的数值状态码;
     * reason 是一个字符串，包含服务器发回的消息。可以把这些信息显示给用户，也可以记录到日志中以便将来分析

WebSocket 对象不支持 DOM 2 级事件侦听器，因此必须使用 DOM 0 级语法分别定义每个事件处理程序。

    var socket = new WebSocket("ws://www.example.com/server.php");
    socket.send("Hello world!");
    socket.onmessage = function(event){
        var data = event.data;
        //处理数据
    };
    socket.onopen = function(){
        alert("Connection established.");
    };
    socket.onerror = function(){
        alert("Connection error.");
    };
    socket.onclose = function(){
        alert("Connection closed.");
    }

## 与其他方法的比较

1. HTTP协议是一个请求－响应协议，浏览器不主动请求，服务器是没法主动发数据给浏览器的。

2. 轮询

	浏览器通过JavaScript启动一个定时器，然后以固定的间隔给服务器发请求。这个机制的缺点一是实时性不够，二是频繁的请求会给服务器带来极大的压力。

3. HTTP流(Comet连接)

	浏览器向服务器发送一个请求，而服务器保持连接打开，然后周期性地向浏览器发送数据。一个HTTP连接在长时间没有数据传输的情况下，链路上的任何一个网关都可能关闭这个连接，而网关是我们不可控的，这就要求Comet连接必须定期发一些ping数据表示连接“正常工作”。