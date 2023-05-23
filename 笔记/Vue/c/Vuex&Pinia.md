# Vuex

**Vuex 使用单一状态树：用一个对象就包含了全部的应用层级的状态**

==**每个应用将仅仅包含一个store 实例**==

◼ **依然我们要使用vuex，首先第一步需要安装vuex：**

`npm install vuex`

## Store

◼ **每一个Vuex应用的核心就是store（仓库）：**

​		 store本质上是一个容器，它包含着你的应用中大部分的状态（state）；

◼ **Vuex和单纯的全局对象有什么区别呢？**

​		◼ 第一：Vuex的状态存储是响应式的

​				 当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会被更新；

​		◼ 第二：你不能直接改变store中的状态

​				 改变store中的状态的唯一途径就显示**提交 (commit) mutation**；

​				 这样使得我们可以方便的跟踪每一个状态的变化，从而让我们能够通过一些工具帮助我们更好的管理应用的状态；

◼ **使用步骤：**

​		 创建Store对象；

​		 在app中通过插件安装；

◼ **在组件中使用store，我们按照如下的方式：**

​		 在模板中使用；

​		 在options api中使用，比如computed；

​		 在setup中使用；

## Getters

**某些属性我们可能需要经过变化后来使用，这个时候可以使用getters：**

```js
const store=createStore({
    getters:{
        totalPrice(state){
            let totalPrice=0;
            for(const book of state.books){
                totalPrice+=book.count*book.price
            }
            return totalPrice
        }
    }
})
```

**getters可以接收第二个参数获取别的getters方法：**

```js
const store=createStore({
    getters:{
        totalPrice(state){
            let totalPrice=0;
            for(const book of state.books){
                totalPrice+=book.count*book.price
            }
            return totalPrice+getters.myName;
        },
        myName(state){
            return state.name;
        }
    }
})
```

◼ **getters中的函数本身，可以返回一个函数，那么在使用的地方相当于可以调用这个函数**

### **mapGetters的辅助函数**

◼ **Vue2**

```js
computed:{
    //数组写法
    ...mapGetters(["totalPrice","myName"]),
    //对象写法
    ...mapGetters({
    	finalPrice:"totalPrice",
        finalName:"myName"
    })
}
```

◼ **在setup中使用**

```js
import {useStore,mapGetters} from 'vuex';
import {computed} from 'vue';

export function useGetters(mapper){
	const store=useStore();
	const stateFns=mapGetters(mapper)
    const state={}
    Object.keys(stateFns).forEach(fnKey=>{
        state[fnKey]=computed(stateFns[fnKey].bind({$store:store}))
    })
    return state
}
```

## **Mutation**

◼ **更改 Vuex 的 store 中的状态的唯一方法是提交 mutation：**

```js
mutations:{
    increment(state){
        state.counter++
    },
    decrement(state){
       	state.counter--     
    }
}
```

### 带参

◼ **很多时候我们在提交mutation的时候，会携带一些数据，这个时候我们可以使用参数：**

```js
mutations:{
    addNumber(state,payload){
        state.counter+=payload
    }
}
```

◼ **payload为对象类型**

```js
mutations:{
    addNumber(state,payload){
        state.counter+=payload.count
    }
}
```

◼ **对象风格的提交方式**

```js
$store.commit({
    type:"addNumber",
    counter:100
})
```

### 常量

◼ **定义常量：mutation-type.js**

`export const ADD_NUMBER = 'ADD_NUMBER'`

◼ **定义mutation**

```js
[ADD_NUMBER](state,payload){
    state.counter += payload.count
}
```

◼ **提交mutation**

```js
$store.commit({
    type:ADD_NUMBER,
    count:100
})
```

### **mapMutations辅助函数**

◼ **我们也可以借助于辅助函数，帮助我们快速映射到对应的方法中：**

```js
methods:{
    ...mapMutations({
        addNumber:ADD_NUMBER,
    }),
    ...mapMutations(["increment","decrement"])
}
```

◼ **在setup中使用也是一样的：**

```js
const mutations=mapMutations(['increment',"decrement"])
const mutations2=mapMutations({
    addNumber:ADD_NUMBER
})
```

### **原则**

◼ 一条重要的原则就是要记住 **mutation 必须是同步函数**

​		 这是因为devtool工具会记录mutation的日记；

​		 每一条mutation被记录，devtools都需要捕捉到前一状态和后一状态的快照；

​		 但是在mutation中执行异步操作，就无法追踪到数据的变化；

◼ **所以Vuex的重要原则中要求 mutation必须是同步函数；**

## **actions**

◼ **Action类似于mutation，不同在于：**

​		 Action提交的是mutation，而不是直接变更状态；

​		 Action可以包含任意异步操作；

```js
mutation:{
    increment(state){
        state.counter++
    }
},
actions:{
    increment(context){
        context.commit("increment")
    }
}
```

◼ **这里有一个非常重要的参数context：**

​		 context是一个和store实例均有相同方法和属性的context对象；

​		 所以我们可以从其中获取到commit方法来提交一个mutation，或者通过 context.state 和 context.getters 来获取 state 和getters；

### 使用-分发

◼ **如何使用action呢？进行action的分发：**

​		 分发使用的是 store 上的dispatch函数；

```js
add(){
    this.$store.dispatch("increment")
}
```

◼ **同样的，它也可以携带我们的参数：**

```js
add(){
    this.$store.dispatch("increment",{ count:100 })
}
```

◼ **也可以以对象的形式进行分发：**

```js
add(){
    this.$store.dispatch({
        type:"increment",
        count:100
    })
}
```

### **mapActions辅助函数**

◼ **action也有对应的辅助函数：**

​		 对象类型的写法；

​		 数组类型的写法；

```js
methods:{
    ...mapActions(["increment","decrement"]),
    ...mapActions({
        add:"increment",
        sub:"decrement"
    })
}
```

```js
const actions1= mapActions(["decrement"]);
const actions2= mapActions({
    add:"increment",
    sub:"decrement"
})
```

### **异步操作**

◼ **Action 通常是异步的，那么如何知道 action 什么时候结束呢？**

​		 我们可以通过让action返回Promise，在Promise的then中来处理完成后的操作；

```js
actions:{
    increment(context){
        return new Promise((resolve)=>{
            setTimeout(()=>{
                context.commit("increment")
                resolve("异步完成")
            },1000)
        })
    }
}
//组件中
const store=useStore()
const increment=()=>{
    store.dispatch("increment").then(res=>{
        console.log(res,"异步完成")
    })
}
```

## **module**

◼ **什么是Module？**

​		 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，当应用变得非常复杂时，store 对象就有可能变得相当臃肿；

​		 为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**；

​		 每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块；

```js
const moduleA={
    state:()=>({}),
    mutations:{},
    actions:{},
    getters:{}
}
const moduleB={
    state:()=>({}),
    mutations:{},
    actions:{},
}

const store=createStore({
    modules:{
        a:moduleA,
        b:moduleB
    }
})

store.state.a//-> moduleA 的状态
store.state.b//-> moduleB 的状态
```

◼ 对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**：

？？？？？？

### **命名空间**

◼ 默认情况下，模块内部的action和mutation仍然是注册在**全局的命名空间**中的：

​		 这样使得多个模块能够对同一个 action 或 mutation 作出响应；

​		 Getter 同样也默认注册在全局命名空间；

◼ 如果我们希望模块具有更高的封装度和复用性，可以添加 namespaced: true 的方式使其成为带命名空间的模块：

​		 当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名；

```js
namespaced:true,
//访问: $store.state.user.xxx
state(){
    return {
        name:"why",
        age:18,
        height:1.88
    }
},
mutations:{
    //访问: $store.commit("user/changeName")
    changeName(state){
        state.name="coderwhy"
    }
},
getters:{
    //访问: $store.getters["user/info"]
    //这里会有四个参数
    info(state,getters,rootState,rootGetters) {
        return `name:${state.name}`
    }
},
actions: {
    //这里一共有六个参数
   changeNameAction({commit,dispatch,state,rootState,getters,rootGetters}){
       commit("changeName","kobe")
       //提交、派发根组件中state
       commit("changerootName",null,{root:true})
       dispatch("changeRootNameAction",null,{root:true})
    }
}
```

# Pinia

◼ **使用Pinia之前，我们需要先对其进行安装：**

`npm install pinia`

◼ **创建一个pinia并且将其传递给应用程序：**

```js
//store/index.js中
import { createPinia } from 'pinia'
const pinia = createPinia()
export default pinia

//main.js中
import pinia from './store'
createApp(App).use(pinia).mount('#app')
```

## store

◼ **什么是Store？**

​		 一个 Store （如 Pinia）是一个实体，它会持有为绑定到你组件树的状态和业务逻辑，也就是保存了全局的状态；

​		 它有点像始终存在，并且每个人都可以读取和写入的组件；

​		 你可以在你的应用程序中定义任意数量的Store来管理你的状态；

◼ **Store有三个核心概念：**

​		 state、getters、actions；

​		 等同于组件的data、computed、methods；

​		 一旦 store 被实例化，你就可以直接在 store 上访问 state、getters 和 actions 中定义的任何属性；

### **定义**

◼ **定义一个Store：**

​		 我们需要知道 Store 是使用 defineStore() 定义的，

​		 并且它需要一个唯一名称，作为第一个参数传递；

```js
export const useCounter=defineStore("counter",{
    state(){
        return {
            counter:0
        }
    }
})
```

◼ **这个name，也称为id，是必要的，Pinia 使用它来将store连接到devtools。**

◼ **返回的函数统一使用useX作为命名方案，这是约定的规范；**

### 使用

◼ **Store在它被使用之前是不会创建的，我们可以通过调用use函数来使用Store：**

```vue
<template>
	<div class="home">
        <h2>Counter:{{counterStore.counter}}</h2>
    </div>
</template>

<script setup>
	import {useCounter} from '@/store/counter'
    const counterStore = useCounter()
</script>
```

◼ **注意Store获取到后不能被解构，那么会失去响应式：**

​		 为了从 Store 中提取属性同时保持其响应式，需要使用storeToRefs()。

```js
const {counter}=counterStore//不是响应式的
const {counter:counter2}=toRefs(counterStore)//是响应式的
const {counter:counter3}=storeToRefs(counterStore)//是响应式的
```

## state

◼ **state是store的核心部分，因为store是用来帮助我们管理状态的。**

​		 在 Pinia 中，状态被定义为返回初始状态的函数；

```js
export const useCounter=defineStore("counter",{
    state:()=>({
        counter:0,
        name:"why"
    })
})
```

### 操作

◼ **读取和写入 state：**

​		 默认情况下，您可以通过 store 实例访问状态来直接读取和写入状态；

```js
const counterStore=useCounter()
counterStore.counter++
counterStore.name="coderwhy"
```

◼ **重置 State：**

​		 你可以通过调用 store 上的 $reset() 方法将状态 重置 到其初始值；

```js
const counterStore=useCounter()
counterStore.$reset()
```

◼ **改变State：**

​		 除了直接用 store.counter++ 修改 store，你还可以调用 $patch 方法；

​		 它允许您使用部分“state”对象同时应用多个更改；

```js
const counterStore=useCounter()
counterStore.$patch({
    counter:100,
    name:"kobe"
})
```

◼ **替换State：**

​		 您可以通过将其 $state 属性设置为新对象来替换 Store 的整个状态：

```js
counterStore.$state={
    counter:1,
    name:"why"
}
```

## getters

◼ **Getters相当于Store的计算属性：**

​		 它们可以用 defineStore() 中的 getters 属性定义；

​		 getters中可以定义接受一个state作为参数的函数；

```js
export const useCounter = defineStore("counter",{
    state:()=>({
        counter:100,
        firstname:"kobe",
        lastname:"bryant",
        age:18
    }),
    getters:{
        doubleCounter:(state)=>state.counter*2,
        doublePlusOne:(state)=>state.counter*2+1,
        fullname:(state)=>state.firstname+state.lastname
    }
})
```

### **访问**

◼ **访问当前store的Getters：**

```js
const counterStore=useCounter()
console.log(counterStore.doubleCounter)
console.log(counterStore.fullname)
```

◼ **Getters中访问自己的其他Getters：**

​		 我们可以通过this来访问到当前store实例的所有其他属性;

```js
doublePlusOne:function(state) {
    return this.doubleCounter + 1
}
```

◼ **访问其他store的Getters：**

```js
message:function(state) {
    const userStore = useUser()
    return this.fullname+":"+userStore.nickname
}
```

◼ **Getters也可以返回一个函数，这样就可以接受参数：**

```js
export const useMain = defineStore("main",{
    state:()=>({
        users:[
            {id:111,name:"why",age:18},
            {id:112,name:"kobe",age:20},
            {id:113,name:"james",age:25},
        ]
    }),
    getters:{
        getUserById(state){
            return userId=>{
                return state.users.find(item=>item.id===userId)
            }
        }
    }
})



const mainStore=useMain()
const getUserById=mainStore.getUserById



<h2>User:{{getUserById(111)}}</h2>
<h2>User:{{getUserById(112)}}</h2>
```

## **Actions**

◼ **Actions 相当于组件中的 methods。**

​		 可以使用 defineStore() 中的 actions 属性定义，并且它们非常适合定义业务逻辑；

```js
actions:{
    increment(){
        this.counter++
    },
    randomCounter(){
        this.counter=Math.random()
    }
}
```

```js
function increment(){
    counterStore.increment()
}
function randomClick(){
    counterStore.randomCounter()
}
```

◼ **和getters一样，在action中可以通过this访问整个store实例的所有操作；**

◼ **并且Actions中是支持异步操作的，并且我们可以编写异步函数，在函数中使用await；**

```js
actions:{
    async fetchHomeDataAction(){
        const res=await fetch("http://123.1.2.3/home")
        const data=await res.json()
        console.log("data:",data)
        return data
    }
}
```

```js
const counterStore=useCounter()
counterStore.fetchHomeDataAction().then(res=>{
    console.log(res)
})
```

