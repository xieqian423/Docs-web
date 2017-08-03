# JSON数据的序列化与反序列化

## 什么是JSON
JSON是一种数据格式，在数据流中以字符串的形式传输，它是轻量级的数据交换方式。
优点：独立于语言、传输效率高

## JSON的序列化
    JSON.stringify()
可以在JSON中表示字符串、数值、布尔值和null。<br/>
当对象的属性值为NaN时，会被转化成null；属性值为undefined，会去掉该属性；new Date()会被转化成具体的日期字符串

## JSON的反序列化

 1.JSON.parse(string)
 
 字符串中对象的属性必须是“双引号”

 2.eval()
 
 eval() 函数使用的是 JavaScript 编译器，可解析JSON 文本，然后生成 JavaScript 对象,属性名不一定要加引号也可以。必须把文本包围在括号中，这样才能避免语法错误：

    eval('('+string+')');
 因为字符串里面的js代码也会被解析执行，所以使用eval方法比较危险

 3.new Function()
 
    (new Function("", "return "+string))();

## 自定义序列化方法
### JSON.stringify()的过滤器
    JSON.stringify()
    除要序列化的JS对象外，还能接收另外两个参数,这两个参数用于指定以不同的方式序列化JS对象。
    第一个参数是个过滤器,可以是一个数组,也可以是一个函数。<br/>
    第二个参数是一个选项,表示是否在JSON字符串中保持缩进。

 1.如果过滤器参数是个数组,那么JSON.stringify()的结果中只包含数组中列出的属性。

    var book = {
        title: "Professional Javascript",
        authors:["zzh","dff"],
        edition:2,
        year:2013
    };
    var jsonText = JSON.stringify(book,["authors"]);
    console.log(jsonText); //{"authors":["zzh","dff"]}

 2.如果过滤器参数是个函数,此函数接收两个参数:属性名(键)和属性值。根据键名处理要序列化的对星座的属性。
    为了返回序列化对象的结果,函数返回的值就是相应键的值。需要注意的是：<br/>
    （1）如果值为undefined,那么相应的属性会被忽略.<br/>
    （2）一定要提供defalut项,这样不需要特殊处理的属性,才能被正常返回，不然未被列出的属性会被忽略。

    var book = {
        title: "Professional Javascript",
        authors:["zzh","dff"],
        edition:2,
        year:2013
    };
    var jsonText = JSON.stringify(book, function(key,value){
        switch(key){
            case "title":
                return "Javascript";
            case "authors":
                return "Yvette";
            case "edition":
                return undefined;
            default:
                return value;
        }
    });
    console.log(jsonText);
    //{"title":"Javascript","authors":"Yvette","year":2013}

### JSON.parse()方法
JSON.parse()的第二个参数被称为还原函数(receiver).这个函数接收的参数同样是一个键和一个值。

    var jsonObj = {
        name:"Tom",
        date: new Date(2011,2,2),
    };
    var str = JSON.stringify(jsonObj);
    console.log(str);

    var obj = JSON.parse(str,function(key,value){
        if(key=='date'){
            return new Date(value);
        }else{
            return value;
        }
    });
    console.log(obj);
    //{name: "Tom", date: Wed Mar 02 2011 00:00:00 GMT+0800 (中国标准时间)}

参考链接：
http://blog.csdn.net/liuyan19891230/article/details/51372665

http://www.cnblogs.com/craftsman-gao/p/5130567.html













