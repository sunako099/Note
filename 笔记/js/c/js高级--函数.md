# **函数对象的属性**

◼ **我们知道JavaScript中函数也是一个对象，那么对象中就可以有属性和方法。**

​		◼ 属性name：一个函数的名词我们可以通过name来访问；

```js
function foo(){
}
console.log(foo.name)
```

​		◼ 属性length：属性length用于返回函数参数的个数；

​				 注意：rest参数是不参与参数的个数的；

```js
var baz=(name,age,...args)=>{
}
console.log(baz.length)
```

# **arguments**

◼ **arguments** 是一个 对应于 **传递给函数的参数** 的 **类数组(array-like)对象**。

```js
funxtion foo(x,y,z){
    //[Arguments] {'0':10,'1':20,'2':30}
	console.log(arguments)
}
foo(10,20,30)
```

◼ array-like意味着它不是一个数组类型，而是一个对象类型：

​		 但是它却拥有数组的一些特性，比如说length，比如可以通过index索引来访问；

​		 但是它却没有数组的一些方法，比如filter、map等；

```js
console.log(arguments.length)
console.log(aeguments[0])
```

## **arguments转Array**

◼ **转化方式一：**

​		 遍历arguments，添加到一个新数组中；

```js
var length=arguments.length
var arr=[]
for(var i=0;i<length;i++){
  arr.push(arguments[i])
}
console.log(arr)
```

◼ **转化方式二：较难理解（有点绕），了解即可**

​		 调用数组slice函数的call方法；

```js
var arr1=Array.prototype.slice.call(arguments)
var arr2=[].slice.call(arguments)
console.log(arr1)
console.log(arr2)
```

◼ **转化方式三：ES6中的两个方法**

​		 Array.from

​		 […arguments]

```javascript
const arr3=Array.from(arguments)
const arr4=[...arguments]
console.log(arr3)
console.log(arr4)
```

## **箭头函数不绑定arguments**

◼ **箭头函数是不绑定arguments的，所以我们在箭头函数中使用arguments会去上层作用域查找：**

```js
console.log(arguments)//全局没有arguments
var foo=(x,y,z)=>{
  console.log(arguments)  //arguments is not defined
}
foo(10,20,30)
```

# **函数的剩余（rest）参数**

◼ **ES6中引用了rest parameter，可以将不定数量的参数放入到一个数组中：**

​		 如果最后一个参数是 ... 为前缀的，那么它会将剩余的参数放到该参数中，并且作为一个**数组**；

```js
function foo(m,n,...args){
  console.log(args)
}
```

◼ **那么剩余参数和arguments有什么区别呢？**

​		 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参；

​		 **arguments对象不是一个真正的数组，而rest参数是一个真正的数组**，可以进行数组的所有操作；

​		 arguments是早期的ECMAScript中为了方便去获取所有的参数提供的一个数据结构，而rest参数是ES6中提供并且希望以此来替代arguments的；

◼ **剩余参数必须放到最后一个位置，否则会报错。**

# **JavaScript纯函数**

** 确定的输入，一定会产生确定的输出；**

** 函数在执行过程中，不能产生副作用；**

副作用?

​	表示在执行一个函数时，除了返回函数值之外，还对调用函数产生了附加的影响，比如修改了全局变量，修改参数或者改变外部的存储；

**示例**

​		 slice：slice截取数组时不会对原数组进行任何操作,而是生成一个新的数组；

​		 splice：splice截取数组, 会返回一个新的数组, 也会对原数组进行修改；

slice就是一个纯函数，不会修改数组本身，而splice函数不是一个纯函数

**作用与优势**

​		 因为你可以安心的编写和安心的使用；

​		 你在**写的时候**保证了函数的纯度，只是单纯实现自己的业务逻辑即可，不需要关心传入的内容是如何获得的或者依赖其他的外部变量是否已经发生了修改；

​		 你在**用的时候**，你确定你的输入内容不会被任意篡改，并且自己确定的输入，一定会有确定的输出；

◼ React中就要求我们无论是**函数还是class声明一个组件**，这个组件都必须**像纯函数一样**，**保护它们的props不被修改**

# **柯里化**

 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余的参数；

 这个过程就称之为柯里化；

就是一种函数的转换，将一个函数从可调用的 f(a, b, c) 转换为可调用的 f(a)(b)(c)。

 柯里化不会调用函数。它只是对函数进行转换。

**优势**：

**函数的职责单一**

**函数的参数复用**

自动柯里化函数封装

```js
function hyCurrying(fn){
  function curried(...args){
    if(args.length>=fn.length){
      return fn.apply(this,args)
    }else{
      return function (...args2) {
        return curried.apply(this,args.concat(args2))  
      }
    }
  }
  return curried
}
```

# 组合函数

◼ **组合（Compose）函数**是在JavaScript开发过程中一种对**函数的使用技巧、模式**：

​		 比如我们现在需要对某一个数据进行函数的调用，执行两个函数fn1和fn2，这两个函数是依次执行的；

​		 那么如果每次我们都需要进行两个函数的调用，操作上就会显得重复；

​		 那么是否可以将这两个函数组合起来，自动依次调用呢？

​		 这个过程就是对函数的组合，我们称之为 组合函数（Compose Function）；

# With语句

◼ **with语句** 扩展一个语句的作用域链。

◼ 不建议使用with语句，因为它可能是混淆错误和兼容性问题的根源

# **eval函数**

◼ **内建函数 eval 允许执行一个代码字符串。**

​		 eval是一个特殊的函数，它可以将传入的字符串当做JavaScript代码来运行；

​		 eval会将最后一句执行语句的结果，作为返回值；

◼ **不建议在开发中使用eval：**

​		 eval代码的可读性非常的差（代码的可读性是高质量代码的重要原则）；

​		 eval是一个字符串，那么有可能在执行的过程中被刻意篡改，那么可能会造成被攻击的风险；

​		 eval的执行必须经过JavaScript解释器，不能被JavaScript引擎优化；

# 严格模式

◼ **那么如何开启严格模式呢？use strict：**

​		 可以支持在js文件中开启严格模式；

​		 也支持对某一个函数开启严格模式；

◼ **没有类似于 "no use strict" 这样的指令可以使程序返回默认模式。**

​		 现代 JavaScript 支持 “class” 和 “module” ，它们会自动启用 use strict；

**限制**

◼ **1. 无法意外的创建全局变量**

◼ **2. 严格模式会使引起静默失败(silently fail,注:不报错也没有任何效果)的赋值操作抛出异常**

◼ **3. 严格模式下试图删除不可删除的属性**

◼ **4.严格模式不允许函数参数有相同的名称**

◼ **5. 不允许0的八进制语法**

◼ **6. 在严格模式下，不允许使用with**

◼ **7. 在严格模式下，eval不再为上层引用变量**

◼ **8. 严格模式下，this绑定不会默认转成对象**