# vue2.0 常用功能

1. 路由跳转

   当我们想要实现点击链接跳转时，可以使用$router来进行跳转

   

   语法如下：

   this.$router.push({path:"/www",query:{id:'1222'}})

   这里path是要跳转的路径，query里面是路径跳转时要携带的参数，以对象的形式存在

2. 

   获取路由参数

   跳转路由过后，我们可能想要获取路由上的参数，这里我们可以使用

   this.$route.query.id  (id为想要获取的参数)

3. 

   获取当前页面的路径

   想要获取当前文件的路径名称，除了原声的location.href之外，也可以利用$route来获取

   this.$route.path

4. 

   离开页面时弹窗

   在有时候我们开发时可能为某个页面添加这样的功能就是在用户离开某个页面时进行弹窗，用来提示用户一些信息。这时候可以用到路由的导航钩子beforeRouteLeave

   beforeRouteLeave(to,from,next){

   //do something

   },

5. 

   操作dom

   虽然vue提倡我们尽量以数据为驱动，但是有时候我们不得不触及要操作dom，

   这个使用除了原生js之外，操作dom可以使用ref直接绑定一个元素节点或者使用this.$el来操作具体的属性值。

   

   # 使用axios获取数据

   axios是基于 Promise 的 HTTP 请求客户端，可同时在浏览器和 Nodejs服务端中使用，同时包含很多新特性，比如：

   拦截请求和响应

   自动转换 JSON 数据

   客户端免受 XSRF 攻击

   等特性

   创建vue工程，安装axios

   ```
   vue init webpack vue2
   
   cd vue2
   
   npm i axios
   ```

   去掉eslint检查（去掉红框中的部分），运行工程：

   ```
   npm run dev
   ```

   ![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=15a4fddcaaec08fa260013a769ef3d4d/023b5bb5c9ea15ced474b96abc003af33a87b23f.jpg)添加测试代码：

   添加按钮：

   ```
   <ul>  <button v-on:click="postData">Greet</button></ul>
   ```

   添加方法：

   ```
   methods: {  
   postData()  {    
   this.$http({     
   method: 'get',     
   url: '/user',    
   data: {    
   name: 'xiaoming',       
   info: '12'    
   }    
   })  
   }
   
   }
   ```

   ![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=9a892c679d0a304e5222a0fae1c9a7c3/b7fd5266d0160924cc8efa78de0735fae6cd342d.jpg)

刚才添加的代码，方法是get方法，现在可以做get请求测试了，点击页面上的按钮即可，我们会发现红框中输出报一个get错误，说明我们刚发了一个get请求，由于相应的请求的服务端我们没做任何开发，所以客户端是要报404的。

![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=197b26be24738bd4c421b231918a876c/b3fb43166d224f4ae36b2a7003f790529922d1e7.jpg)

修改代码的中方法为post

postData(){  this.$http({    method: 'post',    url: '/user',    data: {      name: 'xiaoming',      info: '12'    }  })}![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=0f2cde02f21986184147ef847aed2e69/503d269759ee3d6de9ccdca849166d224f4ade4f.jpg)

点击按钮，测试post方法，点击后，输出中发现报post的错误，说明我们已经发出请求。![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=0ef7fe0bdb09b3deebbfe468fcbe6cd3/c2fdfc039245d6888c86ebc7aec27d1ed31b24c8.jpg)

1. 并发请求：我们修改代码，添加方法twoReq

   按钮

   <ul>  <button v-on:click="twoReq">Greet</button></ul>

   

   方法：

   twoReq(){        self = this        function getUserAccount() {          return self.$http.get('/user/12345');        }        function getUserPermissions() {          return self.$http.get('/user/12345/permissions');        }        self.$http.all([getUserAccount(), getUserPermissions()])          .then(self.$http.spread(function (acct, perms) {          console.log("done");            //两个请求现已完成          }));    }

   [![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=a464fc40ba7eca80120539e7a1229712/a6efce1b9d16fdfab9249c8dbe8f8c5494ee7b0c.jpg)](http://jingyan.baidu.com/album/86112f139e5e0a2736978711.html?picindex=7)

2. 

   点击按钮做并发测试，发现输出有两个请求，说明我们的并发请求可行。更深入内容请上官网了解。

   [![vue2.0 如何使用axios获取数据](https://imgsa.baidu.com/exp/w=500/sign=3ba9d541c2fcc3ceb4c0c933a244d6b7/83025aafa40f4bfb8754c737094f78f0f63618c8.jpg)](http://jingyan.baidu.com/album/86112f139e5e0a2736978711.html?picindex=8)

# vue2.0如何嵌套路由？子路由怎么配置?

1. 我们先来解决上一次的tab路由切换效果激动class的问题!我们不想让它点击其它路由链接的时候第一个的路由class处于激活状态.首先我们修改下min.js里面的路由配置文件.加上首页路由组件配置.红色圈的地方意思是,默认进来显示的路由路径,还有一个就是直接配置默认的路由路径,其实指向的都是同一个组件.

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=62b9f6011cce36d3a20483300af23a24/b90e7bec54e736d1162286d392504fc2d5626933.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=1)

2. 2

   第二步,将导航的路由改动下.这里要注意的是,第一个router-link to不在是直接指向"/"了,而是"/home".然后我们在第一个路由内容的DIV上加一个动态class.用来判断当前的路由路径.写一个三元表达式.$route是一个路由对象,所有的路由都被存在这个对象当中.我们只需要进行判断当前路由是否指向home这个组件.如果是的话加上一个router-link-active如果不是的话就为空class.效果见第二张,第三张图片.

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=23220adda5af2eddd4f149e9bd110102/35a85edf8db1cb131c7f48c8d454564e93584bd2.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=2)

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=691f31604e10b912bfc1f6fef3fcfcb5/d01373f082025aaf00010810f2edab64034f1a3d.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=3)

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=4f9b1d3484b1cb133e693c13ed5556da/bba1cd11728b4710c2218b17cacec3fdfd0323da.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=4)

3. 3

   此时一个完整的路由切换效果就完成了.接下来就分享下嵌套路由是怎么弄的!首先找到min.js路由 配置文件.比如我们在home组件里加一个嵌套路由chi组件.首先要先创建这个chi组件,然后引入到min.js文件中.然后在home路由的里面加上children这个方法.这样,一个嵌套的子路由就配置好了.

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=545f5734a0ec8a13141a57e0c7029157/d62a6059252dd42a2b4772700a3b5bb5c8eab89a.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=5)

4. 4

   到这里,在home组件中写一个入口进入这个嵌套的路由组件中去.嵌套路由点击进去,会进入到之前写好的chi.vue组件中去.

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=35027e30bffd5266a72b3c149b1a9799/1f178a82b9014a9042ca3466a0773912b21bee71.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=6)

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=ae55fb359582d158bb8259b1b00b19d5/9345d688d43f8794db9e35f5db1b0ef41ad53a83.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=7)

5. 5

   点击之后,就会显示.home里面的chi组件.并且显示chi组件中的内容

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=522a77865cfbb2fb342b58127f482043/b7003af33a87e9503eee8cb719385343faf2b46b.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=8)

   [![vue2.0如何嵌套路由？子路由怎么配置?](https://imgsa.baidu.com/exp/w=500/sign=e62f5f253fd3d539c13d0fc30a85e927/7aec54e736d12f2e96fdd1bb46c2d56284356874.jpg)](http://jingyan.baidu.com/album/9c69d48f81ce2313c8024e5a.html?picindex=9)

6. 6

   最后总结下这个的用法地方在哪里.吧比如一个首页中是路由进来的组件,而这个组件中又有一些类似切换tab功能.这时候切换来的也就是嵌套的子路由了.最后在讲个小的提示,就是,细心的同学可能发现,上面显示的chi组件中,下面的导航还在这里.如果说我不希望他出现呢?比如一个实际应用:首页组件展示新闻列表,点击新闻列表进入一个详情页面.此时如果下面的导航还显示的话就不太好了.其实点击新闻列表的时候就相当于上面分享的点击了子路由进入到一个详情组件(这里的chi组件).这个关于导航的问题会在下一次经验中给大家分享.

   END

## 注意事项

- 主要是判断路由路径.

- 重点children用法.

  

# Vue2.0基于vue-cli+webpack Vuex的用法

组件通信的本质其实就是在组件之间传递数据或组件的状态，通过最基本的方式来进行通信，一旦需要管理的状态多了，代码就会变得十分臃肿和庞大。vuex出现了把公用的状态全抽出来放在vuex的容器中，然后根据一定的规则来进行管理。

## 工具/原料

- JavaScript

## 方法/步骤

1. 1

   搭建环境开始：

   在git命令行下，执行以下命令完成环境的搭建：

   1，npm install --global vue-cli  安装vue命令行工具

   2，vue init webpack vue-demo   使用vue命令生成一个webpack项目，项目名称为vue-demo

   3，cd vue-demo 切入项目

   4，npm install安装package.json中的所有依赖包

   5，npm run dev运行项目

   然后删除默认的Hello.vue组件，把App.vue整理成以下样子

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=0523b6fe4f166d223877159476220945/3b87e950352ac65c40e05746f7f2b21193138a19.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=1)

2. 2

   基本环境搭建完毕之后，开始安装vuex了

   安装命令:npm install vuex --save-dev

   一、安装完成之后，在main.js中引入，并注册store：能通过this.$store来获得这个容器了

   main.js代码：

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=4d023e82e550352ab16125086342fb1a/9a504fc2d5628535922ce3149cef76c6a7ef6332.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=2)

3. 3

   二、在src目录下创建vuex目录，然后在vuex目录下创建store.js，用于存放所有的状态(改变的数据)

   store.js代码：

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=3d698291a8c27d1ea5263bc42bd5adaf/f636afc379310a555a4feec1bb4543a98226109f.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=3)

4. 4

   在state存储了一个状态userName

   三、将状态中的数据渲染到组件中

   在components下面新建一个组件Main.vue，代码如下:

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=1287a441a1345982c58ae5923cf4310b/8d5494eef01f3a29a8afe5fb9525bc315c607c9b.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=4)

5. 5

   通过一个计算属性获取到存储在全局容器store中state保存的状态值

   四，在App.vue中引入组件Main.vue

   App.vue代码:

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=8b5d5faaa80f4bfb8cd09e54334f788f/9f2f070828381f30e07bbd9ca5014c086e06f0a0.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=5)

6. 6

   在页面上把store容器中 state的userName的值读取出来了， 如果修改userName的值，页面上也会发生变化

   五、通过一些交互来改变状态，看下组件是否能收到state的值

   在components组件下新建一个Header.vue组件，代码如下：

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=e2ab461361224f4a5799731339f79044/e850352ac65c10380313fef7be119313b07e8991.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=6)

7. 7

   点击按钮的时候，改变state中useName的值，Main组件中的state.userName的值也会跟着更新，这种改变方式很好理解，看下vuex推荐的状态改变方式

   六、vuex推荐的状态改变方法

   在store.js中新建一个mutations，存放被修改的状态

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=52569c20b0315c6043956befbdb0cbe6/3ac79f3df8dcd1000809c8147e8b4710b9122f69.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=7)

8. 8

   其中第一个参数 state 就是 $store.state，第二个参数 msg 需要另外传入，这个参数通过Header.vue的点击事件中的方法来传递

   Header.vue代码：

   [![Vue2.0基于vue-cli+webpack Vuex的用法](https://imgsa.baidu.com/exp/w=500/sign=45c2319e0f3b5bb5bed720fe06d2d523/00e93901213fb80e161a51e23ad12f2eb938943f.jpg)](http://jingyan.baidu.com/album/ea24bc39cd788bda62b33109.html?picindex=8)

9. 9

   $store.commit 把this.msg的值提交给showUserName。state就收到了改变的状态了，这就是一个vuex最基本的用法和作用。

   # ajax获取后端数据如何显示在前端

ajax从后台获取的数据要展示在前台，只需要再ajax的success方法中对返回的不同格式数据拼接后，插入界面对应dom位置即可。

常用的jquery追加数据方法：append、after、before、text等

## 方法/步骤

1. 

   创建asp.net mvc演示项目：AjaxDemo

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=bc428216bafd5266a72b3c149b199799/1f178a82b9014a90cb8ac840a5773912b21bee93.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=1)

2. 

   在AjaxDemo项目中：

   1）添加文件夹Content，并添加Jquery文件

   2）添加HomeController

   3）添加Index页面

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=e660ea178dcb39dbc1c06756e01709a7/8326cffc1e178a8282a5f15dfa03738da877e8b6.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=2)

3. 

   在Index页面中：

   1）添加JS引用

   2）添加样式

   3）增加测试dom节点

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=4b8010d59b2bd40742c7d3fd4b889e9c/728da9773912b31bf2f543d48a18367adbb4e1b6.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=3)

4. 

   运行效果如下：

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=918e8e37eacd7b89e96c3a833f254291/b21bb051f819861873f3617946ed2e738ad4e693.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=4)

5. 

   在HomeController中，添加Ajax1、Ajax2两个方法

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=0c7c66995a4e9258a63486eeac83d1d1/c9fcc3cec3fdfc03971a6d45d83f8794a5c226e8.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=5)

6. 

   Ajax1、Ajax2的方法详细信息如下：

   1）Ajax1：后台拼装字符串返回

   2）Ajax2：后台返回json对象

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=80b7c83d8e025aafd3327ecbcbecab8d/86d6277f9e2f0708799ca4fde524b899a801f2b6.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=6)

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=1d5f4020a764034f0fcdc2069fc27980/1e30e924b899a9011aacd5cb11950a7b0308f5b6.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=7)

7. 

   在Index页面最后面添加js如下：

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=22194dce7f8da9774e2f862b8050f872/63d0f703918fa0ec6073eaf02a9759ee3c6ddb93.jpg)](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=8)

8. 

   运行效果如下：

   [![ajax获取后端数据如何显示在前端](https://imgsa.baidu.com/exp/w=500/sign=99ac1c22c21b9d168ac79a61c3dfb4eb/fc1f4134970a304eca752e07ddc8a786c8175cfc.jpg)
     ](http://jingyan.baidu.com/album/a3761b2be320971576f9aa30.html?picindex=9)