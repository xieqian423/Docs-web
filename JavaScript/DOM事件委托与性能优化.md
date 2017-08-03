# DOM事件委托及性能优化

## 事件委托
事件委托就是利用事件冒泡的原理，将子元素的事件处理尽量将事件绑定在父级元素上。

如下，对列表项绑定事件：

    var list = document.getElementById("myLinks");
    EventUtil.addHandler(list, "click", function(event){
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);
        switch(target.id){
            case "doSomething":
                document.title = "I changed the document's title";
                break;
            case "goSomewhere":
                location.href = "http://www.wrox.com";
                break;
            case "sayHi":
                alert("hi");
                break;
        }
    });

与传统事件绑定相比其优势：

 1. document对象很快就可以访问，可以在页面生命周期的任何时点上为它添加事件处理程序
 2. 所需的 DOM引用更少，减少dom的查询。
 3. 减少函数的创建，整个页面占用的内存空间更少，能够提升整体性能

最适合采用事件委托技术的事件包括 click 、 mousedown 、 mouseup 、 keydown 、 keyup 和 keypress 。
虽然 mouseover 和 mouseout 事件也冒泡， 但要适当处理它们并不容易， 而且经常需要计算元素的位置

## 移除事件
当移除元素时，浏览并不会做出正确的处理同时移除事件，其绑定事件很有可能还留在内存中。
移除元素时，由于事件与元素保持着引用关系，元素也依然保存在内存中。

注意：

 1. 在使用innerHTML或者outerHTML删除元素前，要先手工移除事件处理程序。
 2. 页面卸载或者来回切换回，“刷新”等操作，也要先移除事件处理程序。

事件委托技术使得需要跟踪的事件处理程序更少，移除它们就更容易。