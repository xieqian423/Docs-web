# DOM节点的操作
DOM操作都会及时动态更新节点，所取得的都是最新的结构。虽然DOM树是通过指针连接的，但是同一个节点并不能同时出现在多个位置上

## 增、删、改
下面四个方法都必须先取得父节点，在不支持子节点的节点上调用这些方法，将会导致错误发生
1. appendChild(),用于向childNodes列表的末尾添加一个节点
看下面的两个例子：

    var node = somenode.appendChild(newnode); 在父元素的子节点的最后插入一个新的节点
    alert(node == newnode); //true
    alert(newnode == somenode.lastChild); //true

    var node = somenode.appendChild(somenode.firstChild); 将父元素的第一个节点移到最后一个节点的位置上
    alert(node == somenode.firstChild); //false    此时第一个节点的位置以更新
    alert(node == somenode.lastChild); //true

2. insertBefore() 这个方法接受两个参数：要插入的节点和作为参照的节点。如果参照节点是null ，则 insertBefore() 与 appendChild() 执行相同的操作

    //插入后成为最后一个节点
    var node = somenode.insertBefore(newnode,null);
    alert(node == somenode.lastChild); //true

    //插入后成为第一个子节点
    var node = somenode.insertBefore(newnode,somenode.firstChild);
    alert(node == somenode.firstChild); //true

    //插入到最后一个子节点前面
    var node = somenode.insertBefore(newnode,somenode.lastChild);
    alert(node == somenode.childNodes[somenode.childNodes.length-2]); //true

3. replaceChild()  方法接受的两个参数是： 要插入的节点和要替换的节点，并返回被替换的节点。新的节点的关系指针都会从被替换的节点中复制过来，被替换的节点从文档树中被移除，但仍然在文档中，只是没有了位置

    //替换第一个子节点
    var node = somenode.replaceChild(newnode, somenode.firstChild);

4. removeChild() 该方法移除的节点也是仍然为文档所有，只不过在文档中已经没有了自己的位置

    //移除第一个子节点
    var formerFirstChild = somenode.removeChild(somenode.firstChild);

5.其他方法
所有类型的节点都有的
cloneNode() 接受一个布尔值参数，表示是否执行深复制。
    true执行深复制，也就是复制节点及其整个子节点树；false，执行浅复制，即只复制节点本身，不包含子节点。复制后返回的节点副本属于文档所有，但并没有为它指定父节点。
不会复制添加的js属性，比如事件处理程序


## 查询及遍历

### 选择符查询
所有的JavaScript库的选择符查询，都会写一个基础的CSS解析器，然后再使用已有的。
把这个功能变成原生 API 之后，解析和树查询操作可以在浏览器内部通过编译后的代码来完成，极大地改善了性能。

* querySelector() 返回第一个元素对象
* querySelectAll() 返回NodeList

这两个方法参数都是CSS选择符，可以通过 Document 及 Element 类型的实例调用它们。传入不支持的选择符会有语法错误。

### 遍历

对于元素间的空格， IE9及之前版本不会返回文本节点， 而其他所有浏览器都会返回文本节点。 这样，
就导致了在使用 childNodes 和 firstChild 等属性时的行为不一致。为了弥补这一差异，而同时又保
持 DOM规范不变，Element Traversal API 为 DOM 元素添加了以下 5 个属性：

* childElementCount ：返回子元素（不包括文本节点和注释）的个数。
* firstElementChild ：指向第一个子元素； firstChild 的元素版。
* lastElementChild ：指向最后一个子元素； lastChild 的元素版。
* previousElementSibling ：指向前一个同辈元素； previousSibling 的元素版。
* nextElementSibling ：指向后一个同辈元素； nextSibling 的元素版。

支持 Element Traversal 规范的浏览器有 IE 9+、Firefox 3.5+、Safari 4+、Chrome 和 Opera 10+
利用这些元素不必担心空白文本节点，从而可以更方便地查找 DOM元素了。