# HTML5离线缓存

专门为开发离线 Web 应用而设计的，HTML5离线缓存又叫Application Cache，是从浏览器的缓存中分出来的一块缓存区，
如果要在这个缓存中保存数据，可以使用一个描述文件（manifest file），列出要下载和缓存的资源。

 缓存机制：访问app cache时直接返回缓存资源，然后检测server上的manifest文件是否更新，更新的第一次不生效，刷新后更新（会拉取所有文件）
 一次是更新app cache，一次是更新页面内容。

## 配置
1. Manifest文件格式：
	
		CACHE MANIFEST
    	#需要缓存的文件
    	CACHE:
    	ajax.js
   	 	#每次需要拉取的文件
    	NETWORK:
    	test.html
    	#离线状态下代替的文件
    	FALLBACK:
    	/ajax/ ajax.html

* CACHE MANIFEST 在此标题下列出的文件将在首次下载后进行缓存
* NETWORK 在此标题下列出的文件需要与服务器的连接，且不会被缓存
* FALLBACK 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）
* # 注释

2.在html引用manifest文件（需要同源）

    	<html manifest="sample.appcache">

3. 在服务器添配置mine-type，即text/cache-manifest

  比如tomcat服务器，在conf/web.xml中添加

    	<mime-mapping>
          <extension>appcache</extension>
          <mime-type>text/cache-manifest</mime-type>
    	</mime-mapping>

4. 在浏览器控制台输出下面一堆，说明缓存成功

  		Creating Application Cache with manifest http://127.0.0.1:8080/Test/app.appcache
  		Application Cache Checking event
  		Application Cache Downloading event
  		Application Cache Progress event (0 of 1) http://127.0.0.1:8080/Test/Ajax.js
  		Application Cache Progress event (1 of 1)
  		Application Cache Cached event


## 更新缓存
  * 修改server端manifest文件名，直接删除html中的配置不能生效
  * 用户清空浏览器缓存，这会重新下载文件
  * manifest 文件被修改
  * 由程序来更新应用缓存

    window.applicationCache.update();

## 优势和缺陷
1. 优势：

 * 离线浏览：完全缓存,无网络环境下可以继续使用；
 * 速度：已缓存资源加载得更快；
 * 降低服务器负载:浏览器将只从服务器下载更新过或更改过的资源。

2. 缺陷：

 * 重复缓存含参数页，index和index?a=1会被认为是不同的页面，分别被缓存一次
 * 无法更新某个单独的文件；
 * 更改manifest才会认为是更新，并且要刷新两次才能获取最新资源；
 * 含有manifest属性的请求页无论如何都会被缓存

### 使用场景
1. 单地址页面（不带参数）；
2. 实时性要求不高的业务；
3. 离线webapp；
4. 字体、图片

参考:

http://www.cnblogs.com/hutuzhu/p/4871666.html



