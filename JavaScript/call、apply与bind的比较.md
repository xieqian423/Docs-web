# call、apply与bind的比较

  call/apply/bind都可用于改变函数执行时的上下文环境,也就是函数体内部的this指向。
  
  不同的是：
   1.call和apply会立即执行；<br/>
   2.bind（ES5提供，IE9以上的浏览器才可以使用）绑定一次，绑定后，其上下文环境不会改变，并且会返回相应的函数便于重复调用，比call/apply更高效；

## call/apply
call与apply第一个参数都是要绑定的对象，apply的其他参数以数组的形式传递

    function foo(x,y,z){
     console.log(x,y,this);
    }
    foo.call(100,1,2);      //1,2,Number(100)
    foo.apply(true,[1,2]);  //1,2,Boolean(100)
    foo.apply(null,[1,2]);  //1,2,window,在严格模式下this输出null
    foo.apply(undefined,[1,2]);//1,2,window,在严格模式下this输出undefiend

## bind
bind的另一个作用是函数柯里化。函数柯里化：将函数拆成多个单元，函数比较复杂需要做多次拆分，但是调用模块的某些参数值固定相同
Function.prototype.bind == true 用于判断bind方法是否存在

    function foo(x,y,z){
        console.log(x,y,z);
    }
    var func = foo.bind(null,1);
    func();                          //1 undefined undefined
    var func1 = func.bind(null,2);
    func(3,4);                        //1 3 4
    func1(100);                     //1 2 100
    
    function foo(x,y,z){
        this.b = 100;
        return this.a;
    }
    var func = foo.bind({a:1});
    func();//1
    var test = new func();//foo {b:100}
    说明：new调用时，return的如果不是对象的话，会把this作为返回值，并且this会被初始化为一个空对象，
        这个对象的原型是foo.prototype，所以空对象的b属性设置为100，整个对象作为返回值返回，会忽略return。

    
