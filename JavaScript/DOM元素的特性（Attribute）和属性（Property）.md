# DOM元素e的特性（Attribute）和属性（Property）

Attribute就是dom节点自带的属性，只要是DOM标签中出现的属性（html代码），都是Attribute，例如html中常用的id、class、title、align等：

	<div id="div" class="class1" tutle="title1"></div>

Property是这个DOM元素作为对象，其附加的内容，在访问特定元素的特性时，浏览器解析元素后生成对应对象，例如childNodes、firstChild等。

e.getAttribute()，是标准DOM操作文档元素属性的方法，具有通用性可在任意文档上使用，返回元素在源文件中设置的属性

## 特性（Attribute）

 1. 特性（Attribute）取值

  **e.Attributes** 获取所有的特性信息，e.Attributes将返回一个NamedNodeList类数组（自己设置的，反映在标签上的），其中包含了若干个Attr类型的对象；

  **getAttribute()** 获取的是字符串，如果没有设置，返回null

		<input type="button" id="btn1" value="按钮1"/>

		btn1.getAttribute("height"); //null
		btn1.getAttribute("type"); 	//"button"
		btn1.attributes;  	//NamedNodeMap {0: type, 1: id, 2: value, length: 3}
		btn1.attributes.height; 	// undefined
		btn1.attributes.type; 		//type="button"
		
  

 2.  特性（Attribute）赋值

  用 **setAttrbute()** 赋值，任何Attribute都可以，包括自定义的。而且，赋值的Attribute会立刻表现到DOM元素上。

  setAttribute()的两个参数，都必须是字符串

 3. 特性检测 hasAttribute()

## 属性（Property）

 1. 属性取值很简单。取任何属性的只，用 “.” 就可以：
 2. 赋值和基本的js对象属性赋值一样，用 “.” 即可，属性Property可以赋任何类型的值


## 属性与特性的交叉
常用的Attribute，例如id、class、title等，已经被作为Property附加到DOM对象上，可以和Property一样取值和赋值，是一一对应的，修改其中一个会影响另一个。但是自定义的Attribute，不可以。


    id、class、title等，用“.”和getAttribute()获取结果一样，都是字符串。

    对于style和onclick这两者，使用“.”取值是就不是简单的字符串值了。

1. “class”变成Property之后叫做“className”，
因此e.className和e.getAttrbute('class')

2. 对于style

   用“.”获取style，返回了一个CSSStyleDeclaration对象；
   
   用getAttribute()获取style，返回的就是一个简单的字符串。



对于属性Property的赋值在IE中可能会引起循环引用，内存泄漏。为了防止这个问题，jQuery.data()做了特殊处理，解耦了数据和DOM对象



