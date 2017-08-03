
# JS数据类型及检测方法


## JS的六中基本类型
五种原始类型（简单）：number、string、boolean、null、undefined

一种对象类型（复杂）：object(Function、Array、Date、String、Number、Boolean、Error...)


## 包装对象
在JavaScript中只要引用了字符串（数字、布尔值也是一样的）的属性，就会进行对象的包装，
将string => String()，使基本类型的数据具有包装对象的属性和方法。

看下面一段代码：

    var a = "string";
    console.log(a.length); //6
    a.t = 3;
    console.log(a.t); //undefined

当尝试把基本类型的str当做对象一样访问时，例如：str.length;
解释器会创建一个临时的包装对象，伪代码：

	[[tempObj]] = new String(str);
	[[tempObj]].length; 		// 返回具体的length;
	delete [[tempObj]]; 		// 销毁临时对象

重复访问str.length会重复创建这个临时对象。
所以str.t赋值可以成功，但再次访问str.t返回undefined，因为每次创建的临时包装对象都是不同的。


## 类型检测

 1. typeof:用于基本类型和function的判断，返回一个字符串
	
	typeof Number; 	//返回“function”

|操作数							|返回值			|
|-------------------------------|---------------| 
|undefined、未初始化及未声明的变量|"undefined"	|				
|true或者false					|"boolean"		|
|任意数字或NaN					|"number"		|
|任意字符串 					|"string"		|
|任意函数,包括可执行对象    	|"function"		|
|null或者任意内置对象(非函数) 	|"object"		|


注意：

	可调用对象RegExp,window.alert(),document.getElementsById(),他们其实是对象，而不是函数，但是他们执行typeof的结果并不统一，有些浏览器返回"function"，有些是"object"。
在IE9之前document.getElementsById()返回的是"object"。

要判断是对象还是函数对象（并且具有函数方法），需要检测class属性：
	
	function isFunc（x）{
		return Object.prototype.toString.call(x) == "[object Function]";
	}		


 2. instanceof:判断其他对象类型（原型）

   适合自定义对象，也可以用来检测原生对象，左操作数和右操作数必须在同一个全局作用域内，否则不等（不同的window或iframe间的对象类型检测不能使用instanceof）。
    左操作数是一个对象，右操作数是一个函数，如果有操作数不是函数，则会抛出异常。
    o instanceof f; js会先计算f.prototype,在原型链中查找o，如果找到，那么o是f的一个实例，返回true

instanceof存在的问题：
左操作数和右操作数必须在同一个全局作用域内
无法检测是原生对象还是开发人员自定义类型的对象

 3.Object.prototype.toString
    适合内置对象和基本类型，检测原生构造函数的构造函数名，遇到null和undefined失效,开发人员定义的任何够咱函数都返回"[object Object]"。
    Object.prototype.toString可被修改，这里讨论未被修改的原生对象

     Object.prototype.toString.apply([]); //"[object Array]"
     Object.prototype.toString.apply(function(){}); //"[object Function]"
     Object.prototype.toString.apply(null); //"[object Null]",(IE6/7/8返回[object Object])
     Object.prototype.toString.apply(undefined); //"[object Undefined]"

 4.constructor
    原型对象的constructor容易被改写
 5.duck type




### 任何类型的值都可以调用Boolean()函数，转换为boolean值

	var message='hello';
	var msgBool = Boolean(message);

|数据类型 |转换为true |转换为false|
|---------|-----------|-----------|	
|Boolean  |true		  |false      |
|Number   |非0数值    |0、NaN     |
|String   |非空字符串 |" "、undefined|
|Object   |任何对象   |null       |

对于boolean的转换规则，在流式控制语句（比如if）很重要，比如下面两个例子输出不一样：

	var message='hello';
	if(message){
		console.log('true');
	}
	//输出‘true’

	if(a==true){
    	console.log("true");
    }else{
    	console.log("false");
    }
	//输出‘false’

可以参考[==与====的判断规则](/==和===的区别及转换规则.md)
	

{{ page.date|date_to_string }}




