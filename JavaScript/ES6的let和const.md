#ES6的变量声明let和const

##let
let用于定义块级作用域，只在声明的块级作用域内有效

看一下下面的例子：

    function test(){
        var arr1 = [];
        for(let i = 0; i<3; i++){
            setTimeout(function(){
                console.log(i);
            },50*i);
        }

        for(var j = 0; j<3; j++){
            setTimeout(function(){
                console.log(j);
            },300);
        }
    }
    test(); //依次输出0,1,2 3,3,3
var声明的变量由于存在函数作用域内，执行定时器时，该函数的父作用域是test函数，
而let只在块作用域内有效，设置let变量时会创建一个作用域，循环体内部是一个单独的子作用域，i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量

### 不存在变量提升

    function test(){
        console.log(tmp);  //undefined
        if(true){
            console.log(tmp); //ReferenceError: tmp is not defined
            let tmp='123';
        }
        var tmp='abc';
    }
    test();  //执行时会报错

上述第一次输出的undefined，是var tmp='abc';变量提升的结果，第二次输出报错，由于let命令改变了语法行为，
但是如果不用let声明，依然会访问副作用域的同名变量:

    function test(){
        console.log(tmp);  //undefined
        if(true){
            console.log(tmp); //undefined
        }
        var tmp='abc';
    }
    test();  //

###其他特性
 1.typeof不再安全

    typeof x; // ReferenceError
    let x;

 2.不允许重复声明

     let a = 10;
     let a = 1;  //报错

     function func(arg) {
       let arg; // 报错
     }

##const 命令
 1. const声明常量，一旦声明，常量的值就不能改变，记住是常量哦。

    const PI = 3.1415;
    PI = 3;    // TypeError: Assignment to constant variable.

再看一个例子：

    const foo = {};

    // 为 foo 添加一个属性，可以成功
    foo.prop = 123;
    console.log(foo.prop);  // 123

    // 将 foo 指向另一个对象，就会报错
    foo = {}; // TypeError: "foo" is read-only

说明：常量foo储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，
即不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

 2.因为声明的变量不得改变值，意味着，const一旦声明变量，就必须立即初始化，

    const foo;
    // SyntaxError: Missing initializer in const declaration
 3.和let一样只在块内有效，并且不会提升变量

        if (true) {
          const MAX = 5;
        }
        MAX // Uncaught ReferenceError: MAX is not defined


###块作用域中的函数

    'use strict';
    foo();  //输出1
    if (true) {
       function foo() {
            console.log(1);
        }
    }

可以在块中定义函数，但是依然会有函数提升。

我的理解ES6的块作用域只是针对let和const定义的变量有效。

