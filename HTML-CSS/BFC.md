# BFC

BFC是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素。比如：内部滚动就是一个BFC，当一个父容器的overflow-y设置为auto时，并且子容器的长度大于父容器时，就会出现内部滚动，无论内部的元素怎么滚动，都不会影响父容器以外的布局，这个父容器的渲染区域就叫BFC。

### 满足下列条件之一就可触发BFC

1. 根元素，即HTML元素
2. float的值不为none
3. overflow的值不为visible
4. display的值为inline-block、table-cell、table-caption
5. position的值为absolute或fixed

### 作用
1. 可以包含浮动元素
2. 不被浮动元素覆盖
3. 阻止父子元素的margin折叠

参考：

http://blog.csdn.net/liyuanyuan_0511/article/details/52441709






