#  document的createDocumentFragment()方法

在所有节点类型中，只有DocumentFragment 在文档中没有对应的标记。可以包含和控制节点，继承了 Node 的所有方法，但不会像其他节点文档那样占用额外的资源

 1. 可以将它作为一个“仓库”来使用，在里面保存将来可能会添加到文档中的节点。添加到文档片段中的新节点不属于文档树。

 2. 如果将文档中的节点添加到文档片段中，就会从文档树中移除该节点，也不会从浏览器中再看到该节点。

 3. 可以通过 appendChild() 或 insertBefore() 将文档片段中内容添加到文档中。

但是当要向document中添加大量数据时，如果逐个地添加，将会导致浏览器反复渲染（呈现）新信息。
为避免这个问题，可以像下面这样使用一个文档片段来保存创建的节点，然后再一次性添加到文档中。

      var fragment = document.createDocumentFragment();
      var ul = document.getElementById("myList");
      var li = null;
      for (var i=0; i < 3; i++){
      li = document.createElement("li");
      li.appendChild(document.createTextNode("Item " + (i+1)));
      fragment.appendChild(li);
      }
      ul.appendChild(fragment);