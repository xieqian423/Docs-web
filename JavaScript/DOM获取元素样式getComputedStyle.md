# DOM样式操作及类操作

## 获取样式的方法
 1. e.style

  只可用于获取特性的值，并不能真实反映dom所呈现的样式。这种方式能读能写

 2. getComputedStyle  

 是一个可以获取当前元素所有最终使用的CSS属性值，方法是只读的，只能获取样式。

   接收两个参数：dom对象，伪类；不需要获取伪类样式，第二个参数null

    	window.getComputedStyle(dom , ":after");

   window.getComputedStyle 与 document.defaultView.getComputedStyle 相似，只是在FireFox3.6上只能使用defaultView获取样式

 3. currentStyle

 是IE浏览器的一个属性 e.currentStyle 与 getComputedStyle() 作用相近，但不支持伪类样式获取

 例如，我们要获取一个元素的高度，可以类似下面的代码：

        alert((element.currentStyle? element.currentStyle : window.getComputedStyle(element, null)).height);


## getComputedStyle
   getPropertyValue 方法可以获取CSS样式申明对象上的属性值（直接属性名称）

    window.getComputedStyle(element, null).getPropertyValue("float"); //不支持驼峰式写法

某些特殊样式,比如float

   不能使用 getComputedStyle(element, null).float，而应该是getComputedStyle(element, null).cssFloat与getComputedStyle(element, null).styleFloat


## HTML5与类相关的
html4之后，class属性用的越来越多，所以添加与类相关的操作

1. getElementsByClassName()

 在 document 对象上调用getElementsByClassName() 始终会返回与类名匹配的所有元素，在元素上调用该方法就只会返回后代元素中匹配的元素。
因为返回的对象是 NodeList，所以使用这个方法与使用 getElementsByTagName()以及其他返回 NodeList 的 DOM 方法都具有同样的性能问题

2. classList
 classList属性是新集合类型 DOMTokenList 的实例。

 DOMTokenList有一个表示自己包含多少元素的length属性，此外，这个新类型还定义如下方法：
 * add(value) ：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
 * contains(value) ：表示列表中是否存在给定的值，如果存在则返回 true ，否则返回 false 。
 * remove(value) ：从列表中删除给定的字符串。
 * toggle(value) ：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。

     //删除"disabled"类
     div.classList.remove("disabled");
     //添加"current"类
     div.classList.add("current");
     //切换"user"类
     div.classList.toggle("user");

有了classList属性，除非你需要全部删除所有类名，或者完全重写元素的 class 属性，否则也就用不到 className 属性了。
支持 classList 属性的浏览器有 Firefox 3.6+和 Chrome。
