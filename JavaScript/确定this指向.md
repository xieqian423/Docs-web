## 确定this指向
    
在函数中this取何值，是在函数真正被调用执行的时候确定的，函数定义的时候确定不了。
因为this的取值是执行上下文环境的一部分，每次调用函数，都会产生一个新的值，this一旦确定就不可更改。
   
    function fn () {
        this = obj;       // 这句话试图修改this，运行后会报错
        console.log(this.a);
    }
    fn();

### 函数的调用方式及this值
 1.作为全局对象的属性
 
    var x=10;
    function fn(){
      var x=20;
      console.log(this.x);
    }
    window.fn();  //10
  
 也可以直接调用fn(),但是这种情况在严格模式下，属于单独调用情况，直接调用fn()会报错
 
 2.作为普通对象的属性调用
 
    var obj = {
      x:10,
      fn:function(){
        console.log(this.x);
      }
    }
  
    obj.fn();  //10
  
    var fn1 = obj.fn;  
    fn1();    //undefined, 属于单独调用情况，非严格模式下指向的是window对象
  
  
  3.使用call、apply改变this指向
   this的值就取传入的对象的值，即第一个参数
   
    function foo(x,y,z){
     console.log(x,y,this);
    }
    foo.call(100,1,2);      //1,2,Number(100)
    foo.apply(true,[1,2]);  //1,2,Boolean(100)
    foo.apply(null,[1,2]);  //1,2,window,在严格模式下this输出null
    foo.apply(undefined,[1,2]);//1,2,window,在严格模式下this输出undefiend
  
  4.作为构造函数调用
  作为构造函数被调用时，构造函数无返回值，this指向新创建的对象；
  
    function C1(){  //默认返回值为this
        this.a = 1;
    }
    var o = new C1();
    console.log(o.a);  //1


    function C2(){
        this.a = 1;
        this.b = 2;
        
        return {a:100};
    }
    var o = new C2();
    console.log(o.a);  //100
    console.log(o.b);  //undefined
    这种情况下this指向构造函数本身

  5.其他几种例子：

    var a = 20;
    function fn() {
        var a = 10;
        function foo() {
            console.log(this.a);
        }
        foo();     
    }
    fn();   //20    
    这是因为fn里面的foo单独调用，可以用第一种情况解释，严格模式下报错
    
  再看下面这个例子
    
    var a = 20;
    var obj = {
        a: 10,
        c: this.a + 20,
        fn: function () {
            return this.a;
        }
    }
    console.log(obj.c);    //40
    console.log(obj.fn()); /10
    
    对象obj中的c属性使用this.a + 20来计算，而他的调用者obj.c并非是一个函数。因此他不适用于上面的规则，我们要对这种方式单独下一个结论：<br/>
    当obj在全局声明时，无论obj.c在什么地方调用，这里的this都指向全局对象；而当obj在函数环境中声明时，这个this指向undefined，在非严格模式下，会自动转向全局对象。
        
    
### 根据以上总结：
如果函数，被某一个对象所拥有，那么该函数在调用时，内部的this指向该对象。如果函数独立调用，那么该函数内部的this，则指向undefined。
但是在非严格模式中，当this指向undefined时，它会被自动指向全局对象。

  那么对于下面的this，我们就可以很快清楚this的指向了

    var x=100;
    var o={
        x:1,
        fn:function(){
            console.log(this.x);
            function fn1(){
                console.log(this.x);
            }
            fn1();
        }
    }
    o.fn();
    输出结果：1 100，说明fn1执行时，this值为window

    var func = o.fn;
    func();
    输出结果：100 100,说明fn执行时，this值为window








