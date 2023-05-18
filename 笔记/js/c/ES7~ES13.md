# **ES7**

## **Array Includes**

◼ 在ES7之前，如果我们想判断一个数组中是否包含某个元素，需要通过 indexOf 获取结果，并且判断是否为 -1。

◼ 在ES7中，我们可以通过**includes**来判断一个数组中是否包含一个指定的元素，根据情况，如果包含则返回 true，否则返回false。

`arr.includes(valueToFind[,fromIndex])`

```js
if(names.includes("why",4)){
    console.log("包含why")
}

console.log(names.indexOf(NaN)) //-1
console.log(names.includes(NaN)) //true
```

## **指数exponentiation运算符**

◼ 在ES7之前，计算数字的乘方需要通过 Math.pow 方法来完成。

◼ 在ES7中，增加了 ***\* 运算符**，可以对数字来计算乘方。

```js
const result1=Math.pow(3,3)
const result2=3**3
```

# ES8

## **Object values**

◼ 之前我们可以通过 Object.keys 获取一个对象所有的key

◼ **在ES8中提供了** **Object.values** **来获取所有的value值：**

```js
const obj={
    name:"why",
    age:18
}
console.log(Object.values(obj))//['why',18]
//如果传入字符串
console.log(Object.values("abc"))//['a','b','c']
```

## **Object entries**

◼ **通过** **Object.entries** **可以获取到一个数组，数组中会存放可枚举属性的键值对数组。**

​		 可以针对对象、数组、字符串进行操作；

```js
const obj={
    name:"why",
    age:18
 }
 console.log(Object.entries(obj)) //[ ['name','why'],['age',18] ]
 for(const entry of Object.entries(obj)){
    const [key,value]=entry
    console.log(key,value)
 }
 //如果是一个数组
 console.log(Object.entries(["abc","cba"]))//[ ['0','abc],['1','cba'] ]
 //如果是一个字符串
 console.log(Object.entries("abc"))//[ ['0','a'],['1','b'],['2','c'] ]
```

## **String Padding**

◼ 某些字符串我们需要对其进行前后的填充，来实现某种格式化效果，ES8中增加了 padStart 和 padEnd 方法，分别是对字符串的首尾进行填充的

```js
 const message="Hello World"

 console.log(message.padStart(15,"a"))//aaaaHello World
 console.log(message.padEnd(15,"b"))//Hello Worldbbbb
```

◼ **我们简单具一个应用场景：比如需要对身份证、银行卡的前面位数进行隐藏：**

```js
 const cardNum="325846864586632456"
 const lastNum=cardNum.slice(-4)
 const finalNum=lastNum.padStart(cardNum.length,"*")
 console.log(finalNum)//************2456
```

## **Trailing Commas**

鸡肋。◼ **在ES8中，我们允许在函数定义和调用时多加一个逗号：**

```js
function foo(a,b,){
    console.log(a,b)
}
foo(10,20,)
```

## **Object Descriptors**

◼ **Object.getOwnPropertyDescriptors ：**

​		 见js高级-对象-额外方法补充。

◼ **Async Function**：**async、await**

​		 见后续。

# ES9

◼ **Async iterators：后续迭代器讲解**

◼ **Object spread operators：展开运算符，见ES6**

◼ **Promise finally：后续讲Promise讲解**

# ES10

## **flat、flatMap**

◼ **flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。**

```js
const arr = [1, 2, [3, 4, [5, 6]]];
const Arr1 = arr.flat(1);
const Arr2 = arr.flat(2);
console.log(Arr1); // [1, 2, 3, 4, [5, 6]]
console.log(Arr2); // [1, 2, 3, 4, 5, 6]
```

◼ **flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。**

​		 注意一：flatMap是先进行map操作，再做flat的操作；

​		 注意二：flatMap中的flat相当于深度为1；

```js
const arr = [1, 2, 3];
const squaredFlattenedArr = arr.flatMap(x => [x ** 2]);
console.log(squaredFlattenedArr);//[1,4,9]
```

## **Object fromEntries**

◼ **在前面，我们可以通过 Object.entries 将一个对象转换成 entries**

◼ **那么如果我们有一个entries了，如何将其转换成对象呢？**

​		 ES10提供了 Object.formEntries来完成转换

使用场景示例：

```js
 const paramsString='name=why&age=18'
 const searchParams=new URLSearchParams(paramsString)
 for(const param of searchParams){
    console.log(param)//['name', 'why']['age', '18']
 }
 const searchObj=Object.fromEntries(searchParams)
 console.log(searchObj)//{name: 'why', age: '18', height: '1.88'}
```

## **trimStart、trimEnd**

◼ **去除一个字符串首尾的空格，我们可以通过trim方法，如果单独去除前面或者后面呢？**

​		 ES10中给我们提供了trimStart和trimEnd；

## **其他知识点**

◼ **Symbol description：已经讲过了**

◼ **Optional catch binding：后面讲解try cach讲解**

# ES11

## **BigInt**

◼ **在早期的JavaScript中，我们不能正确的表示过大的数字：**

​		 大于MAX_SAFE_INTEGER的数值，表示的可能是不正确的。

◼ **那么ES11中，引入了新的数据类型BigInt，用于表示大的整数：**

​		 BitInt的表示方法是在数值的后面加上n

```js
const bigInt=9007865548654785215n
console.log(bigInt+1n)
```

## **Nullish Coalescing Operator**

Nullish coalescing operator是JavaScript的一个新运算符，用来简化对null或undefined值的判断。**它的语法是双问号（??）。**

```js
const foo = null ?? 'default value';
console.log(foo); // 'default value'

const bar = undefined ?? 'default value';
console.log(bar); // 'default value'

const baz = 0 ?? 'default value';
console.log(baz); // 0

const qux = '' ?? 'default value';
console.log(qux); // ''
```

## **Optional Chaining**

**可选链**是JavaScript的一个新特性，用于简化访问对象或数组可能不存在的属性或元素的代码。**它的语法是问号加点（?.）**，可以在尝试访问属性或元素时避免出现TypeError错误。

例如，在访问嵌套的对象属性时，可以使用optional chaining来避免可能出现的“cannot read property of undefined”错误：

```js
const obj = {
  nestedObj: {
    prop: 'hello'
  }
};

console.log(obj.nestedObj.prop); // 'hello'
console.log(obj.nonExistentObj.prop); // TypeError: Cannot read property 'prop' of undefined

// 使用 optional chaining：
console.log(obj.nonExistentObj?.prop); // undefined
```

类似地，在访问数组元素时，可以使用optional chaining来避免可能出现的“cannot read property of undefined”错误：

```js
const arr = [1, 2, 3];

console.log(arr[0]); // 1
console.log(arr[3]); // undefined
console.log(arr[3].prop); // TypeError: Cannot read property 'prop' of undefined

// 使用 optional chaining：
console.log(arr[3]?.prop); // undefined
```

Optional chaining也可以与nullish coalescing operator一起使用，以便在属性或元素不存在时返回一个默认值：

```js
const obj = {
  nestedObj: {}
};

console.log(obj.nestedObj.prop ?? 'default value'); // 'default value'

const arr = [1, 2, 3];

console.log(arr[3]?.prop ?? 'default value'); // 'default value'
```

## **Global This**

◼ **在之前我们希望获取JavaScript环境的全局对象，不同的环境获取的方式是不一样的**

​		 比如在浏览器中可以通过this、window来获取；

​		 比如在Node中我们需要通过global来获取；

◼ **在ES11中对获取全局对象进行了统一的规范：globalThis**

```js
console.log(globalThis)
console.log(this)//浏览器上
console.log(global)//Node中
```

## **for..in标准化**

◼ **在ES11之前，虽然很多浏览器支持for...in来遍历对象类型，但是并没有被ECMA标准化。**

◼ **在ES11中，对其进行了标准化，for...in是用于遍历对象的key的：**

```js
const obj={
    name:"why",
    age:18
}
for(const key in obj){
    console.log(key)
}
```

## **其他知识点**

◼ **Dynamic Import：**后续ES Module模块化中讲解。

◼ **Promise.allSettled：**后续讲Promise的时候讲解。

◼ **import meta：**后续ES Module模块化中讲解

# ES12

## **FinalizationRegistry**

◼ **FinalizationRegistry** **对象可以让你在对象被垃圾回收时请求一个回调。**

​		 FinalizationRegistry 提供了这样的一种方法：当一个在注册表中注册的对象被回收时，请求在某个时间点上调用一个清理回调。（清理回调有时被称为 finalizer ）;

​		 你可以通过调用register方法，注册任何你想要清理回调的对象，传入该对象和所含的值;

```js
 let obj={name:"why"}
 const registry=new FinalizationRegistry(value=>{
    console.log("对象被回收！",value)
 })
 registry.register(obj,"obj")
 obj=null
```

## **WeakRefs**

◼ 如果我们默认将一个对象赋值给另外一个引用，那么这个引用是一个强引用（不会被回收）：

​		 如果我们希望是一个弱引用的话，可以使用WeakRef；

​		 使用WeakRef拿值需要调用deref()

```js
let info={name:"why",age:18}
 let obj=new WeakRef(info)

 const finalRegistry=new FinalizationRegistry(()=>{
    console.log("对象被回收！")
 })
 finalRegistry.register(info,"info")

 setTimeout(()=>{
    info=null
 },2000)

 setTimeout(()=>{
    console.log(obj.deref().name,obj.deref().age)
 },8000)
```

## **logical assignment operators**

逻辑赋值运算符是一种组合运算符，它将逻辑运算符（如AND、OR、XOR）与赋值运算符结合在一起，可以用来简化代码并提高可读性。以下是常见的逻辑赋值运算符及其示例：

1.`&&=`：如果左侧变量为真，则执行右侧操作，并将结果赋值给左侧变量。否则，左侧变量的值保持不变。

```js
bool a = true;
int b = 5;
a &&= (b < 10); // 等同于 a = a && (b < 10)
// a 的值现在为 true，因为 b < 10 是真
```

2.`||=`：如果左侧变量为假，则执行右侧操作，并将结果赋值给左侧变量。否则，左侧变量的值保持不变。

```js
bool a = false;
int b = 5;
a ||= (b > 10); // 等同于 a = a || (b > 10)
// a 的值现在为 false，因为 b > 10 是假
```

3.`^=`：对左侧变量和右侧操作进行异或运算，并将结果赋值给左侧变量。

```js
bool a = true;
int b = 5;
a ^= (b == 5); // 等同于 a = a ^ (b == 5)
// a 的值现在为 false，因为 b == 5 是真，所以 a 和右侧操作结果异或得到 false
```

## 其它知识点

◼ **Numeric Separator：讲过了；允许在数字字面量中使用下划线 `_` 来增加可读性，而不影响数字的值。**

◼ **String.replaceAll：字符串替换；**

```js
const str = 'Hello World! Hello Universe!';
const newStr = str.replaceAll('Hello', 'Hi');
console.log(newStr); //Hi World! Hi Universe!
```

# ES13

## **method .at()**

◼ 前面我们有学过字符串、数组的at方法，它们是作为ES13中的新特性加入的：

```js
 //1.数组
 var names=["abc","cba","nba"]
 console.log(names.at(-1)) //nba
 console.log(names.at(1)) //cba
 //2.字符串
 var str="Hello CoderWhy"
 console.log(str.at(1))//e
 console.log(str.at(-1))//y
```

## **Object.hasOwn(obj, propKey)**

◼ Object中新增了一个静态方法（类方法）： hasOwn(obj, propKey)

​		 该方法用于判断一个对象中是否有某个自己的属性；

◼ 那么和之前学习的Object.prototype.hasOwnProperty有什么区别呢？

**因为Object.prototype.hasOwnProperty是实例方法，需要实例调用，而Object.hasOwn是类方法（静态方法），直接可以类调用。**所以：

​		 区别一：防止对象内部有重写hasOwnProperty

​		 区别二：对于隐式原型指向null的对象， hasOwnProperty无法进行判断

```js
var info=Object.create(null)
info.name="why"
console.log(info.hasOwnProperty("name"))//报错
console.log(Object.hasOwn(info,"name"))//true
```

## **New members of classes**

◼ 在ES13中，新增了定义class类中成员字段（field）的其他方式：

 Instance public fields

 Static public fields

 Instance private fields

 static private fields

 static block

```js
class Person{
    address="中国"
    static totalCount="70亿"

    //只能类内部访问
    #sex="male"
    static #maleCount="10亿"

    constructor(name,age){
        this.name=name
        this.age=age
    }

    static {
        console.log("做一些初始化操作。。类加载时一开始就调用")
    }

    printInfo(){
        console.log(this.address,this.#sex,Person.#maleCount)
    }
 }
 var p=new Person(p,18)
 p.printInfo()
```

