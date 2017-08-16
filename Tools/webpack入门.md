# webpack入门

1. 安装全局包

   $ npm install webpack -g    全局安装
   $ npm install webpack --save-dev  局部安装
局部安装是官方推荐的做法， 不要npm install webpack -g 因为这样会影响以后的项目。

2. 使用npm init初始化，生成package.json文件

进入项目根目录，执行 $ npm init

关于package.json属性的说明参照npm官方网站，这里关于其[属性的简要介绍](http://www.cnblogs.com/zourong/p/5943224.html)

3. 添加包及插件

这里需要使用React，所以先安装它，--save命令用于将包添加至package.json文件中

   $ npm install react --save
   $ npm install react-dom --save
用这种方式添加依赖的包

4. 创建必要的文件
webpack支持CommonJs、AMD及ES6的模块管理

5. webpack打包
 * 在当前项目的package.json里面的scripts里面加一句 "webpack":"webpack  app.js  app.bundle.js"
 * 然后在命令行敲 npm run webpack 就行了
 * npm run webpack 就相当于之前的 webpack  app.js  app.bundle.js

参数
 --watch 自动更新打包
   webpack  app.js  app.bundle.js --watch

 --progress 查看打包情况
 --display-modules 查看打包模块

6. css的模块处理

 * 安装 $npm install css-loader style-loader --save-dev
 * 在js文件头部引用 require('style-loader!css-loader .\style.css')
 * 运行 $webpack hello.js hello.bundle.js
 或者
 * 绑定参数 require('.\style.css')
 * $webpack hello.js hello.bundle.js --module-bind "css=style-loader!css-loader"


7. webpack的配置文件webpack.config.js

官方文档有[详细的配置介绍](https://webpack.js.org/configuration/

   module.exports = {
     entry: "./index.js",  //入口文件，从哪个文件开始打包
     output: {
       path: "./dist/js",  //打包文件放置的目录
       filename: "bundle.js"                 //打包后的文件
     }
   }


