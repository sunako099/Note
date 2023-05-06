# **Iterator迭代器**

**迭代器是帮助我们对某个数据结构进行遍历的对象**

◼ **在JavaScript中，==迭代器也是一个具体的对象==，这个对象需要符合迭代器协议（iterator protocol）：**

​		 迭代器协议定义了产生一系列值（无论是有限还是无限个）的标准方式；

​		 在JavaScript中这个标准就是一个==特定的next方法==；

◼ **next方法有如下的要求：**

​		 一个无参数或者一个参数的函数，返回一个应当拥有以下两个属性的对象：

​		** done（boolean）**

​				✓ 如果迭代器可以产生序列中的下一个值，则为 false。（这等价于没有指定 done 这个属性。）

​				✓ 如果迭代器已将序列迭代完毕，则为 true。这种情况下，value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值。

​		** value**

​				✓ 迭代器返回的任何 JavaScript 值。done 为 true 时可省略（值为undefined。

```js
const friends=['suanko','rin','len']

function createArrayIterator(arr){
  let index=0
  return {
    next:function(){
      if(index<arr.length){
        return{done:false,value:arr[index++]}
      }else{
        return{done:true}
      }
    }
  }
}

const friendsIterator=createArrayIterator(friends)
console.log(friendsIterator.next())
console.log(friendsIterator.next())
console.log(friendsIterator.next())
console.log(friendsIterator.next())
```

## 可迭代对象

◼ **什么是可迭代对象呢？**

​		 它和迭代器是不同的概念；

​		 当一个对象实现了iterable protocol协议时，它就是一个可迭代对象；

​		 **这个对象的要求是必须实现 @@iterator 方法，在代码中我们使用 Symbol.iterator 访问该属性；**

◼ **转成这样的一个东西有什么好处呢？**

​		 当一个对象变成一个可迭代对象的时候，就可以进行某些迭代操作；

​		 比如 for...of 操作时，其实就会调用它的 @@iterator 方法；

```js
const info={
  friends:['suanko','rin','len'],
  [Symbol.iterator]:function(){
    let index=0
    return{
      next:()=>{
        if(index<this.friends.length){
          return{done:false,value:this.friends[index++]}
        }else{
          return{done:true}
        }
      }
    }
  }
}
```

## **原生迭代器对象**

◼ **事实上我们平时创建的很多原生对象已经实现了可迭代协议，会生成一个迭代器对象的：**

​		 String、Array、Map、Set、arguments对象、NodeList集合；

```js
//数组本身是一个可迭代对象
const names=['abc','cba','nba']

//获取可迭代的函数
console.log(names[Symbol.iterator]) //[Function:values]

//调用可迭代函数，获取到迭代器
const iterator=names[Symbol.iterator]()
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())//{value:undefined,done:true}
```

## **可迭代对象的应用**

◼ **那么这些东西可以被用在哪里呢？**

​		 JavaScript中语法：**for ...of、展开语法（spread syntax）、yield*（后面讲）、解构赋值（Destructuring_assignment）；**

​		 创建一些对象时：**new Map([Iterable])、new WeakMap([iterable])、new Set([iterable])、new WeakSet([iterable]);**

​		 一些方法的调用：**Promise.all(iterable)、Promise.race(iterable)、Array.from(iterable);**

## **自定义类的迭代**

 在面向对象开发中，我们可以通过class定义一个自己的类，这个类可以创建很多的对象：

 如果我们也希望自己的类创建出来的对象默认是可迭代的，那么在设计类的时候我们就可以添加上 @@iterator 方法；

◼ **案例：创建一个classroom的类**

​		 教室中有自己的位置、名称、当前教室的学生；

​		 这个教室可以进来新学生（push）；

​		 创建的教室对象是可迭代对象；

```js
class Classroom{
  constructor(name,address,finitialStudent){...}

  push(student){
      this.students.push(student)
  }

  [Symbol.iterator](){
    let index=0
    return{
      next:()=>{
        if(index<this.students.length){
          return{done:false,value:this.students[index++]}
        }else{
          return{done:true}
        }
      }
    }
  }
}

const Classroom1=new Classroom("2201","3",["aok","rin"])
for(const stu of Classroom1){
  console.log(stu)
}
```

## **迭代器的中断**

◼ **迭代器在某些情况下会在没有完全迭代的情况下中断：**

​		 比如遍历的过程中通过break、return、throw中断了循环操作；

​		 比如在解构的时候，没有解构所有的值；

◼ **那么这个时候我们想要监听中断的话，可以添加return方法：**

```js
for(const stu of Classroom1){
  console.log(stu)
  if(stu==="abc"){
    break
  }
}

[Symbol.iterator](){
  let index=0
  return{
    next:()=>{
      if(index<this.students.length){
        return{done:false,value:this.students[index++]}
      }else{
        return{done:true}
      }
    },
    return(){
      console.log("迭代器中止了")
      return {done:true}
    }
  }
}
```

# Generator生成器

◼ **生成器是ES6中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执行等。**

​		 平时我们会编写很多的函数，这些函数终止的条件通常是返回值或者发生了异常。

◼ ==**生成器函数也是一个函数**==，但是和普通的函数有一些区别：

​		 首先，生成器函数需要**在function的后面加一个符号：***

​		 其次，生成器函数可以**通过yield关键字来控制函数的执行流程**

​		 最后，**生成器函数的==返回值是一个Generator==（生成器）**

​				✓ **==生成器事实上是一种特殊的迭代器；==**

## **生成器函数执行**

◼ **我们发现下面的生成器函数foo的执行体压根没有执行，它只是返回了一个生成器对象。**

​		 那么我们如何可以让它执行函数中的东西呢？调用next即可；

​		 我们之前学习迭代器时，知道迭代器的next是会有返回值的；

​		 但是我们很多时候不希望next返回的是一个undefined，这个时候我们可以通过yield来返回结果；

```js
function* foo(){
  const value1=100
  console.log(value1)
  yield value1

  const value2=200
  console.log(value2)
  yield value2

  console.log("Bye~~~")
}
//返回生成器
const generator=foo()
console.log(generator.next()) //100   {value: 100, done: false}
console.log(generator.next()) //200   {value: 200, done: false}
console.log(generator.next()) //Bue~~~   {value: undefined, done: true}
```

## **生成器传递参数**

 我们在**调用next函数**的时候，可以**给它传递参数，那么这个参数会作为上一个yield语句的返回值(yield前面的变量）**；

 注意：也就是说我们是为本次的函数代码块执行提供了一个值；

```js
function* myGenerator() {
  const x = yield
  const y = yield x + 1
  console.log(y)  //undefined
  yield y + 2
}
const gen = myGenerator()
console.log(gen.next())  //{ value: undefined, done: false }
console.log(gen.next(10))//{ value: 11, done: false }
console.log(gen.next())//undefined { value: NaN, done: false }
console.log(gen.next())  //{ value: undefined, done: true }
```

## **生成器提前结束** 

◼ **还有一个可以给生成器函数传递参数的方法是通过return函数：**

​		 return传值后这个生成器函数就会结束，之后调用next不会继续生成值了；

```js
function* foo(i){
  const value1=yield "why"
  console.log("value1:",value1)
  const value2=yield value1
  const value3=yield value2
}

const generator=foo()
console.log(generator.next())
console.log(generator.return(123))
console.log(generator.next())
```

## **生成器抛出异常**

◼ **除了给生成器函数内部传递参数之外，也可以给生成器函数内部抛出异常：**

​		 抛出异常后我们可以在生成器函数中捕获异常；

​		 但是在catch语句中不能继续yield新的值了，但是可以在catch语句外使用yield继续中断函数的执行；

```js
function foo() {
  console.log('Start!!!')

  try {
    yield "why"
  } catch (err) {
    console.log("捕獲到異常：",err)
  }

  yield 200

  console.log("End~~~~~")
}

const generator1=foo()
const resultt=generator1.next()
generator1.throw("error message")
console.log(generator1.next())
console.log(generator1.next())
console.log(generator1.next())
```

## **生成器替代迭代器**

◼ **我们发现生成器是一种特殊的迭代器，那么在某些情况下我们可以使用生成器来替代迭代器：**

```js
function* createArrayIterator(arr){
  for(const item of arr){
    yield item
  }
}

const names=["abc","cba","nba"]
namesIterator=createArrayIterator(names)
console.log(namesIterator.next())
console.log(namesIterator.next())
console.log(namesIterator.next())
console.log(namesIterator.next())
```

◼ **事实上我们还可以使用yield\*来生产一个可迭代对象：**

​		 这个时候相当于是一种yield的语法糖，只不过会依次迭代这个可迭代对象，每次迭代其中的一个值；

```js
function* createArrayIterator(arr){
    yield* arr
}
```

## **自定义类迭代 – 生成器实现**

◼ **在之前的自定义类迭代中，我们也可以换成生成器：**

```js
class Classroom{
  constructor(name,address,finitialStudent){...}

  push(student){
      this.students.push(student)
  }

  *[Symbol.iterator](){
    yield* this.students
  }
}
```

# **异步处理方案**

◼ **学完了我们前面的Promise、生成器等，我们目前来看一下异步代码的最终处理方案。**

◼ **案例需求：**

​		 我们需要向服务器发送网络请求获取数据，一共需要发送三次请求；

​		 第二次的请求url依赖于第一次的结果；

​		 第三次的请求url依赖于第二次的结果；

​		 依次类推

```js
function requestData(url){
  return new Promise((resolve,reject)=>{
    setTimeout(()=>{
      resolve(url)
    },2000)
  })
}
```



## promise方案

```js
function getData() {
  requestData('why').then(res1 => {
    requestData(res1 + 'aaa').then(res2 => {
      requestData(res2 + 'bbb').then(res3 => {
        console.log('res3:', res3)
      })
    })
  })
}
```

```js
function getData() {
  requestData('why').then(res1 => {
    return requestData(res1+"aaa")
  }).then(res2=>{
    return requestData(res2+"bbb")
  }).then(res3=>{
    console.log("res3:",res3)
  })
}
```

## **Generator方案**

```js
function* getData() {
  const res1 = yield requestData('why')
  const res2 = yield requestData(res1 + 'aaa')
  const res3 = yield requestData(res2 + 'bbb')
  const res4 = yield requestData(res3 + 'ccc')
  console.log(res4)
}

const generator = getData()
generator.next().value.then(res => {
  generator.next(res).value.then(res => {
    generator.next(res).value.then(res => {
      generator.next(res)
    })
  })
})
```

## **自动执行generator函数**

◼ **目前我们的写法有两个问题：**

​		 第一，我们不能确定到底需要调用几层的Promise关系；

​		 第二，如果还有其他需要这样执行的函数，我们应该如何操作呢？

◼ **所以，我们可以封装一个工具函数execGenerator自动执行生成器函数**

```js
function execGenerator(genFn){
  const generator=genFn()
  function exec(res){  //递归
    const result=generator.next(res)
    if(result.done) return result.value
    result.value.then(res=>{
      exec(res)
    })
  }
  exec()
}
```

