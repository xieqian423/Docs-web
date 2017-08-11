# jQuery中bind，live，delegate，on绑定事件的方式与区别
jQuery中提供了四种事件监听方式，分别是bind、live、delegate、on，
对应的解除监听的函数分别是unbind、die、undelegate、off。

* type:事件类型，如click、change、mouseover等;
* data:传入监听函数的参数，通过event.data取到。可选;
* function:监听函数，可传入event对象，这里的event是jQuery封装的event对象，与原生的event对象有区别，使用时需要注意。

## bind
	$('a').bind(type,[data],function(eventObject))
bind是使用频率较高的一种，作用就是在选择到的元素上绑定特定事件类型的监听函数。对于动态添加的属于匹配到的元素，不会被触发事件的

内部调用了on方法,源码:

    bind: function( types, data, fn ) {
        return this.on( types, null, data, fn );
    }


## live
	$( "#members li a" ).live(type, [data], fn)
live利用了事件委托机制来完成事件的监听处理，把节点的处理委托给了document。

此方法在jQuery1.7的时候已经废除,event.stopPropagation()不再有效了，因为事件已经委托到了document上了

源码：

    live: function( types, data, fn ) {
        jQuery( this.context ).on( types, this.selector, data, fn );
        return this;
    }
live方法并没有将监听器绑定到自己(this)身上，而是绑定到了this.context上了

## delegate
利用事件委托，可以选择委托的对象

    $( "#members" ).delegate( "li a", "click", function( e ) {} );

源码:

    delegate: function( selector, types, data, fn ) {
        return this.on( types, selector, data, fn );
    }

## on

    $( "#members" ).on(type,[selector],[data],fn)

  参数与delegate差不多但还是有细微的差别，首先type与selector换位置了，其次selector变为了可选项。
传selector进去，就是跟delegate一样的意义了，除了参数顺序不同，其他完全一样。


#### 参考
http://blog.csdn.net/hi_kevin/article/details/37757657