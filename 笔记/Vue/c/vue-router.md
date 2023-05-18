◼ **vue-router是基于路由和组件的**

​		 路由用于设定访问路径, 将路径和组件映射起来；

​		 在vue-router的单页面应用中, 页面的路径的改变就是组件的切换；

◼ **安装Vue Router：**

`npm install vue-router`

# 基本使用

 第一步：创建路由需要映射的组件（打算显示的页面）；

 第二步：通过createRouter创建路由对象，并且传入routes和history模式；

​		✓ 配置路由映射: 组件和路径映射关系的routes数组；

​		✓ 创建基于hash或者history的模式；

 第三步：使用app注册路由对象（use方法）；

 第四步：路由使用: 通过<router-link>和<router-view>；

![vue-router](./vue-router.png)

# **路由的默认路径**

◼ **在routes中又配置了一个映射：**

​		 path配置的是根路径: /

​		 redirect是重定向, 也就是我们将根路径重定向到/home的路径下, 这样就可以得到我们想要的结果了

```js
const routes=[
    {path:'/',redirect:'/home'},
    {path:'/home',component:Home}
]
```

# **history模式**

```js
import {createRouter,createWebHistory} from 'vue-router'
const router=createRouter({
    routes,
    history:creatWebHistory()
})
```

# **router-link**

◼ **router-link事实上有很多属性可以配置：**

◼ **to属性：**

​		 是一个字符串，或者是一个对象

◼ **replace属性：**

​		 设置 replace 属性的话，当点击时，会调用 router.replace()，而不是 router.push()；

◼ **active-class属性：**

​		 设置激活a元素后应用的class，默认是router-link-active

◼ **exact-active-class属性：**

​		 链接精准激活时，应用于渲染的 <a> 的 class，默认是router-link-exact-active；

# **路由懒加载**

◼ **当打包构建应用时，JavaScript 包会变得非常大，影响页面加载：**

​		 如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就会更加高效；

​		 也可以提高首屏的渲染效率；

◼ **其实这里还是我们前面讲到过的webpack的分包知识，而Vue Router默认就支持动态来导入组件：**

​		 这是因为component可以传入一个组件，也可以接收一个函数，该函数 需要放回一个Promise；

​		 而import函数就是返回一个Promise；

```js
const routes=[
    {path:'/home',component:()=>import('../pages/Home.vue')},
    {path:'/about',component:()=>import('../pages/About.vue')}
]
```

# **路由的其他属性**

◼ name属性：路由记录独一无二的名称；

◼ meta属性：自定义的数据

```js
{
    path:'/about',
    name:'about-router',
    component:()=>import('../pages/About.vue'),
    meta:{
        name:'why',
        age:18
    }
}
```

