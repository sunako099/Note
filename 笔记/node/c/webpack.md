# **内置模块path**

◼ **从路径中获取信息**

​		 dirname：获取文件的父文件夹；

​		 basename：获取文件名；

​		 extname：获取文件扩展名；

◼ **路径的拼接：path.join**

​		 如果我们希望将多个路径进行拼接，但是不同的操作系统可能使用的是不同的分隔符；

​		 这个时候我们可以使用path.join函数；

◼ **拼接绝对路径：path.resolve**

​		 path.resolve() 方法会把一个路径或路径片段的序列解析为一个绝对路径；

​		 给定的路径的序列是从右往左被处理的，后面每个 path 被依次解析，直到构造完成一个绝对路径；

​		 如果在处理完所有给定path的段之后，还没有生成绝对路径，则使用当前工作目录；

​		 生成的路径被规范化并删除尾部斜杠，零长度path段被忽略；

​		 如果没有path传递段，path.resolve()将返回当前工作目录的绝对路径；

# **Webpack**

◼ **webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序；**

◼ 我们来对上面的解释进行拆解：

​		 **打包bundler**：webpack可以将帮助我们进行打包，所以它是一个打包工具

​		 **静态的static**：这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）；

​		 **模块化module**：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等；

​		 **现代的modern**：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发展；

## **Webpack的安装**

◼ **webpack的安装目前分为两个：webpack、webpack-cli**

◼ **那么它们是什么关系呢？**

​		 执行webpack命令，会执行node_modules下的.bin目录下的webpack；

​		 webpack在执行时是依赖webpack-cli的，如果没有安装就会报错；

​		 而webpack-cli中代码执行时，才是真正利用webpack进行编译和打包的过程；

​		 所以在安装webpack时，我们需要同时安装webpack-cli（第三方的脚手架事实上是没有使用webpack-cli的，而是类似于自己的vue-service-cli的东西）

`npm install webpack webpack-cli -D`

## 基本使用

◼ 第一步：创建package.json文件，用于管理项目的信息、库依赖等

`npm init`

◼ 第二步：安装局部的webpack

`npm install webpack webpack-cli -D`

◼ 第三步：使用局部的webpack

`npx webpack`

◼ 第四步：在package.json中创建scripts脚本，执行脚本打包即可

```js
"scripts":{
	"build":"webpack"
}
```

`npm run build`