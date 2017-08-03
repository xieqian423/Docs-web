# NodeList的使用
理解 NodeList 及其“近亲” NamedNodeMap 和 HTMLCollection。这三个集合都是“动态的”，每当文档结构发生变化时，它们都会得到更新。

下面的代码将会导致死循环：

    var divs = document.getElementsByTagName("div"),
    i,
    div;
    for(i=0;i<divs.length;i++){
        div = document.createElement("div");
        document.body.appendChild(div);
    }
因为每次循环都要对条件 i < divs.length 求值，divs.length不断增加，导致死循环。为了避免死循环，先将 divs.length在循环开始时保存起来：

    var divs = document.getElementsByTagName("div"),
    i,
    len,
    div;
    for(i=0,len=divs.length; i<len; i++){
        div = document.createElement("div");
        document.body.appendChild(div);
    }

应该尽量减少对NodeList的访问，因为每次访问 NodeList，都会运行一次基于文档的查询。



























