# CSS动画设置
在Web应用中，实现动画效果的方法比较多

 * setTimeout
 * css3可以使用 transition 和 animation
 * html5 中的 canvas
 * html5 请求动画的API，即 requestAnimationFrame（rAF）。

## CSS3动画
### transition
让元素的CSS属性值的变化在一段时间内平滑的过渡，形成动画效果

* 结束事件 transitionEnd

	该事件是在CSS完成过渡后触发，如果过渡在完成之前被移除，则不会触发transitionEnd

### animation
需要设置一个@keyframes，定制不同帧的样式

 * animation-play-state:paused 可以暂停动画
 * animation-fill-mode:forwards 可以让动画完成后定格在最后一帧

js 监听animation的三个事件：

 1. animationStart 开始
 2. animationEnd	  结束
 3. animationIteration 重复播放，当播放次数设置为1时，不会触发animationIteration。


为了使元素的变换更加丰富多彩，CSS3还引入了transfrom 属性，它可以通过对元素进行
 平移(translate)、旋转(rotate)、放大缩小(scale)、倾斜(skew) 等操作，来实现2D和3D变换效果。

### 区别
 transition 只能通过主动改变元素的css值才能触发动画效果，而animation一旦被应用，就开始执行动画。


## requestAnimationFrame
HTML5新增的一个动画API，它通过JS来调用，并按照屏幕的绘制频率来改变元素的CSS属性，从而达到动画效果。
系统每次绘制之前会主动调用 rAF 中的回调函数

### 与setTimeout的比较

1. setTimeout实现的动画在某些低端机上会出现卡顿、抖动的现象
 * setTimeout的渲染原理：

  setTimeout执行只是在内存中对元素属性进行改变，这个变化必须要等到屏幕下次绘制时才会被更新到屏幕上。如果两者的步调不一致，就可能会导致中间某一帧的操作被跨越过去，而直接更新下一帧的元素。setTimeout 的执行步调和屏幕的刷新步调不一致，就会引起丢帧现象。

2. CPU节能：使用setTimeout 实现的动画，当页面被隐藏或最小化时，setTimeout仍然在后台执行动画任务，浪费CPU资源。
而rAF，当页面处理未激活的状态下，该页面的屏幕绘制任务也会被系统暂停，因此跟着系统步伐走的 rAF 也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销。

3. 在高频率事件(resize,scroll等)中，为了防止在一个刷新间隔内发生多次函数执行，使用 rAF 可保证每个绘制间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销。

 参考：
 http://www.cnblogs.com/onepixel/p/7078617.html