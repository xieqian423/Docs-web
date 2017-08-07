# Promise中的事件循环

    setTimeout(function () {
      console.log('timer');
    }, 0);

    let promise = new Promise(function(resolve, reject){
      console.log('Promise');
      resolve();
    });
    promise.then(function() {
      console.log('Resolved.');
    });
    console.log('Hi!');
    // Promise
    // Hi!
    // Resolved
    // timer


Promise 新建后立即执行，所以首先输出的是Promise。
然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，即本轮事件循环的末尾，所以“Resolved”在“Hi”后面输出。
setTimeout(fn, 0)在下一轮“事件循环”开始时执行


转载：

http://www.jianshu.com/p/063f7e490e9a
