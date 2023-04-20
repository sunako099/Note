### express下载

npm init

npm i express

# Express路由

定义：路由确定了应用程序如何响应客户端对特定端点的请求

一个路由的组成有 **==请求方法==** ， **==路径==** 和 **==回调函数==** 组成

### 简单使用

```js
//导入 express
const express = require('express');
//创建应用对象
const app = express();
//创建 get 路由
app.get('/home', (req, res) => {
res.send('网站首页');
});
//首页路由
app.get('/', (req,res) => {
res.send('我才是真正的首页');
});
//创建 post 路由
app.post('/login', (req, res) => {
res.send('登录成功');
});
//匹配所有的请求方法
app.all('/search', (req, res) => {
res.send('1 秒钟为您找到相关结果约 100,000,000 个');
});
//自定义 404 路由
app.all("*", (req, res) => {
res.send('<h1>404 Not Found</h1>')
});
//监听端口 启动服务
app.listen(3000, () =>{
console.log('服务已经启动, 端口监听为 3000');
});
```

## 获取请求参数

**req.query  获取查询字符串**

**req.get('host')   获取指定的请求头**

**获取路由参数: **    路由参数指的是 URL 路径中的参数（数据）

```js
app.get('/:id.html', (req, res) => {
res.send('商品详情, 商品 id 为' + req.params.id);
});
```



```js
app.get('/request', (req, res) => {
//1. 获取报文的方式与原生 HTTP 获取方式是兼容的
console.log(req.method);
console.log(req.url);
console.log(req.httpVersion);
console.log(req.headers);

//2. express 独有的获取报文的方式
//获取查询字符串
console.log(req.query); // 『相对重要』
//获取指定的请求头
console.log(req.get('host'));

res.send('请求报文的获取');

});
```

## 获取请求体数据

以使用 body-parser 包处理请求体

1. 安装

   `npm i body-parser`

2. 导入 body-parser 包

   `const bodyParser = require('body-parser');`

3. 获取中间件函数

   ```js
   //处理 querystring 格式的请求体
   let urlParser = bodyParser.urlencoded({extended:false}));
   
   //处理 JSON 格式的请求体
   let jsonParser = bodyParser.json();
   ```

4. 设置路由中间件，然后使用 request.body 来获取请求体数据

   ```js
   app.post('/login', urlParser, (request,response)=>{
   //获取请求体数据
   //console.log(request.body);
   
   //用户名
   console.log(request.body.username);
   
   //密码
   console.log(request.body.userpass);
   response.send('获取请求体数据');
   
   });
   ```

   获取到的请求体数据：

   `[Object: null prototype] { username: 'admin', userpass: '123456' }`

## 设置响应配置

```js
//获取请求的路由规则
app.get("/response", (req, res) => {
//1. express 中设置响应的方式兼容 HTTP 模块的方式
res.statusCode = 404;
res.statusMessage = 'xxx';
res.setHeader('abc','xyz');
res.write('响应体');
res.end('xxx');
//2. express 的响应方法
res.status(500); //设置响应状态码
res.set('xxx','yyy');//设置响应头
res.send('中文响应不乱码');//设置响应体
//连贯操作
res.status(404).set('xxx','yyy').send('你好朋友')
//3. 其他响应
res.redirect('http://atguigu.com')//重定向
res.download('./package.json');//下载响应
res.json();//响应 JSON
res.sendFile(__dirname + '/home.html') //响应文件内容
});
```

## 

# Express中间件

本质：==**一个回调函数**==，可以像路由回调一样访问 请求对象（request） ， 响应对象（response）。

作用：使用函数封装公共操作，简化代码。

### 全局中间件

1. 声明中间件函数

   ```js
   let recordMiddleware = function(request,response,next){
   //实现功能代码
   //.....
   //执行next函数(当如果希望执行完中间件函数之后，仍然继续执行路由中的回调函数，必须调用next)
   next();
   }
   ```

2. 应用中间件

   ```js
   app.use(recordMiddleware);
   ```

声明时可以直接将匿名函数传递给 use:

```js
app.use(function (request, response, next) {
console.log('定义第一个中间件');
next();
})
```

### 路由中间件

调用格式如下：

```js
app.get('/路径',`中间件函数1`,`中间件函数2`,(request,response)=>{
});
```

### 静态资源中间件

app.use(express.static('静态资源文件夹'));

简单使用：

```js
//引入express框架
const express = require('express');
//创建服务对象
const app = express();
//静态资源中间件的设置，将当前文件夹下的public目录作为网站的根目录
app.use(express.static('./public')); //当然这个目录中都是一些静态资源
//如果访问的内容经常变化，还是需要设置路由
//但是，在这里有一个问题，如果public目录下有index.html文件，单独也有index.html的路由，
//则谁书写在前，优先执行谁
app.get('/index.html',(request,response)=>{
respsonse.send('首页');
});
//监听端口
app.listen(3000,()=>{
console.log('3000 端口启动....');
});
```

> 注意事项:
> 1. index.html 文件为默认打开的资源
> 2. 如果静态资源与路由规则同时匹配，谁先匹配谁就响应
> 3. 路由响应动态资源，静态资源中间件响应静态资源

# Router

express 中的 Router 是一个完整的中间件和路由系统，可以看做是一个小型的 app 对象。
作用：对路由进行模块化，更好的管理路由

### 简单使用

创建独立的 JS 文件（homeRouter.js）

```js
//1. 导入 express
const express = require('express');
//2. 创建路由器对象
const router = express.Router();
//3. 在 router 对象身上添加路由
router.get('/', (req, res) => {
res.send('首页');
})
router.get('/cart', (req, res) => {
res.send('购物车');
});
//4. 暴露
module.exports = router;
```

主文件

```js
const express = require('express');
const app = express();
//5.引入子路由文件
const homeRouter = require('./routes/homeRouter');
//6.设置和使用中间件
app.use(homeRouter);
app.listen(3000,()=>{
console.log('3000 端口启动....');
})
```

### 设置路由前缀

主文件

`app.use('/users', usersRouter);`

routes/users.js

```js
router.get('/test', function(req, res, next) {
 res.send('respond with a resource');
});   //在访问127.0.0.1/users/test呈现该内容
```



# EJS 模板引擎

模板引擎是分离 **用户界面**和**业务数据** (h5和服务端js)的一种技术

### 简单使用

1. 下载安装

   `npm i ejs --save`

2. ```js
   //1.引入ejs
   const ejs = require('ejs');
   //2.定义数据
   let person = ['张三','李四','王二麻子'];
   //3.ejs解析模板返回结构
   //<%= %> 是ejs解析内容的标记，作用是输出当前表达式的执行结构
   let html = ejs.render(‘<%= person.join(",") %>’, {person:person});
   //4.输出结果
   console.log(html);
   ```

## EJS常用语法

执行JS代码

`<% code %>`

输出转义数据到模板

`<%= code %>`

输出非转义数据到模板

`<%- code %>`