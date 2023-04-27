# this

 1.函数在调用时，JavaScript会默认给this绑定一个值；

 2.this的绑定**和定义的位置（编写的位置）没有关系**；

 3.this的绑定和调用方式以及调用的位置有关系；

 4.this是**在运行时被绑定**的；

## 默认绑定

◼ **什么情况下使用默认绑定呢？独立函数调用。**

​		 独立的函数调用我们可以理解成函数没有被绑定到某个对象上进行调用；

==**注意：一般指向全局window。但严格模式下，this会指向undefined。**==

## 隐式绑定

◼ 另外一种比较常见的调用方式是通过某个对象进行调用的：

​		也就是它的调用位置中，是通过某个对象发起的函数调用。

## new绑定

回顾JS基础笔记：

◼ **如果一个函数被使用new操作符调用了，那么它会执行如下操作：**

​		 1. 在内存中创建一个新的对象（空对象）；

​		 2. 这个对象内部的[[prototype]]属性会被赋值为该构造函数的prototype属性

​		 3. 构造函数内部的this，**会指向创建出来的新对象**；

​		 4. 执行函数的内部代码（函数体代码）；

​		 5. 如果构造函数没有返回非空对象，则返回创建出来的新对象

## 显式绑定

◼ **隐式绑定有一个前提条件：**

​		 必须在调用的对象内部有一个对函数的引用（比如一个属性）；

​		 如果没有这样的引用，在进行调用时，会报找不到该函数的错误；

​		 正是通过这个引用，间接的将this绑定到了这个对象上；

◼ **如果我们不希望在 对象内部包含这个函数的引用，同时又希望在这个对象上进行强制调用，可以用apply、call、bind**

**◼ apply和call**

​		 第一个参数是相同的，要求传入一个对象；

​				✓ 这个对象的作用是什么呢？就是给this准备的。

​				✓ 在调用这个函数时，会将this绑定到这个传入的对象上。

​		 后面的参数，apply为数组，call为参数列表；

```js
function.apply(thisArg,[argsArray])
function.call(thisArg,arg1,arg2,...)
```

◼ **bind**

**会让一个函数总是显示的绑定到一个对象上**

​		 使用bind方法，bind() 方法创建一个新的绑定函数（bound function，BF）；

​		 绑定函数是一个 exotic function object（怪异函数对象，ECMAScript 2015 中的术语）

​		 在 bind() 被调用时，这个**新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。**

​		 后续再调用传参，不会改写，只会往后传参

## 内置函数的绑定

根据经验判断、、

内置函数：些JavaScript的内置函数，或者一些第三方库中的内置函数。这些函数会要求我们传入另一个函数。比如setTimeout、数组的forEach、div的点击。这些函数中的this根据经验判断指向。

## 绑定规则优先级

◼ **1.默认规则的优先级最低**

◼ **2.显示绑定优先级高于隐式绑定**

◼ **3.new绑定优先级高于隐式绑定**

◼ **4.new绑定优先级高于bind**

​		 new绑定和call、apply是不允许同时使用的，所以不存在谁的优先级更高

​		 new绑定可以和bind一起使用，new绑定优先级更高

### this规则之外

◼ **情况一：如果在显示绑定中，我们传入一个null或者undefined，那么这个显示绑定会被忽略，使用默认规则：**

```js
function foo(){
    console.log(this)
}
var obj={
    name:"why"
}
foo.call(null)   //window
foo.call(undefined)  //window
```

◼ **情况二：创建一个函数的间接引用，这种情况使用默认绑定规则。**

​		 赋值(obj2.foo = obj1.foo)的结果是foo函数；

​		 foo函数被直接调用，那么是默认绑定；

```js
function foo(){
    console.log(this)
}
var obj={
    name:"obj1"
    foo:foo
}
var obj2={
    name:"obj2"
}
obj1.foo()   //obj1对象
(obj2.foo=obj1.foo)()   //window
```

◼ **情况三：箭头函数**

 就没有this，用显式绑定也没有用

# 箭头函数

 箭头函数不会绑定this、arguments属性；

 箭头函数不能作为构造函数来使用（不能和new一起来使用，会抛出错误）；

## 编写优化

◼ **优化一: 如果只有一个参数()可以省略**

`nums.forEach(item=>{})`

◼ **优化二: 如果函数执行体中只有一行代码, 那么可以省略大括号**

​		 并且这行代码的返回值会作为整个函数的返回值

`nums.forEach(item => console.log(item))`

`nums.filter(item => true)`

◼ **优化三: 如果函数执行体只有返回一个对象, 那么需要给这个对象加上()**

```js
var foo=()=>{
    return {name:"abc"}
}
var bar=()=>({name:"abc"})
```

## 场景示例

```js
//模拟网络请求
function request(url,callbackFn){
  var results=["abc","buc","fds"]
  callbackFn(results)   //不做处理的话，这里单独调用this会指向全局
}

var obj={
  names:[],
  network:function(){
    //早期解决办法
    // var _this=this
    // request("/names",function(res){
    //   _this.names=[].concat(res)
    // })
    //箭头函数解决办法
    request("/names",(res)=>{
      this.names=[].concat(res)
    })
  }
}

obj.network();
```

```js
//定时器中的回调函数
var obj={
  data:[],
  getData:function () {
      setTimeout(()=>{
        var res=["abc","vdx","sdd"]
        this.data.push(...res)
      },1000)
    }
}
//如果getData也是一个箭头函数，this则指向window
var obj={
  data:[],
  getData:()=> {
      setTimeout(()=>{
        console.log(this)  //this
      },1000)
    }
}
```

