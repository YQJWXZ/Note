# Vue

前端开发：
* 模块化(js的模块化、css的模块化、资源的模块化)
* 组件化(复用现有的UI结构、样式、行为)
* 规范化(目录结构的划分、编码规范化、接口规范化、文档规范化、Git分支管理)
* 自动化(自动化构建、自动部署、自动化测试)

> Vue 一套用于构建用户界面的前端框架

Vue的特性：
* 数据驱动视图  ``vue会监听数据的变化，从而自动重新渲染页面的结构``
* 双向数据绑定  ``辅助开发者在不操作DOM的前提下，自动把用户填写的内容同步到数据源中``


## 1. MVVM

MVVM是vue实现数据驱动视图和双向数据绑定的核心原理。 MVVM指的是Model、View和ViewModel
* Model表当前页面渲染时所依赖的数据源
* View表示当前页面所渲染的DOM结构
* ViewModel表示Vue的实例，是MVVM的核心

## 2. Vue使用
* 导入vue.js的script脚本文件
* 在页面声明一个将要被vue所控制的DOM区域
* 创建vue实例对象
```
el: '#app' // 表示当前的vm实例要控制页面上的哪个区域，接收的值是一个选择器

data  // data对象就是要渲染到页面上的数据

methods // 定义事件的处理函数
```

## 3. Vue指令
1. 内容渲染指令
* v-text
* *{{}}*
* v-html
2. 属性绑定指令
* v-bind 为元素的属性动态绑定值  ``指令可以简写成:``
3. 事件绑定指令
* v-on 为DOM元素绑定事件监听  ``指令可以简写成@``
* vue提供了内置变量$event, 它就是原生DOM的事件对象e
* 事件修饰符
    * event.prevent 阻止默认行为
    * event.stop  阻止事件冒泡
    * event.capture  以捕获模式触发当前的事件处理函数
    * event.once 绑定的事件只触发一次
    * event.self 只有在event.target是当前元素自身时触发事件处理函数
* 按键修饰符
    * .esc
    * .enter 
4. 双向绑定指令
* v-model 在不操作DOM的前提下，快速获取表单的数据
    * v-model.number  自动将用户的输入值转为数值类型
    * v-model.trim  自动过滤用户输入的首尾空白字符
    * v-model.lazy  在"change"时而非"input"时更新
5. 条件渲染指令
* v-if  ``每次动态创建或移除元素，实现元素的显示和隐藏``
* v-show  ``动态为元素添加或移除display: none样式，来实现元素的显示和隐藏``
> 如果频繁的切换元素的显示状态，用v-show性能会更好
>
> 如果刚进入页面的时候，某些元素默认不需要被显示，而且后期这个元素很可能也不需要被展示出来，此时v-if性能更好
6. 列表渲染指令
* v-for 基于一个数组来循环渲染一个列表结构
  * item in items  ``(item, index) in list``
> 只要用到v-for指令，那么一定要绑定一个:key属性，而且，尽量把id作为key的值(字符串或数字类型)

## 4. 过滤器

1. 过滤器常用于文本的格式化。可以用在两个地方：插值表达式和v-bind属性绑定。
过滤器应该被添加在JavaScript表达式的尾部，由"管道符"进行调用：(私有过滤器)
```html
<!--在双括号中通过"管道符"调用capitalize过滤器，对message的值进行格式化-->
<p>{{message | capitalize}}</p>

<!--在v-bind中通过"管道符"调用formatId过滤器，对rawId的值进行格式化-->
<div v-bind: id="rawId | formatId"></div>

<script src="./vue-xxxx.js"></script>
<script>
  const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    // 过滤器函数 必须定义在filters节点下
    filters:{
        // 过滤器函数形参中的val, 永远都是"管道符"前面的那个值
        capitalize(val){
            const first = val.charAt(0).toUpperCase()
            const other = val.slice(1)
            // 强调：过滤器中，一定要有一个返回值
            return first+other
        }
    }
  })
</script>
```

2. 使用vue.filter定义全局过滤器
```js
// 第1个参数，是全局过滤器的"名字"
// 第2个参数，是全局过滤器的"处理函数"
Vue.filter('capitalize', (str) => {
    return sr.charAt(0).toUpperCase() + str.slice(1)
})
```

## 5. 侦听器

1. watch侦听器
> 允许开发者监视数据的变化，从而针对数据的变化做特定的操作(检查用户名是否被占用)
* 方法格式的侦听器 ``缺点：无法在刚进入页面的时候自动触发``
```js
const vm = new Vue({
  el: '#app',
  data: {username: ''},
  watch: {
      username(newVal, oldVal) {
        if(newVal=='') return
        $.get('https://www.ecbook.cn/api/finduser/' + newVal, function (result) {
          console.log(result)
        })
      }
  }
})
```
* 对象格式的侦听器  ``好处：可以通过immediate选项，让侦听器自动触发``
```js
const vm = new Vue({
  el: '#app',
  data: {username: 'admin'},
  watch: {
      // 定义对象格式的侦听器
      username: {
          handler(newVal, oldVal){
            console.log(newVal, oldVal)
          },
          immediate: true
      }
  }
})
```
* 深度侦听
```js
const vm = new Vue({
  el: '#app',
  data: {
      // 用户的信息对象
    info: {
        username: 'admin'
    }
  },
  watch: {
      // 定义对象格式的侦听器
/*      info: {
          handler(newVal, oldVal){
            console.log(newVal, oldVal)
          },
        // 开启深度监听，只要对象中任何一个属性变化了，都会触发"对象的侦听器"
          deep: true
      }*/
     // 如果要侦听的是对象的子属性的变化，则必须包裹一层单引号
    'info.username'(newVal) {
      console.log(newVal)
    }
  }
})
```

## 6. 计算属性
> 计算属性指的是通过一系列运算后，最终得到一个属性值。这个动态计算出来的属性值可以被模板结构或methods方法使用
```html
<div class="box" :style="{backgroundColor: rgb}">{{ rgb }}</div>

<script>
  var vm = new Vue({
    el: '#app',
    data: {
        r:0, g:0, b:0
    },
    // 所有的计算属性，都要定义在computed节点之下
    // 计算属性在定义的时候，要定义成"方法格式"
    computed: {
        rgb() {return `rgb(${this.r}, ${this.g}, ${this.b})`}
    },
    methods: {
        show() { console.log(this.rgb)}
    },
  })
</script>
```

## 7. axios

> 一个专注于网络请求的库

```js
document.querySelector('#btnPost').addEventListener('click', async function() {
    /*1. 调用axios之后，使用async/await 进行简化
    2. 使用解构赋值， 从axios封装的大对象中，把data属性解构出来
    3. 把解构出来的data属性，使用冒号进行重命名，一般都命名为{ data: res }*/
  
  
// 如果调用某个方法的返回值是Promise实例，则前面可以添加await
// await只能用在被async修饰的方法中
  // 解构赋值的时候，使用：重命名
  const {data: res} = await axios({
    method: '请求的类型',
    url: '请求的url地址',
    // URL中的查询参数
    params: {},
    // 请求体参数
    data: {}
  }).then((result) => {
    // .then 用来指定请求成功之后的回调函数
    // 形参中的result是请求成功之后的结果
  })

  console.log(res.data)
})
```

## 8. vue-cli

单页面应用程序：SPA, 指的是一个Web网站中只有唯一的一个HTML页面

基于vue-cli快速生成工程化的Vue项目：
> vue create 项目名称

1. vue-cli中src目录的构成
* assets文件夹 ``存放项目中用到的静态资源文件``
* components文件夹  ``封装的、可复用的组件``
* main.js  ``项目的入口文件``
* App.vue  ``项目的根组件``

2. vue项目的运行流程：通过main.js把App.vue渲染到index.html的指定区域中

3. vue组件(对UI结构的复用)的三个组成部分
* template  ``组件的模板结构``
* script  ``组件的JavaScript行为``
* style  ``组件的样式``
> .vue组件中的data不能像之前一样，不能指向对象；必须是一个函数

4. 使用组件的三个步骤
* 使用import语法导入需要的组件 ``import left from '@/components/Left.vue'``
* 使用components节点注册组件(私有子组件)
```js
export default {
    components: {
        Left
    }
}
```
* 以标签形式使用刚才注册的组件  ``<Left></Left>``

5. 使用Vue.components全局注册组件
在vue项目的main.js入口文件中，通过Vue.component()方法，可以注册全局组件
```js
import Count from '@/components/Count.vue'

// 参数1：字符串格式，表示组件的"注册名称"
// 参数2：需要被全局注册的那个组件

Vue.component('MyCount', Count)
```

6. 组件的props

props是组件的自定义属性，在封装通用组件的时候，合理的使用props可以极大的提高组件的复用性
```js
export default {
    // 组件的自定义属性
    // props中的数据，可以直接在模板结构中被使用
    props: ['自定义属性A', '自定义属性B', '其他自定义属性...'],
  
   // 组件的私有数据
   data() {
        return { }
   }
}
```
> props是只读的，不能直接去修改props的值
>
> 要想修改props的值，可以把props的值转存到data中，data中的数据是可读可写的

props中的default默认值：
```js
// 在声明自定义属性时，可以通过default来定义属性的默认值
export default {
    props: {
        init: {
            default: 0,
            // 用type属性定义属性的值类型
            type: Number,
            // 必填项校验
            required: true
        }
    }
}
```
7. v-bind使用自定义属性

8. 组件之间的样式冲突问题
*默认情况下，写在.vue组件中的样式会全局生效，容易造成多个组件之间的样式冲突问题*

导致组件之间的样式冲突的根本原因是：
* 单页面应用程序中，所有组件的DOM结构，都是基于唯一的index.html页面进行呈现的
* 每个组件中的样式，都会影响到整个index.html页面中的DOM元素

解决组件样式冲突问题：
* 属性添加scoped
* 当使用第三方组件库的时候，如果有修改第三方组件默认样式的需求，需要用到 /deep/

9. vue组件的实例对象

## 9. 生命周期 & 数据共享

### 9.1 生命周期与生命周期函数

1. 生命周期：是指一个组价从 创建 -> 运行 -> 销毁 的整个阶段，强调的是一个时间段

2. 生命周期函数：是由vue框架提供的内置函数，会伴随着组件的生命周期，自动按次序进行

3. 组件生命周期的分类：
* 组件创建阶段：new Vue() -> beforeCreate -> created -> beforeMount -> mounted
* 组件运行阶段：-> beforeUpdate -> updated
* 组件销毁阶段: -> beforeDestroy -> destroyed
![生命周期](./images/lifecycle.png)

4. 父组件向子组件共享数据：
* 父组件向子组件共享数据需要使用*自定义属性*
```js
//  父组件
<Son :msg="message" :user="userinfo"></Son>

data() {
    return {
        message: 'hello vue.js',
        userinfo: { name: 'zs', age: 20}
    }
}
```
```js
<template>
  <div>
    <h5>Son组件</h5>
    <p>父组件传递过来的 msg 值是: {{ msg }}</p>
    <p>父组件传递过来的 user 值是: {{ user }}</p>
  </div>
</template>

props: ['msg', 'user']
```
5. 子组件向父组件共享数据：
* 子组件向父组件共享数据需要使用*自定义事件*
```js
// 子组件
export default {
    data() {
        return { count: 0 }
    },
    methods: {
        add() {
            this.count += 1
            // 修改数据时，通过$emit()触发自定义事件
            this.$emit('numchange', this.count)
        }
    }
}
```
```js
// 父组件
<Son @numchange="getNewCount"></Son>

export default {
    data() {
        return { countFromSon: 0}
    },
    methods: {
        getNewCount(val) {
            this.countFromSon = val
        } 
    }
}
```
6. 兄弟组件之间的数据共享
* 在Vue2.x中，兄弟组件之间数据共享的方案是EventBus
  * 创建eventBus.js模块，并向外共享一个Vue的实例对象
  * 在数据发送方，调用bus.$emit('事件名称', 要发送的数据)方法触发自定义事件
  * 在数据接收方，调用bus.$on('事件名称',事件处理函数)方法注册一个自定义事件
```js
// 数据发送方
import bus from './eventBus.js'

export default {
    data() {
        return {
            msg: 'hello vue.js'
        }
    },
    methods: {
        sendMsg() {
            bus.$emit('share', this.msg)
        }
    }
}
```
```js
import Vue from 'vue'
// 向外共享Vue的实例对象
export default new Vue()
```
```js
// 数据接收方
import bus from './eveBus.js'

export default {
    data() {
        return {
          msgFromLeft: ''
        }
    },
    created() {
        bus.$on('share', val => {
            this.msgFromLeft = val
        })
    }
}
```

## 10. ref引用

ref用来辅助开发者在不依赖jQuery的情况下，获取DOM元素或组件的引用。
每个Vue的组件实例上，都包含一个$refs对象，里面存储着对应的DOM元素或组件的引用。
默认情况下，组件的$refs指向一个空对象

1. 使用ref引用组件实例

2. this.$nextTick(cb)方法：会把cd回调推迟到下一个DOM更新周期之后执行。
通俗的理解是：等组件的DOM更新完成之后，再执行cd回调函数。从而能保证cd回调函数可以操作到最新的DOM元素。

3. 数组中的方法
* some循环
```js
arr.some((item, index)=>{
    if(item == ''){
      console.log(index)
      // 在找到对应的项之后，可以通过return true固定的语法，来终止som循环
      return true
    }
})
```
* every循环
```js
// 判断数组中是否被全选
const result = arr.every(item => item.state)
console.log(result)
```
* reduce方法
```js
// 数组中的累加
// arr.filter(item => item.state).reduce((累加的结果, 当前循环项) => {}, 初始值)
const result = arr.filter(item => item.state).reduce((amt, item) => {
    return amt += item.price * item.count
}, 0)
```

## 11. 动态组件 & 插槽 & 自定义指令

### 11.1 动态组件

1. <component>组件，专门用来实现动态组件的渲染
> <component :is="值"></component>

2. 使用keep-alive保持状态: keep-alive可以把内部的组件进行缓存，而不是销毁组件
```js
<keep-alive>
  <component :is="conName"></component>
</keep-alive>
```

3. keep-alive对应的生命周期函数
* 当组件被*缓存*时，会自动触发组件的*deactivated*生命周期函数
* 当组件被*激活*时，会自动触发组件的*activated*生命周期函数

4. keep-alive的include和exclude属性
* include属性用来指定：只有*名称匹配的组件*会被缓存。多个组件名之间使用英文的逗号分隔
* exclude属性指定哪些组件不需要被缓存
```js
<keep-alive include="MyLeft,MyRight">
  <component :is="conName"></component>
</keep-alive>
```
5. 组件的"注册名称"的主要应用场景就是：以标签的形式，把注册好的组件，渲染和使用到页面结构中

6. 组件声明时候的"name"名称的主要应用场景：结合<keep-alive>标签实现组件缓存功能，以及在调试工具中看到组件的name名称

### 11.2 插槽

插槽(Slot)是vue为组件的封装着提供的能力。允许开发者在封装组件时，把不确定的、希望由用户指定的部分定义为插槽
> <slot></slot>

1. v-slot指令(简写形式 # )
```js
<Left>
  <template #default>
    <p></p>
  </template>
</Left>
```

2. 具名插槽

3. 作用域插槽

在封装组件时，为预留的<slot>提供属性对应的值，这种用法叫做"作用域插槽"

### 11.3 自定义指令
```js
directives: {
    // 定义名为color的指令 指向一个配置对象
    color: {
        // 当指令第一次被绑定到元素上的时候，会立即触发bind函数
      
        // 形参中的el 表示当前指令所绑定到的那个DOM对象
      bind(el, binding) {
        el.style.color = 'red'
      },

      //  当DOM更新时bind函数不会被触发，update函数会在每次DOM更新时被调用
      update(el, binding) {
          el.style.color = binding.value
      }
    }
}
```

1. 全局自定义指令
```js
// 参数1：字符串，表示全局自定义指令的名字
// 参数2：对象，用来接收指令的参数值

Vue.directive('color', function (el, binding) {
  el.style.color = binding.value
})
```

2. 把axios挂载到Vue的原型上并配置请求根路径
> axios.default.baseURL = '请求根路径'
> 
> Vue.prototyp.$http = axios
> 今后在每个 .vue组件中要发起请求，直接调用 this.$http.xxx
```js
export default {
    methods: {
        async getInfo() {
            const {data: res} = await this.$http.get('http://www.liulongbin.top:3306/api/get')
          console.log(res)
        }
    }
}
```
缺点：无法实现api接口的复用 

## 12. 路由

SPA：单页面应用程序

前端路由：Hash地址与组件之间的对应关系

1. 前端路由的工作方式
* 用户点击了页面上的路由链接
* 导致URL地址栏中的Hash值发生了变化
* 前端路由监听了到Hash地址的变化
* 前端路由把当前Hash地址对应的组件渲染到浏览器中

### 12.1 vue-router

1. 安装与配置
* 安装vue-router包
* 创建路由模块
  * 在src源代码目录下，新建router/index.js路由模块，并初始化如下代码
```js
// 1. 导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2. 调用Vue.use() 函数, 把VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3. 创建路由的实例对象
const router = new VueRouter()

// 4.向外共享路由的实例对象
export default router
```
* 导入并挂载路由模块
```js
import router from '@/router/index.js'

new Vue({
  render: h => h(App),
  // router: router
  router
})
```
* 声明路由链接和占位符
> router-view
```js
import Vue from 'vue'
import VueRouter from 'vue-router'

import Home from '@/components/Home.vue'
import Movie from '@/components/Movie.vue'
import About from '@/components/About.vue'
Vue.use(VueRouter)

const router = new VueRouter({
    // routes是一个数组，作用：定义hash地址与组件之间的对应关系
  routes：[
      // 路由规则
  { path: './home', component: Home },
  { path: './movie', component: Movie },
  { path: './about', component: About }
    ]
})

export default router
```

2. 当安装和配置了vue-router后，就可以使用*router-link*来替代普通的a链接了
> <router-link to="/home">首页</router-link>

3. 路由重定向
```js
const router = new VueRouter({
  routes：[
      // 重定向的路由规则
  { path: '/', redirect: '/home'},
      // 路由规则
  { path: './home', component: Home },
  { path: './movie', component: Movie },
  { path: './about', component: About }
    ]
})
```

4. 嵌套路由
> <router-link to="/about/tab1">tab1</router-link>

```js
import Tab1 from '@/components/tabs/Tab1.vue'
import Tab2 from '@/components/tabs/Tab2.vue'

const router = new VueRouter({
  routes：[
  {
      path: './about', 
      component: About,
      children: [
          // 通过children属性声明子路由规则
        { path: 'tab1', component: Tab1 },
        { path: 'tab2', component: Tab2 }
      ]
  } 
    ]
})
```

> this.$route是路由的参数对象
> 
> this.$router是路由的导航对象

5. 动态路由

动态路由：把Hash地址中可变的部分定义为参数项，从而提高路由规则的复用性
> { path: '/movie/:id', component: Movie }

为路由规则开启props传参：
> { path: '/movie/:id', component: Movie, props: true}

### 12.2 声明式导航 & 编程式导航

在浏览器中，点击链接实现导航的方式，叫做声明式导航
> 例如点击<a>链接，点击<router-link>

在浏览器中，调用API方法实现导航的方式，叫做编程式导航
> 例如调用location.href跳转到新页面的方式

1. vue-router中的编程式导航API
* this.$router.push('hash地址')  ``跳转到指定的hash地址，并增加一条历史记录``
* this.$router.replace('hash地址')  ``跳转到指定的hash地址，并替换掉当前的历史记录``
* this.$router.go(数值n)  ```在浏览历史中前进或后退```
  * $router.back()  ``在历史记录中，后退到上一个页面``
  * $router.forward()

### 12.3 导航守卫

导航守卫可以控制路由的访问权限

1. 全局前置守卫
```js
// 创建路由实例对象
const router = new VueRouter({.....})

// 调用路由实例对象的 beforeEach 方法，即可声明"全局前置守卫"
// 每次发生路由导航跳转的时候，都会自动触发"回调函数"
router.beforeEach((to, from, next) => {
    next()
    // next 是一个函数，调用next()表示放行，允许这次路由导航
})
```
2. next函数的三种调用方式：
* 用户拥有后台主页的访问权限，直接放行： next()
* 用户没有后台主页的访问权限
  * 强制跳转到登录界面：next('/login')
  * 不允许跳转到后台主页：next(false)

