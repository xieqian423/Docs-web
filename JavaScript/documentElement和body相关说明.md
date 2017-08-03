# documentElement 和 body 相关说明

 body是DOM对象里的body子节点，即 <body> 标签;

 documentElement 是整个节点树的根节点root，即<html> 标签；


一般来说：

页面具有 DTD，或者说指定了 DOCTYPE 时，使用 document.documentElement。

页面不具有 DTD，或者说没有指定了 DOCTYPE，时，使用 document.body。


一般用来获取滚动的高度，兼容写法：

var top = document.documentElement.scrollTop || document.body.scrollTop;

