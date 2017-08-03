# iframe的优缺点

iframes 提供了一个简单的方式把一个网站的内容嵌入到另一个网站中.但我们需要慎重的使用iframe。iframe的创建比其它包括scripts和css的 DOM 元素的创建慢了 1-2 个数量级。

## 优点
 1. 程序调入静态页面比较方便;
 2. 页面和程序分离;

## 缺点
 1. iframe好在能够把原先的网页全部原封不动显示下来,但是如果用在首页,是搜索引擎最讨厌的.
 2. 样式/脚本需要额外链入，会增加请求。
 3. 框架结构有时会让人感到迷惑，特别是在多个框架中都出现上下、左右滚动条的时候。这些滚动条除了会挤占已经特别有限的页面空间外.

## iframe与frame的比较
 1. frame不能脱离frameSet单独使用，iframe可以；
 2. frame不能放在body中,存在frame时不能与 <frameset></frameset> 标签一起使用 <body></body> 标签，两者选择其一；
 3、嵌套在frameSet中的iframe必需放在body中；
 4. frame的高度只能通过frameSet控制；iframe可以自己控制，不能通过frameSet控制。
