# DOM样式操作

## 获取样式的方法
 1. e.style

  只可用于获取特性的值，并不能真实反映dom所呈现的样式。这种方式能读能写

 2. getComputedStyle  

是一个可以获取当前元素所有最终使用的CSS属性值，方法是只读的，只能获取样式。

   接收两个参数：dom对象，伪类；不需要获取伪类样式，第二个参数null

    window.getComputedStyle(dom , ":after");

   window.getComputedStyle 与 document.defaultView.getComputedStyle 相似，只是在FireFox3.6上只能使用defaultView获取样式

3.currentStyle

 是IE浏览器的一个属性 e.currentStyle 与 getComputedStyle() 作用相近，但不支持伪类样式获取


例如，我们要获取一个元素的高度，可以类似下面的代码：

    alert((element.currentStyle? element.currentStyle : window.getComputedStyle(element, null)).height);


## getComputedStyle
   getPropertyValue 方法可以获取CSS样式申明对象上的属性值（直接属性名称）

    window.getComputedStyle(element, null).getPropertyValue("float"); //不支持驼峰式写法

某些特殊样式,比如float

   不能使用 getComputedStyle(element, null).float，而应该是getComputedStyle(element, null).cssFloat与getComputedStyle(element, null).styleFloat