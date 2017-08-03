# children属性与childNodes属性的差别

 1. childNodes 它是标准属性
 返回所有的节点，包括文本节点、注释节点；
    可以通过nodeType来判断是哪种类型的节点，只有当nodeType==1时才是元素节点，2是属性节点，3是文本节点。

 有时候需要获取指定元素的第一个HTML子节点（非属性/文本节点），最容易想到的就是firstChild 属性。代码中第一个HTML节点前如果有换行，空格，那么firstChild返回的就不是你想要的了。

 2. children属性,非标准属性
  只返回元素的子元素的集合.虽然不是标准的DOM属性,但是得到了几乎所有浏览器的支持。

	使用children属性返回元素的子元素的集合。即：HTMLCollection对象。根据子元素在元素中出现的先后顺序进行排序。

	使用 HTMLCollection对象的 length属性获取子元素的数量，然后使用序列号(index，起始值为0)访问每个子元素。

注：IE6到IE8完全支持children属性，但是，返回元素节点和注释节点，IE9以上版本只返回元素节点。

