# 移动端事件


* touchstart ：当手指触摸屏幕时触发；即使已经有一个手指放在了屏幕上也会触发。
* touchmove ： 当手指在屏幕上滑动时连续地触发。 在这个事件发生期间，调用 preventDefault()可以阻止滚动。
* touchend ：当手指从屏幕上移开时触发。
* touchcancel ：当系统停止跟踪触摸时触发。关于此事件的确切触发时间，文档中没有明确说明。

上面这几个事件都会冒泡，也都可以取消。

每个 Touch 对象包含下列属性：

    * clientX ：触摸目标在视口中的 x 坐标。
    * clientY ：触摸目标在视口中的 y 坐标。
    * identifier ：标识触摸的唯一 ID。
    * pageX ：触摸目标在页面中的 x 坐标。
    * pageY ：触摸目标在页面中的 y 坐标。
    * screenX ：触摸目标在屏幕中的 x 坐标。
    * screenY ：触摸目标在屏幕中的 y 坐标。
    * target ：触摸的 DOM 节点目标


在移动端，手指点击一个元素，一般会经过：

	touchstart -> touchmove -> touchend -> click

## click触发的条件

手指触摸开始，且手指未曾在屏幕上移动（某些浏览器允许移动一个非常小的位移值），且触摸和离开屏幕之间的间隔时间较短（某些浏览器不检测间隔时间，也会触发click）才能触发。
	touchstart -> touchend -> click

## 移动click事件的300ms延迟

* 原因：最初是为了判断用户是否要双击缩放页面

* 解决：用tap事件代替，自定义的事件

* 自定义Tap事件的原理：

	在 touchstart、touchend 时记录时间、手指的位置，如果手指尾同一位置（或允许移动一个非常小的位移值），且时间间隔较短，一般认为200ms，且这个过程未曾触发过touchmove，


## Tap事件的点透bug

使用touchstart去代替click事件有两个不好的地方：

1. touchstart是手指触摸屏幕就触发，有时候用户只是想滑动屏幕，却触发了touchstart事件，这不是我们想要的结果；
2. 使用touchstart事件在某些场景下可能会出现点击穿透的现象。

touchend 结束后，蒙层消失，300ms以后触发 click 事件，由于蒙层的消失，造成click被底层元素接收，造成“点透”。

解决办法：

1. 使用缓动动画，过渡300ms的延迟再消失
2. 中间加入透明层，接受“穿透事件”，随后消失
3. 都统一使用tap事件，不使用click事件
4. 改用FastClick库,来解决300ms的延迟

