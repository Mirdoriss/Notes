
#一、对于MVVM的理解？

MVVM 是 Model-View-ViewModel 的缩写。
**Model**代表数据模型：定义数据修改和操作的业务逻辑。
**View** 代表UI 组件：负责将数据模型转化成UI 展现出来。
**ViewModel** 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步View 和 Model的对象，连接Model和View。
在MVVM架构下，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。
**ViewModel** 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

#二、Vue的生命周期

**beforeCreate**（创建前） 在数据观测和初始化事件还未开始
**created**（创建后） 完成数据观测，属性和方法的运算，初始化事件，$el属性还没有显示出来
**beforeMount**（载入前） 在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。
**mounted**（载入后） 在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。
**beforeUpdate**（更新前） 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。
**updated**（更新后） 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
**beforeDestroy**（销毁前） 在实例销毁之前调用。实例仍然完全可用。
**destroyed**（销毁后） 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

```
1.什么是vue生命周期？
答： Vue 实例从创建到销毁的过程，就是生命周期。从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为 Vue 的生命周期。

2.vue生命周期的作用是什么？
答：它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。

3.vue生命周期总共有几个阶段？
答：它可以总共分为8个阶段：创建前/后, 载入前/后,更新前/后,销毁前/销毁后。

4.第一次页面加载会触发哪几个钩子？
答：会触发 下面这几个beforeCreate, created, beforeMount, mounted 。

5.DOM 渲染在 哪个周期中就已经完成？
答：DOM 渲染在 mounted 中就已经完成了。
```

1️⃣ Vue 生命周期的理解

Vue 实例有一个完整的生命周期，生命周期也就是指一个实例从开始创建到销毁的这个过程。

- `beforeCreated()`：在实例创建之间执行，数据未加载状态。
- `created()`：在实例创建、数据加载后，能初始化数据，DOM 渲染之前执行。
- `beforeMount()`：虚拟 DOM 已创建完成，在数据渲染前最后一次更改数据。
- `mounted()`：页面、数据渲染完成，真实 DOM 挂载完成。
- `beforeUpadate()`：重新渲染之前触发。
- `updated()`：数据已经更改完成，DOM 也重新 render 完成，更改数据会陷入死循环。
- `beforeDestory()` 和 `destoryed()`：前者是销毁前执行（实例仍然完全可用），后者则是销毁后执行。

#三、 Vue实现数据双向绑定的原理

vue实现数据双向绑定主要是：采**用数据劫持结合发布者-订阅者模式**的方式，通过**Object.defineProperty（）**来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应监听回调。当把一个普通 Javascript 对象传给 Vue 实例来作为它的 data 选项时，Vue 将遍历它的属性，用 Object.defineProperty 将它们转为 getter/setter。用户看不到 getter/setter，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。

vue的数据双向绑定 将MVVM作为数据绑定的入口，整合Observer，Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令（vue中是用来解析 {{}}），最终利用watcher搭起observer和Compile之间的通信桥梁，达到数据变化 —>视图更新；视图交互变化（input）—>数据model变更双向绑定效果。

**js实现简单的双向绑定**

```
<body>
    <div id="app">
    <input type="text" id="txt">
    <p id="show"></p>
</div>
</body>
<script type="text/javascript">
    var obj = {}
    Object.defineProperty(obj, 'txt', {
        get: function () {
            return obj
        },
        set: function (newValue) {
            document.getElementById('txt').value = newValue
            document.getElementById('show').innerHTML = newValue
        }
    })
    document.addEventListener('keyup', function (e) {
        obj.txt = e.target.value
    })
</script>
```


#四、Vue组件间的参数传递

**1.父组件与子组件传值**
父组件传给子组件：子组件通过props方法接受数据,绑定父组件数据，实现双方通信;
子组件传给父组件：$emit方法传递参数
**2.非父子组件间的数据传递，兄弟组件传值**
eventBus，就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。项目比较小时，用这个比较合适。（虽然也有不少人推荐直接用VUEX，具体来说看需求咯。技术只是手段，目的达到才是王道。）



```eventbus.vue
/*新建一个Vue实例作为中央事件总嫌*/
let event = new Vue();

/*监听事件*/
event.$on('eventName', (val) => {`

//......do something`
});
 
/*触发事件*/
event.$emit('eventName', 'this is a message.')
```



#五、Vue的路由实现

**hash模式：**在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；
特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。
hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 http://www.xxx.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。

**history模式：**history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。
history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。**Vue-Router 官网里如此描述：**“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。”

#六、Vue、Angular、React的区别？

（版本在不断更新，以下的区别有可能不是很正确。我工作中只用到vue，对angular和react不怎么熟）
**1.与AngularJS的区别**
相同点：

- [ ] 都支持指令：内置指令和自定义指令；
- [ ] 都支持过滤器：内置过滤器和自定义过滤器；
- [ ] 都支持双向数据绑定；
- [ ] 都不支持低端浏览器。

不同点：

- [ ] AngularJS的学习成本高，比如增加了Dependency Injection特性，而Vue.js本身提供的API都比较简单、直观；
- [ ] 在性能上，AngularJS依赖对数据做脏检查，所以Watcher越多越慢；
- [ ] Vue.js使用基于依赖追踪的观察并且使用异步队列更新，所有的数据都是独立触发的。

**2.与React的区别**
相同点：

- [ ] React采用特殊的JSX语法，Vue.js在组件开发中也推崇编写.vue特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用；
- [ ] 中心思想相同：一切都是组件，组件实例之间可以嵌套；
- [ ] 都提供合理的钩子函数，可以让开发者定制化地去处理需求；
- [ ] 都不内置列数AJAX，Route等功能到核心包，而是以插件的方式加载；
- [ ] 在组件开发中都支持mixins的特性。

不同点：

- [ ] React采用的Virtual DOM会对渲染出来的结果做脏检查；
- [ ] Vue.js在模板中提供了指令，过滤器等，可以非常方便，快捷地操作Virtual DOM。

#七、vue路由的钩子函数

首页可以控制导航跳转，beforeEach，afterEach等，一般用于页面title的修改。一些需要登录才能调整页面的重定向功能。

- [ ] **beforeEach**主要有3个参数to，from，next：
- [ ] **to**：route即将进入的目标路由对象，
- [ ] **from**：route当前导航正要离开的路由
- [ ] **next**：function一定要调用该方法resolve这个钩子。执行效果依赖next方法的调用参数。可以控制网页的跳转。

1️⃣**谈谈你对Vue钩子函数的理解**

vue-router的导航钩子主要用来拦截导航，让他完成跳转或取消。

2️⃣ **vue-router 路由实现**

路由就是用来跟后端服务器进行交互的一种方式，通过不同的路径，来请求不同的资源，请求不同的页面是路由的其中一种功能。

#八、vuex是什么？怎么使用？

只用来读取的状态集中放在store中； 改变状态的方式是提交mutations，这是个同步的事物； 异步逻辑应该封装在action中。
在main.js引入store，注入。新建了一个目录store，….. export 。
场景有：单页应用中，组件之间的状态、音乐播放、登录状态、加入购物车
![图片描述](https://mmbiz.qpic.cn/mmbiz_png/qXrEtQTlRiaj2Nozw1F1ib2mlrkbvpFAUb9w9LxFgATThvRPonrRFaT97iaICCibbcLia6z7K1Sk3neXiaLtH3BjawRg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**state**
Vuex 使用单一状态树,即每个应用将仅仅包含一个store 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。
**mutations**
mutations定义的方法动态修改Vuex 的 store 中的状态或数据。
**getters**
类似vue的计算属性，主要用来过滤一些数据。
**action** 
actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。

```
const store = new Vuex.Store({ //store实例
      state: {
         count: 0
             },
      mutations: {                
         increment (state) {
          state.count++
         }
          },
      actions: { 
         increment (context) {
          context.commit('increment')
   }
 }
})
```

**modules**
项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。

```
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
 }
const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
 }

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
})
```



#九、vue-cli如何新增自定义指令？



1.创建局部指令

```
var app = new Vue({
    el: '#app',
    data: {    
    },
    // 创建指令(可以多个)
    directives: {
        // 指令名称
        dir1: {
            inserted(el) {
                // 指令中第一个参数是当前使用指令的DOM
                console.log(el);
                console.log(arguments);
                // 对DOM进行操作
                el.style.width = '200px';
                el.style.height = '200px';
                el.style.background = '#000';
            }
        }
    }
})
```

2.全局指令

```
Vue.directive('dir2', {
    inserted(el) {
        console.log(el);
    }
})
```

3.指令的使用

```
<div id="app">
    <div v-dir1></div>
    <div v-dir2></div>
</div>
```



#十、vue如何自定义一个过滤器？



html代码：

```
<div id="app">
     <input type="text" v-model="msg" />
     {{msg| capitalize }}
</div>
```

JS代码：

```
var vm=new Vue({
    el:"#app",
    data:{
        msg:''
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
})
```

全局定义过滤器

```
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
```

过滤器接收表达式的值 (msg) 作为第一个参数。capitalize 过滤器将会收到 msg的值作为第一个参数。



#十一、对keep-alive 的了解？



**keep-alive**是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 。

使用方法

```
<keep-alive include='include_components' exclude='exclude_components'>
  <component>
    <!-- 该组件是否缓存取决于include和exclude属性 -->
  </component>
</keep-alive>
```

参数解释
include - 字符串或正则表达式，只有名称匹配的组件会被缓存
exclude - 字符串或正则表达式，任何名称匹配的组件都不会被缓存
include 和 exclude 的属性允许组件有条件地缓存。二者都可以用“，”分隔字符串、正则表达式、数组。当使用正则或者是数组时，要记得使用v-bind 。

使用示例

```
<!-- 逗号分隔字符串，只有组件a与b被缓存。 -->
<keep-alive include="a,b">
  <component></component>
</keep-alive>

<!-- 正则表达式 (需要使用 v-bind，符合匹配规则的都会被缓存) -->
<keep-alive :include="/a|b/">
  <component></component>
</keep-alive>

<!-- Array (需要使用 v-bind，被包含的都会被缓存) -->
<keep-alive :include="['a', 'b']">
  <component></component>
</keep-alive>
```

#十二、一句话就能回答的面试题

**1.css只在当前组件起作用**
答：在style标签中写入**scoped**即可 例如：<style scoped></style>

**2.v-if 和 v-show 区别**
答：v-if按照条件是否渲染，v-show是display的block或none；

**3.$route和$router的区别**
答：$route是“路由信息对象”，包括path，params，hash，query，fullPath，matched，name等路由信息参数。而$

- [x] `$route` 为当前 `router` 跳转对象里面可以获取 `name` 、 `path` 、 `query` 、 `params` 等。

router是“路由实例”对象包括了路由的跳转方法，钩子函数等。

- [x] `$router` 为 `VueRouter` 实例，想要导航到不同 `URL`，则使用 `$router.push` 方法。

**4.vue.js的两个核心是什么？**
答：数据驱动、组件系统

**5.vue几种常用的指令**
答：v-for 、 v-if 、v-bind、v-on、v-show、v-else

**6.vue常用的修饰符？**
答：

- [ ] .prevent: 提交事件不再重载页面；
- [ ] .stop: 阻止单击事件冒泡；
- [ ] .self: 当事件发生在该元素本身而不是子元素的时候会触发；
- [ ] .capture: 事件侦听，事件发生的时候会调用

**7.v-on 可以绑定多个方法吗？**
答：可以

**8.vue中 key 值的作用？**
答：当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟DOM。

**9.什么是vue的计算属性？**
答：在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。

好处：

- [ ] ①使得数据处理结构清晰；
- [ ] ②依赖于数据，数据更新，处理结果自动更新；
- [ ] ③计算属性内部this指向vm实例；
- [ ] ④在template调用时，直接写计算属性名即可；
- [ ] ⑤常用的是getter方法，获取数据，也可以使用set方法改变数据；
- [ ] ⑥相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

**10.vue等单页面应用及其优缺点**
答：

优点：

- [ ] Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统**良好的交互体验**。
- [ ] MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。**减轻服务器压力******良好的前后端工作分离模式**

缺点：

- [ ] 不支持低版本的浏览器，最低只支持到IE9；
- [ ] 不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）**SEO 难度较高**；
- [ ] 第一次加载首页耗时相对长一些**初次加载耗时多**；
- [ ] 不可以使用浏览器的导航按钮需要自行实现前进、后退**前进、后退管理**。

**11.Vue中引入组件的步骤?**

1）采用ES6的import ... from ...语法或CommonJS的require()方法引入组件

2）对组件进行注册,代码如下

```
注册Vue.component('自定义组件-component', {
 template:'自定义组件 component!'
})
```

3）使用组件<自定义组件-component> </自定义组件-component>

**12.<keep-alive> </keep-alive>的作用是什么?**

包裹动态组件时，会缓存不活动的组件实例，主要用于保留组件状态或避免重新渲染。

**13.vue组件封装过程**

**14.NextTick 是做什么的？**

`$nextTick` 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 `$nextTick`，则可以在回调中获取更新后的 DOM。

具体可参考官方文档：深入响应式原理(https://cn.vuejs.org/v2/guide/reactivity.html)。

# 十三、vuex面试相关问题

**1.vuex有哪几种属性？**

一共有五种属性，分别是 State、 Get|ter、Mu|ta|tion 、Action、 Module.

**2.不用Vuex会带来什么问题？****

可维护性下降，而修改数据要维护三个地方；

可读性下降，影响组件里的数据，根本无法看出是从哪来的；

增加耦合性，大量上传派发，让耦合性大大增加，本来Vue用Component就是为了减少耦合，但现在和组件化的背道而驰。

**3. this.$router 和this.$route有何区别？**

- 1.$router为VueRouter实例，想要导航到不同URL，则使用$router.push方法
- 2.$route为当前router跳转对象，里面可以获取name、path、query、params等

###  [vue-router query和params传参(接收参数)](https://www.cnblogs.com/zhangruiqi/p/9412539.html)

### 1.query方式传参和接收参数

```
传参: 
this.$router.push({
        path:'/xxx'
        query:{
          id:id
        }
      })
  
接收参数:
this.$route.query.id
```


  2.params方式传参和接收参数

```
传参: 
this.$router.push({
        name:'xxx'
        params:{
          id:id
        }
      })
  
接收参数:
this.$route.params.id
```

```
//$router : 是路由操作对象，只写对象
//$route : 路由信息对象，只读对象
//操作 路由跳转
this.$router.push({
name:'hello',
params:{
name:'word',
age:'11'
}
})
//读取 路由参数接收
this.name = this.$route.params.name;
this.age = this.$route.params.age;
```

**注意:params传参，push里面只能是 name:'xxxx',不能是path:'/xxx',因为params只能用name来引入路由，如果这里写成了path，接收参数页面会是undefined！！！**

### 另外，二者还有点区别，直白的来说query相当于get请求，页面跳转的时候，可以在地址栏看到请求参数，而params相当于post请求，参数不会再地址栏中显示

**1·query传递参数**

```
  我看了很多人都说query传参要用path来引入，params传参要用name来引入，只是我测试了一下，query使用name来引入也可以传参，使用path也可以。如果有人知道原因可以告诉我一下，谢谢！
//query传参，使用name跳转
this.$router.push({
    name:'second',
    query: {
        queryId:'20180822',
        queryName: 'query'
    }
})


//query传参，使用path跳转
this.$router.push({
    path:'second',
    query: {
        queryId:'20180822',
        queryName: 'query'
    }
})


//query传参接收
this.queryName = this.$route.query.queryName;
this.queryId = this.$route.query.queryId;
```

![img]()![img](https://oscimg.oschina.net/oscnet/d26bf3ef53ef3b06fe118a42eb6ed120892.jpg)

最终不管是path引入还是name引入效果都一样如下图

![img]()![img](https://oscimg.oschina.net/oscnet/3c4ee2f47fdd3bc45e96dd7a2e3d2b6351b.jpg)

**2·params传递参数**

- 使用params传参只能使用name进行引入

```
//params传参 使用name
this.$router.push({
  name:'second',
  params: {
    id:'20180822',
     name: 'query'
  }
})

//params接收参数
this.id = this.$route.params.id ;
this.name = this.$route.params.name ;

//路由

{
path: '/second/:id/:name',
name: 'second',
component: () => import('@/view/second')
}
```

![img](https://oscimg.oschina.net/oscnet/77fa90f59e7ed5f7f493a6f9457d6d0f2a3.jpg)

效果如下图

![img]()![img](https://oscimg.oschina.net/oscnet/53f56de8f69521684825319c2b8a192b301.jpg)

需要注意的是：

params是路由的一部分,必须要在路由后面添加参数名。query是拼接在url后面的参数，没有也没关系。

**params一旦设置在路由，params就是路由的一部分**，如果这个路由有params传参，但是在跳转的时候没有传这个参数，会导致跳转失败或者页面会没有内容。

```
如果路由后面没有  /:id/:name效果如下图，地址栏没有参数
```

![img]()![img](https://oscimg.oschina.net/oscnet/7358f302c45ed65f33245e755615d1afec2.jpg)

但是如果你刷新一下，就会发现页面失败，效果如下图

![img]()![img](https://oscimg.oschina.net/oscnet/cac7ff06a0369dd0dd3df3c157f8915c141.jpg)

因此我们不可能让用户不要刷新，所以我们必须在路由后面加上 /:id/:name

- 使用path进行传参

```
//params传参 使用path
this.$router.push({
  path:'second',
   params: {
    id:'20180822',
     name: 'query'
  }
})

//params接收参数
this.id = this.$route.params.id ;
this.name = this.$route.params.name ;
```

效果如下图

使用path传参什么效果都没有。

![img]()![img](https://oscimg.oschina.net/oscnet/b4ca092edb74cb808939387b24091b2b4ef.jpg)

**3.总结**

1. 传参可以使用params和query两种方式。
2. 使用params只能用name来引入路由，即push里面只能是 name:'xxxx',不能是path:'/xxx',因为params只能用name来引入路由，如果这里写成了path，接收参数页面会是undefined！！！。
3. 使用quer传参y使用path来引入路由。
4. params传参是路由的一部分,必须要在路由后面添加参数名。query是拼接在url后面的参数，没有也没关系。
5. 二者还有点区别，直白的来说query相当于get请求，页面跳转的时候，可以在地址栏看到请求参数，而params相当于post请求，参数不会再地址栏中显示.

### **注意:传参是this.$router,接收参数是this.$route,这里千万要看清了！！！**

# [vue和微信小程序的区别、比较](https://segmentfault.com/a/1190000015684864)

# 一、生命周期

先贴两张图：

#### vue生命周期

![clipboard.png](https://segmentfault.com/img/bVVORa?w=1200&h=3039)

#### 小程序生命周期

![clipboard.png](https://segmentfault.com/img/bVbcfCK?w=662&h=1014)

相比之下，`小程序`的钩子函数要简单得多。

`vue`的钩子函数在跳转新页面时，钩子函数都会触发，但是`小程序`的钩子函数，页面不同的跳转方式，触发的钩子并不一样。

- `onLoad`: 页面加载
  一个页面只会调用一次，可以在 `onLoad` 中获取打开当前页面所调用的 `query` 参数。
- `onShow`: 页面显示
  每次打开页面都会调用一次。
- `onReady`: 页面初次渲染完成
  一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。
  对界面的设置如`wx.setNavigationBarTitle`请在`onReady`之后设置。详见生命周期
- `onHide`: 页面隐藏
  当`navigateTo`或底部tab切换时调用。
- `onUnload`: 页面卸载
  当`redirectTo`或`navigateBack`的时候调用。

#### 数据请求

在页面加载请求数据时，两者钩子的使用有些类似，`vue`一般会在`created`或者`mounted`中请求数据，而在`小程序`，会在`onLoad`或者`onShow`中请求数据。

# 二、数据绑定

`VUE`:vue动态绑定一个变量的值为元素的某个属性的时候，会在变量前面加上冒号：，例：

```
<img :src="imgSrc"/>
```

`小程序`：绑定某个变量的值为元素属性时，会用两个大括号括起来，如果不加括号，为被认为是字符串。例：

```
<image src="{{imgSrc}}"></image>
```

# 三、列表渲染

直接贴代码，两者还是有些相似
**vue：**

```
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>

var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

**小程序：**

```
Page({
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})

<text wx:for="{{items}}">{{item}}</text>
```

# 四、显示与隐藏元素

`vue`中，使用`v-if` 和`v-show`控制元素的显示和隐藏

`小程序`中，使用`wx-if`和`hidden`控制元素的显示和隐藏

# 五、事件处理

`vue`：使用`v-on:event`绑定事件，或者使用`@event`绑定事件,例如:

```
<button v-on:click="counter += 1">Add 1</button>
<button v-on:click.stop="counter+=1">Add1</button>  //阻止事件冒泡
```

`小程序`中，全用`bindtap(bind+event)`，或者`catchtap(catch+event)`绑定事件,例如：

```
<button bindtap="noWork">明天不上班</button>
<button catchtap="noWork">明天不上班</button>  //阻止事件冒泡
```

# 六、数据双向绑定

#### 1.设置值

在`vue`中,只需要再`表单`元素上加上`v-model`,然后再绑定`data`中对应的一个值，当表单元素内容发生变化时，`data`中对应的值也会相应改变，这是`vue`非常nice的一点。

```
<div id="app">
    <input v-model="reason" placeholder="填写理由" class='reason'/>
</div>

new Vue({
  el: '#app',
  data: {
   reason:''
  }
})
```

但是在`小程序`中，却没有这个功能。那怎么办呢？
当表单内容发生变化时，会触发表单元素上绑定的方法，然后在该方法中，通过`this.setData({key:value})`来将表单上的值赋值给`data`中的对应值。
下面是代码，可以感受一下:

```
<input bindinput="bindReason" placeholder="填写理由" class='reason' value='{{reason}}' name="reason" />

Page({
data:{
    reason:''
},
bindReason(e) {
    this.setData({
      reason: e.detail.value
    })
  }
})
```

当页面表单元素很多的时候，更改值就是一件体力活了。和`小程序`一比较，`vue`的`v-model`简直爽的不要不要的。

#### 2.取值

`vue`中，通过`this.reason`取值

`小程序`中，通过`this.data.reason`取值

![clipboard.png](https://segmentfault.com/img/bVbdXrK?w=242&h=180)

# 七、绑定事件传参

在`vue`中，绑定事件传参挺简单，只需要在触发事件的方法中，把需要传递的数据作为形参传入就可以了，例如：

```
<button @click="say('明天不上班')"></button>

new Vue({
  el: '#app',
  methods:{
    say(arg){
    consloe.log(arg)
    }
  }
})
```

在`小程序`中，不能直接在绑定事件的方法中传入参数，需要将参数作为属性值，绑定到元素上的`data-`属性上，然后在方法中，通过`e.currentTarget.dataset.*`的方式获取，从而完成参数的传递，很麻烦有没有...

```
<view class='tr' bindtap='toApprove' data-id="{{item.id}}"></view>
Page({
data:{
    reason:''
},
toApprove(e) {
    let id = e.currentTarget.dataset.id;
  }
})
```

![clipboard.png](https://segmentfault.com/img/bVbdXGC?w=340&h=327)

# 八、父子组件通信

### 1.子组件的使用

在`vue`中，需要：

1. 编写子组件
2. 在需要使用的父组件中通过`import`引入
3. 在`vue`的`components`中注册
4. 在模板中使用

```
//子组件 bar.vue
<template>
  <div class="search-box">
    <div @click="say" :title="title" class="icon-dismiss"></div>
  </div>
</template>
<script>
export default{
props:{
    title:{
       type:String,
       default:''
      }
    }
},
methods:{
    say(){
       console.log('明天不上班');
       this.$emit('helloWorld')
    }
}
</script>

// 父组件 foo.vue
<template>
  <div class="container">
    <bar :title="title" @helloWorld="helloWorld"></bar>
  </div>
</template>

<script>
import Bar from './bar.vue'
export default{
data(){
    return{
        title:"我是标题"
    }
},
methods:{
    helloWorld(){
        console.log('我接收到子组件传递的事件了')
    }
},
components:{
    Bar
}
</script>
```

在`小程序`中，需要：

1. 编写子组件

2. 在子组件的`json`文件中，将该文件声明为组件

   ```
   {
     "component": true
   }
   ```

3. 在需要引入的父组件的`json`文件中，在`usingComponents`填写引入组件的组件名以及路径

   ```
   "usingComponents": {
       "tab-bar": "../../components/tabBar/tabBar"
     }
   ```

4. 在父组件中，直接引入即可

   ```
   <tab-bar currentpage="index"></tab-bar>
   ```

   具体代码:

   ```
   // 子组件
   <!--components/tabBar/tabBar.wxml-->
   <view class='tabbar-wrapper'>
     <view class='left-bar {{currentpage==="index"?"active":""}}' bindtap='jumpToIndex'>
       <text class='iconfont icon-shouye'></text>
       <view>首页</view>
     </view>
     <view class='right-bar {{currentpage==="setting"?"active":""}}' bindtap='jumpToSetting'>
       <text class='iconfont icon-shezhi'></text>
       <view>设置</view>
     </view>
   </view>
   ```

### 2.父子组件间通信

#### **在vue中**

父组件向子组件传递数据，只需要在子组件通过`v-bind`传入一个值，在子组件中，通过`props`接收，即可完成数据的传递，示例:

```
// 父组件 foo.vue
<template>
  <div class="container">
    <bar :title="title"></bar>
  </div>
</template>
<script>
import Bar from './bar.vue'
export default{
data(){
    return{        
        title:"我是标题"
    }
},
components:{
    Bar
}
</script>

// 子组件bar.vue
<template>
  <div class="search-box">
    <div :title="title" ></div>
  </div>
</template>
<script>
export default{
props:{
    title:{
       type:String,
       default:''
      }
    }
}
</script>
```

子组件和父组件通信可以通过`this.$emit`将方法和数据传递给父组件。

#### **在小程序中**

父组件向子组件通信和`vue`类似，但是`小程序`没有通过`v-bind`，而是直接将值赋值给一个变量，如下：

```
<tab-bar currentpage="index"></tab-bar>

此处， “index”就是要向子组件传递的值
```

在子组件`properties`中，接收传递的值

```
properties: {
    // 弹窗标题
    currentpage: {            // 属性名
      type: String,     // 类型（必填），目前接受的类型包括：String, Number, Boolean, Object, Array, null（表示任意类型）
      value: 'index'     // 属性初始值（可选），如果未指定则会根据类型选择一个
    }
  }
```

子组件向父组件通信和`vue`也很类似，代码如下:

```
//子组件中
methods: {   
    // 传递给父组件
    cancelBut: function (e) {
      var that = this;
      var myEventDetail = { pickerShow: false, type: 'cancel' } // detail对象，提供给事件监听函数
      this.triggerEvent('myevent', myEventDetail) //myevent自定义名称事件，父组件中使用
    },
}

//父组件中
<bar bind:myevent="toggleToast"></bar>

// 获取子组件信息
toggleToast(e){
    console.log(e.detail)
}
```

#### 如果父组件想要调用子组件的方法

`vue`会给子组件添加一个`ref`属性，通过`this.$refs.ref的值`便可以获取到该子组件，然后便可以调用子组件中的任意方法，例如：

```
//子组件
<bar ref="bar"></bar>

//父组件
this.$ref.bar.子组件的方法
```

`小程序`是给子组件添加`id`或者`class`，然后通过`this.selectComponent`找到子组件，然后再调用子组件的方法,示例：

```
//子组件
<bar id="bar"></bar>

// 父组件
this.selectComponent('#id').syaHello()
```

#### 小程序父组件改变子组件样式

1.父组件将style传入子组件
2.父组件传入变量控制子组件样式
3.在父组件样式中，在子组件类名前面加上父组件类名

```
<view class='share-button-container' bindtap='handleShareBtn'>
   <share-button  product="{{goodProduct}}" type="1" back-color="#fff" fore-color="#9e292f" bind:error="on_error" />
</view>

.share-button-container .button--btn-navigator__hover{
  background: #fff;
}
```

小程序和vue在这点上太相似了，有木有。。。

# [写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1#) 

key是给每一个vnode的唯一id,可以`依靠key`,更`准确`, 更`快`的拿到oldVnode中对应的vnode节点。

### `v-for` with `v-if`优先级

 `v-for` 的优先级比 `v-if` 更高，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。当你想为仅有的 一些 项渲染节点时，这种优先级的机制会十分有用。

**永远不要把 v-if 和 v-for 同时用在同一个元素上。**

# vue.js中computed计算属性和methods方法的区别

计算属性是基于他们的依赖进行缓存的，只有在它的相关依赖改变时才会重新求值

而methods方法是不产生缓存的，每次都会重新加载

# 6. bindtap与catchtap

1. `bindtap`与`catchtap`都是点击事件
2. `bindtap`存在事件冒泡
3. `catchtap`可以消除事件冒泡

三：vue和jquey对比 

jQuery是使用选择器（$）选取DOM对象，对其进行赋值、取值、事件绑定等操作，其实和原生的HTML的区别只在于可以更方便的选取和操作DOM对象，而数据和界面是在一起的。比如需要获取label标签的内容：`$("lable").val();`,它还是依赖DOM元素的值。 

Vue则是通过Vue对象将数据和View完全分离开来了。对数据进行操作不再需要引用相应的DOM对象，可以说数据和View是分离的，他们通过Vue对象这个vm实现相互的绑定。这就是传说中的MVVM。

四：控制显示隐藏，我们从中可以看出vue只需要控制属性isShow的值为true和false即可，而jquery则还是需要操作dom元素控制按钮的显示和隐藏

五：vue适用的场景：复杂数据操作的后台页面，表单填写页面

　   jquery适用的场景：比如说一些html5的动画页面，一些需要js来操作页面样式的页面

　　然而二者也是可以结合起来一起使用的，vue侧重数据绑定，jquery侧重样式操作，动画效果等，则会更加高效率的完成业务需求

# 微信小程序数据类型判断


  数据类型的判断可以使用 constructor 属性。