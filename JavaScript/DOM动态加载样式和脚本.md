# dom操作
加载外部样式是异步的

IE 将 <style> 视为一个特殊的、 与 <script> 类似的节点， 不允许访问其子节点。
## 动态脚本
1.通过script向页面中插入js代码，可以这样封装：

    function loadScript(url){
        var script = document.createElement("script");
        script.type = "text/javascript";
        script.src = url;
        document.body.appendChild(script);
    }
然后，就可以通过调用这个函数来加载外部的 JavaScript 文件了：

    loadScript("client.js");

用什么方法判断什么时候脚本加载完毕呢？

2.指定js代码的行内方式：

    function loadScriptString(code){
        var script = document.createElement("script");
        script.type = "text/javascript";
        try {
            script.appendChild(document.createTextNode(code));
        } catch (ex){
            script.text = code;   //兼容IE
        }
        document.body.appendChild(script);
    }
下面是调用这个函数的示例：
loadScriptString("function sayHi(){alert('hi');}");

## 动态样式
1.通过style标签动态添加样式
把CSS样式嵌入到HTML中，必须放在head中才能保证所有的浏览器行为一致

    function loadStyles(url){
        var link = document.createElement("link");
        link.rel = "stylesheet";
        link.type = "text/css";
        link.href = url;
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(link);
    }
调用 loadStyles() 函数的代码如下所示：

    loadStyles("styles.css");

2.动态添加行内样式

    function loadStyleString(css){
        var style = document.createElement("style");
        style.type="test/css";

        try{
            style.appendChild(document.creatTextNode(css));
        }catch{
            style.styleSheet.cssText = css;  //兼容IE
        }

        var head = document.getElementsByTagName("head")[0];
        head.appendChild(style);
    }

 调用这个函数的示例如下：
    loadStyleString("body{background-color:red}");
 这种方式会实时地向页面中添加样式，因此能够马上看到变化。

 注意：在IE中重用同一个style元素时，容易造成浏览器崩溃




























