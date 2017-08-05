# Commonjs与AMD、CMD
通行的模块规范有两种：CommonJS 和 AMD（Asynchronous Module Definition）。

* 同步模块加载:(CommonJS) 

	nodejs 的模块系统参照 COMMONJS 规范实现的,同步加载,因为服务端的所有模块都放在本地,可以同步加载完成，加载时间只是读取文件的时间

* 异步模块加载:(AMD, require.js 实现了 AMD 规范)

	浏览器端模块的加载时间取决于网速的快慢,很有可能要等待很长时间,影响后面代码的执行,造成"假死"的状态,所以只能采用"异步加载"。

## Commonjs
在服务器端一定要有模块，CommonJS规范是由NodeJS发扬光大。

commonjs是用在服务器端的，同步的，只有加载完成才能执行后面的操作。
先加载模块，然后就可以调用模块提供的方法：

    var math = require('math');
    math.add(2,3);

## 浏览器的模块加载

    <script src="1.js"></script>
    <script src="2.js"></script>
    <script src="3.js"></script>
    <script src="4.js"></script>

这段代码依次加载多个js文件。这样的写法有很大的缺点。

1. 加载的时候，浏览器会停止网页渲染，加载文件越多，网页失去响应的时间就会越长；
2. 由于js文件之间的依赖关系，必须严格保证加载顺序（比如上例的1.js要在2.js的前面），当依赖关系很复杂的时候，代码的编写和维护都会变得困难。

 require.js的诞生，就是为了解决这两个问题：
 * 实现js文件的异步加载，避免网页失去响应;
 * 管理模块之间的依赖性，便于代码的编写和维护。

### AMD（Asynchronous Module Definition，异步模块定义）

所有依赖这个模块的语句，都定义在一个回调函数中,实现了异步方式，模块的加载不影响他后面语句的运行：

    require([module1,module2], callback);

AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块，用户体验好，因为没有延迟，依赖模块提前执行了

     define(['./a', './b'], function(a, b) {  // 依赖必须一开始就写好
         a.doSomething()
         // 此处略去 100 行
         b.doSomething()
         ...
     })

缺点：
加载多个模块就会发送多次http请求，影响网页加载速度，需要将多个模块合并在一个js文件中，
对此Rquirejs提供了优化工具r.js，

### CMD（Common Module Definition，通用模块定义），如SeaJS
CMD和AMD要解决的问题是一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。

参照Commonjs和AMD规范编写，推崇就近依赖，性能好，因为只有用户需要的时候才执行。

     define(function(require, exports, module) {
         var a = require('./a')
         a.doSomething()
         // 此处略去 100 行
         var b = require('./b') // 依赖可以就近书写
         b.doSomething()
     })


总结：RequireJS和SeaJS都是通过js的运行时来支持“匿名闭包”、“依赖分析”和“模块记载”等功能

依赖分析：需要js运行时通过正则匹配到模块的依赖关系，然后顺着依赖链顺序加载模块，但是当依赖关系复杂时，会影响性能，这一点可以参考FIS解决方案：
 http://fex.baidu.com/blog/2014/03/fis-module/