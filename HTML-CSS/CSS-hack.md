# CSS-hack
不同厂商的浏览器，或者是同一厂商的浏览器的不同版本，如IE6和IE7，对CSS的解析认识不完全一样，因此会导致生成的页面效果不一样。
CSS hack的目的就是使你的CSS代码兼容不同的浏览器。利用CSS hack为不同版本的浏览器定制编写不同的CSS效果。

hack就是在CSS样式中加入一些特殊的符号，让不同的浏览器识别不同的符号。

## CSS Hack大致有3种表现形式
以下都在标准模式中

### 条件注释法
 HTML条件注释Hack,这种方式是IE浏览器专有的Hack方式，微软官方推荐使用的hack方式。举例如下

 	只在IE下生效
 	<!--[if IE]>
 	这段文字只在IE浏览器显示
 	<![endif]-->

 	只在IE6下生效
 	<!--[if IE 6]>
 	这段文字只在IE6浏览器显示
 	<![endif]-->

 	只在IE6以上版本生效
 	<!--[if gte IE 6]>
 	这段文字只在IE6以上(包括)版本IE浏览器显示
 	<![endif]-->

 	只在IE8上不生效
 	<!--[if ! IE 8]>
 	这段文字在非IE8浏览器显示
 	<![endif]-->

 	非IE浏览器生效
 	<!--[if !IE]>
 	这段文字只在非IE浏览器显示
 	<![endif]-->

###  类内属性前缀法
属性前缀法是在CSS样式属性名前加上一些只有特定浏览器才能识别的hack前缀

    selector{<hack>?property:value<hack>?;}

比如 IE6能识别下划线"_"和星号" * "，IE7能识别星号" * "，但不能识别下划线"_"，而firefox两个都不能认识。

    div{
        background:green;/*forfirefox*/
        *background:red;/*forIE6*/(bothIE6&&IE7)
    }
	在IE6中看到是红色的，在firefox中看到是绿色的。

上面的css在firefox中，它是认识不了后面的那个带星号的东西是什么的，于是将它过滤掉，不予理睬，解析得到的结果是: div{background:green}, 于是理所当然这个 div 的背景是绿色的。
在IE6中呢，它两个 background 都能识别出来，它解析得到的结果是: div{background:green;*background:red;},于是根据优先级别，处在后面的 red 的优先级高，于是当然这个div的背景颜色就是红色的了。

    test{
        color:#000; /* 正常写法普遍支持 */
        color:#00F\9; /* 所有IE浏览器(ie6+)支持 */
        /*但是IE8不能识别“ * ”和“ _ ” */
        [color:#000;color:#0F0; /* SF,CH支持 */
        color:#00F\0; /* IE8支持*/
        *color:#FF0; /* IE7支持 */
        _color:#F00; /* IE6支持 */
    }

### 选择器前缀法
在CSS选择器前加上一些只有某些特定浏览器才能识别的前缀进行hack。

    *html *前缀只对IE6生效
    *+html *+前缀只对IE7生效
    @media screen\9{...}只对IE6/7生效
    @media \0screen {body { background: red; }}只对IE8有效
    @media \0screen\,screen\9{body { background: blue; }}只对IE6/7/8有效
    @media screen\0 {body { background: green; }} 只对IE8/9/10有效
    @media screen and (min-width:0\0) {body { background: gray; }} 只对IE9/10有效
    @media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {body { background: orange; }} 只对IE10有效等等


注意：

1. 不管是什么方法，书写的顺序都是firefox的写在前面，IE7的写在中间，IE6的写在最后面。

2. 一般情况下，我们尽量避免使用CSS hack，但是有些情况为了顾及用户体验实现向下兼容，不得已才使用hack。过多的滥用会造成html文档混乱不堪，增加管理和维护的负担

仅作参考：
https://baike.baidu.com/item/css%20hack/7026361?fr=aladdin

http://blog.csdn.net/freshlover/article/details/12132801

http://www.zhangxinxu.com/wordpress/2010/07/pie%E4%BD%BFie%E6%94%AF%E6%8C%81css3%E5%9C%86%E8%A7%92%E7%9B%92%E9%98%B4%E5%BD%B1%E4%B8%8E%E6%B8%90%E5%8F%98%E6%B8%B2%E6%9F%93/



