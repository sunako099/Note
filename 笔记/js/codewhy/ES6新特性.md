# 字面量的增强

属性增强、方法增强、计算属性名

```js
var name="why"
var key="address"+"city"

var obj={
    //1.属性增强
    name,   //同name:name
    //2.方法增强
    running(){
        console.log("running~") //同running:function(){...}
    }, 
    //3.计算属性名
    [key]:"广州"   //同"addresscity":"广州"
}
```

# 数组与对象解构

## 数组解构

```js
var names=["abc","cba",undefined,"nba"]
//1.基本使用
   //var name1=names[0]
   //var name2=names[1]
   //var name3=names[2]
var [name1,name2,name3]=names

//2.顺序问题，严格依照顺序，中间跳过也要留空
var [name1, ,name3]=names

//3.剩余解构为数组
var [name1,name2,...newNames]=names

//4.解构的默认值
var [name1,name2,name3="default"]=names
```

## 对象解构

```js
var obj={name:"why",age:18,height:1.88}
//1.基本使用
    //var name=obj.name
    //var age=obj.age
    //var height=obj.height
   var {name,age,height}=obj

//2.顺序问题，没有顺序，依照key解构
var {height,name,age}=obj

//3.对变量进行重命名
var {height:wHeight,name:wNAme,age:wAge}=obj

//4.默认值
var {height:wHeight,name:wNAme,age:wAge,address:wAddress="中国"}=obj

//5.剩余的解构为对象
var {name,age,...newObj}=obj
```

# **新的ECMA代码执行描述**

◼ **在执行学习JavaScript代码执行过程中，我们学习了很多ECMA文档的术语：**

​		 执行上下文栈：Execution Context Stack，用于执行上下文的栈结构；

​		 执行上下文：Execution Context，代码在执行之前会先创建对应的执行上下文；

​		 变量对象：Variable Object，上下文关联的VO对象，用于记录函数和变量声明；

​		 全局对象：Global Object，全局执行上下文关联的VO对象；

​		 激活对象：Activation Object，函数执行上下文关联的VO对象；

​		 作用域链：scope chain，作用域链，用于关联指向上下文的变量查找；

◼ **在新的ECMA代码执行描述中（ES5以及之上），对于代码的执行流程描述改成了另外的一些词汇：**

​		 基本思路是相同的，只是对于一些词汇的描述发生了改变；

​		 执行上下文栈和执行上下文也是相同的；

## **词法环境**

◼ **词法环境是一种规范类型，用于在词法嵌套结构中定义关联的变量、函数等标识符；**

​		 一个词法环境是由环境记录（Environment Record）和一个外部词法环境（outer Lexical Environment）组成；

​		 一个词法环境经常用于关联一个函数声明、代码块语句、try-catch语句，当它们的代码被执行时，词法环境被创建出来；

◼ **也就是在ES5之后，执行一个代码，通常会关联对应的词法环境；**

​		 执行上下文会关联两个词法环境：词法环境（Lexical Environments）和变量环境（Variable Environments）

### **LexicalEnvironment和VariableEnvironment**

◼ LexicalEnvironment用于处理let、const声明的标识符

◼ VariableEnvironment用于处理var和function声明的标识符

## **环境记录**

◼ **在这个规范中有两种主要的环境记录值:声明式环境记录和对象环境记录。**

​		 声明式环境记录：声明性环境记录用于定义ECMAScript语言语法元素的效果，如函数声明、变量声明和直接将标识符绑定与ECMAScript语言值关联起来的Catch子句。

​		 对象式环境记录：对象环境记录用于定义ECMAScript元素的效果，例如WithStatement，它将标识符绑定与某些对象的属性关联起来。

![词法环境](./词法环境.png)

# let/const

◼ **let关键字：**

​		 从直观的角度来说，let和var是没有太大的区别的，都是用于声明一个变量；

◼ **const关键字：**

​		 const关键字是constant的单词的缩写，表示常量、衡量的意思；

​		 它表示保存的数据一旦被赋值，就不能被修改；

​		 但是如果赋值的是引用类型，那么可以通过引用找到对应的对象，修改对象的内容；比如可以修改其属性（如果它是一个对象）或元素（如果它是一个数组）

◼ 注意：

​		 另外let、const不允许重复声明变量；

## **暂时性死区 (TDZ)**

◼ **我们知道，在let、const定义的标识符真正执行到声明的代码之前，是不能被访问的**

​		 从块作用域的顶部一直到变量声明完成之前，这个变量处在暂时性死区（TDZ，temporal dead zone）

◼ 使用术语 “temporal” 是因为区域取决于执行顺序（时间），而不是编写代码的位置；

```js
function foo(){
    console.log(message)
}
let message="HW"
foo()
```

**let/const有没有作用域提升呢？**

◼ **在执行上下文的词法环境创建出来的时候，变量事实上已经被创建了，只是这个变量是不能被访问的。**

​		 那么变量已经有了，但是不能被访问，是不是一种作用域的提升呢？

◼ **事实上维基百科并没有对作用域提升有严格的概念解释，那么我们自己从字面量上理解；**

​		 **作用域提升：**在声明变量的作用域中，如果这个变量可以在声明之前被访问，那么我们可以称之为作用域提升；

​		 在这里，它虽然被创建出来了，但是不能被访问，我认为不能称之为作用域提升；

◼ 所以我的观点是let、const没有进行作用域提升，但是会在解析阶段被创建出来。

**使用 let 或 const 声明的变量会被存储在词法环境中，使用 var 声明的变量一样直接添加到全局对象（例如 window）上。**

## 块级作用域

ES5时，只有两个作用域：**全局作用域和函数作用域**

ES6中新增了**块级作用域**，并且通过let、const、function、class声明的标识符是具备块级作用域的限制的：

```js
 {
    let foo="foo"
    function bar(){
        console.log("bar")
    }
    class Person{}
 }
 console.log(foo) //报错
 bar() //可以访问
 var p=new Person() //报错
```

◼ 但是我们会发现函数拥有块级作用域，但是外面依然是可以访问的：

​		 这是因为**引擎会对函数的声明进行特殊的处理，允许像var那样进行提升**；

