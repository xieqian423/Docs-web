## 实例化与原型链

### new一个对象，发生了什么？
（1）创建一个新对象var obj = {};
（2）把这个对象obj作为this去调用构造函数
（3）把这个对象的__ptoto__属性设置为构造函数的prototype
（4）返回obj（如果构造函数有显式返回对象类型的，则是构造函数返回的这个对象

new 只能作用于构造函数即function


### prototype 与 _proto_
prototype：只有函数(准确地说是构造函数)才有的，内置constructor属性，prototype也属于普通对象所以有__proto__属性
__proto__：是所有对象(包括函数)都有的属性，它才叫做对象的原型，原型链就是靠它形成的，实例化一个对象时将prototype中的值拷贝到每个新建对象的__proto__中。

### 原型链
原型链的概念：
　　每个对象都会在其内部初始化一个属性，就是__proto__，当我们访问一个对象的属性 时，如果这个对象内部不存在这个属性，那么他就会去__proto__里找这个属性，这个__proto__又会有自己的__proto__，于是就这样 一直找下去。
  
      var A = function () {}
      A.prototype.getName = function () {
         console.log(1)
      }
      
      var  a = new A()
      var b = new A()
      a.getName()  // 1
      A.prototype.getName = function () {
           console.log(2)
      }
      b.getName()  // 2
   由此可以看出, new对象时是将对象的__ptoto__属性指向为构造函数的prototype的引用
   
 ### Object.create
 作用于 function 和 object
