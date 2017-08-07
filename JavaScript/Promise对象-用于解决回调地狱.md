# Promise
Promise 是异步编程的一种解决方案，将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

看下面这个例子：

    var wait1 = function(){
        var p = new Promise(function(resolve, reject) {
            var tasks = function(){
                console.log("1执行完毕！");
                resolve(); // 我已成功，下一位请继续
            };
            setTimeout(tasks,3000);
        });
        return p;
    };

    var wait2 = function(){
        var p = new Promise(function(resolve, reject) {
            var tasks = function(){
                console.log("2执行完毕！");
                resolve();
            };
            setTimeout(tasks,2000);
        });
        return p;
    };

    var wait3 = function(){
        var p = new Promise(function(resolve, reject) {
            var tasks = function(){
                console.log("3执行完毕！");
                resolve();
            };
            setTimeout(tasks,1000);
        });
        return p;
    };

    wait1().then(wait2).then(wait3).then(function(){
        console.log("定时器执行完毕");
    });
    //1执行完毕！
    //2执行完毕！
    //3执行完毕！
    //定时器执行完毕

这个例子相当于顺序执行（瀑布流的方式），后面的Promise.all()相当于并行执行任务。

## Promise的三种状态

* Pending（进行中）
* Resolved（已完成，又称 Fulfilled）
* Rejected（已失败）

Promise对象的状态改变，只有两种可能：
从Pending变为Resolved或从Pending变为Rejected。
一旦状态改变，就不会再变.

## 创建Promise实例
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject，这两个函数可以将异步操作结果，作为参数传递出去。
* resolve函数的作用是，将状态从“未完成”变为“成功”；
* reject函数的作用是，将状态从“未完成”变为“失败”。

     var  fn = function(num) {
        return new Promise(function(resolve, reject) {
            if (typeof num == 'number') {
                resolve(num);
            } else {
                reject('TypeError');
            }
        });
     };

    fn(2).then(function(num){
        console.log('first: ' + num);
        return num + 1;
    }).then(function(num){
        //获取上一个then回调函数的return返回结果
        console.log('second: ' + num);
    });


最好在resolve或reject前面加上return语句，这样可以保证后面的语句不会执行，后继操作应该放到then方法里面。

    new Promise((resolve, reject) => {
      return resolve(1);
      // 后面的语句不会执行
      console.log(2);
    })

## API
### Promise.prototype.then()
then方法返回的是一个新的Promise实例，可以采用链式写法，再调用另一个then方法，其
接受两个回调函数作为参数。

* 第一个是Promise对象的状态变为Resolved时调用，
* 第二个是Promise对象的状态变为Rejected时调用。


### Promise.prototype.catch()
该方法是.then(null, rejection)的别名（建议总是使用catch方法代替），用于指定发生错误时的回调函数。Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。

状态就会变为Rejected，或者then方法的回调函数中抛出错误，也会被下一个catch方法捕获。

    var promise = new Promise(function(resolve, reject) {
      throw new Error('test');
    });
    promise.catch(function(error) {
      console.log(error);
    });

如果Promise状态已经变成Resolved，再抛出错误是无效的。

    var promise = new Promise(function(resolve, reject) {
      resolve('ok');
      throw new Error('test');
    });
    promise.then(function(value){
        console.log(value)
    }).catch(function(error){
        console.log(error)
    });
    // ok

如果没有报错，则会跳过到下一个then方法。

    var someAsyncThing = function() {
        return new Promise(function(resolve, reject) {
            // 下面一行会报错，因为x没有声明
            resolve(x + 2);
        });
    };
    someAsyncThing().then(function(){
        console.log('log1');
    }).catch(function(){
        console.log('error1');
    }).then(function() {
        console.log('log2');
    }).catch(function(){
        console.log('error2');
    });
    //error1
    //log2

### Promise.all()
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

	var p = Promise.all([p1, p2, p3]);


（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的catch函数。


如果p1,p2,p3有自己的catch方法，执行完catch方法后，也会变成resolved，所以会调用p的then方法的回调，并不会被p的catch方法捕获。

### Promise.race()

	var p = Promise.race([p1, p2, p3]);

只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变,最先改变的 Promise 实例的返回值，就传递给p的回调函数。否则调用catch回调

### Promise.resolve()
将现有对象转为Promise对象后，执行then方法的回调。

	Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))

1. 如果参数是Promise实例，那么Promise.resolve将不做任何修改。
2. 参数是一个具有then方法的对象，将这个对象转为Promise对象，然后立即执行该对象的then方法。
3. 不带有任何参数，直接返回一个Resolved状态的Promise对象。  
4. 非以上参数，比如原始值，返回Promise实例，并调用resolve传递该参数。

### Promise.reject()
同Promise.resolve()，但是会被catch回调捕获。

    var p = Promise.reject('出错了');
    // 等同于
    var p = new Promise((resolve, reject) => reject('出错了'))
    p.then(null, function (s) {
      console.log(s)
    });
    // 出错了

