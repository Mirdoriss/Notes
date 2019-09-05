

# 页面优化  初级

请分别从html、css、js、服务器4个方面简述网站优化的方法

HTML：1.尽可能的减少 HTTP 的请求数 [content]

 	          2.减少 DNS 查询 [content]

​               3.使 AJAX 缓存

CSS：1.将 CSS 样式放在页面的上方 [css]

​		   2.避免使用 CSS 中的 Expressions表达式 [css]

​		   3.配置实体标签（ETags） [css]

JS：1.将脚本移动到底部（包括内联的） [javascript]

​	    2.移除重复的脚本 [javascript]

CSS&JS： 1.将 JavaScript 和 CSS 独立成外部文件 [javascript] [css]

​				   2.压缩 JavaScript 和 CSS (包括内联的) [javascript] [css]

SERVER： 1.使用 CDN（Content Delivery Network） [server]

​					2.添加 Expires 头(或者 Cache-control ) [server]

​					3.Gzip 组件 [server]

​					4.避免重定向 [server]

SEO优化搜索引擎   CSS Sprites Inline Images  Lazy Load Images

**html压缩**

1. 使用在线网站进行压缩(开发过程中一般不用)
2. nodejs 提供了html-minifier工具
3. 后端模板引擎渲染压缩

**css压缩：**

1. 使用在线网站进行压缩(开发过程中一般不用)

2. 使用html-minifier工具

3. 使用clean-css对css压缩

   

**js的压缩和混乱**

1. 使用在线网站进行压缩(开发过程中一般不用)
2. 使用html-minifier工具
3. 使用uglifyjs2对js进行压缩

**文件合并**

1. 使用在线网站进行文件合并
2. 使用nodejs实现文件合并(gulp、fis3)

# 浏览器 初级

常见的浏览器内核有哪些？

```
  Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
  Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
  Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
  Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
```

介绍一下你对浏览器内核的理解？

```
  主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。
  渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

  JS引擎则：解析和执行javascript来实现网页的动态效果。

  最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。
```

# JavaScript 初级

### JavaScript里document.write()和innerHTML的区别



| 区别     | document.write()               | innerHTML               |
| -------- | ------------------------------ | ----------------------- |
| 类型     | document对象中的方法           | 存在于Element对象中属性 |
| 插入位置 | 脚本元素script的位置           | 指定的元素内            |
| 拼接方法 | 多次调用                       | 利用+=                  |
| 覆盖问题 | 文档解析完再调用会覆盖否则不会 | 直接调用会覆盖原内容    |

### JavaScript运算符typeof和instanceof的区别

| 区别       | typeof                                                       | instancenof                      |
| ---------- | ------------------------------------------------------------ | -------------------------------- |
| 作用       | 检测数据类型<br />简单数据类型<br />Undefined、Null、Boolean、Number、String<br />复杂数据类型Object | 检测对象之间的关联性(实例)       |
| 返回       | 小写字母字符串                                               | 布尔值                           |
| 操作数     | 简单数据类型、函数或者对象                                   | 左边必须是引用类型右边必须是函数 |
| 操作数数量 | 1个                                                          | 2个                              |

### JavaScript对调两个变量的多种方法

临时变量法

```临时变量法
var a=3,b=5,c=b;
b=a;
a=c;
```

加减法

```加减法
var a=3,b=5;
a=a+b;
b=a-b;
a=a-b;
```

数组法

```数组法
var a=3,b=5;
a=[a,b];
b=a[0];						   数组  ['Tech','Egg','Teacher']
a=a[1];						   索引		0	  1	      2
```

对象法

```对象法
var a=3,b=5;							对象是以键值对来保存数据
a={a:b,b:a};
b=a.b;	键b的值
a=a.a;  键a的值
```

数组运算法（高级）

```数组运算法（高级）
var a=3,b=5;
a=[b,b=a][0];←
   ↑ ↑根据运算符优先执行
```

按位异或法

```按位异或法
var a=,b=5;
a=a^b;  //0110
b=b^a;	//0011
a=a^b;  //0101
```

$$
3的二进制是0011	|
5的二进制是0101
$$

解构赋值法

```解构赋值法
var a=3，b=5；
[a,b]=[b,a]
```

### JavaScript相等('= =')和全等('= = =')运算符的区别

```
6='6';true 6==='6';false
true==1;false==0;true
true===1;false===0;false
''==0;' '==0; true
null=undefine; true
null==0; false
undefined='' false
'false'==false;false
NaN==NaN;false
NaN==false;false
NaN===false;false

var a={};
var b={};
var c=a;
a==b;false
a===b;false		
a==c;true
a===c;true
```

| 栈（stack） | 堆（heap） |
| ----------- | ---------- |
| c           |            |
| b→          | ｛｝       |
| a→          | ｛｝       |

### JS异步编程方式有几种？

a. 回调函数

b. 事件监听

c. 发布订阅

d. promise

### javascript的typeof返回哪些数据类型

```
undefined	string	boolean	number	symbol(ES6)	Object	Function
```

请用原生js编写一个字符串除重函数

1）字符串去重

1.for遍历

```

function removeRepeatStr(str){
    var newStr = '';
    var flag;
    var len = str.length;
    for(var i=0; i<len; i++){
        flag = 1;
        var newLen = newStr.length;
        for(var j=0; j<newLen; j++){
            if(str[i] == newStr[j]){
                flag = 0;
                break;
            }
        }
        if(flag){
            newStr = newStr + str[i];
        }
    }
    return newStr; 
}

```

2）indexOf方法（无兼容问题）

```
function removeRepeatStr(str){
    var newStr = '';
    var len = str.length;
    for(var i=0; i<len; i++){
        if(newStr.indexOf(str[i])==-1){
            newStr = newStr + str[i];
        }
    }
    return newStr;
}

```

3）search方法

```

function removeRepeatStr(str){
    var newStr = '';
    var len = str.length;
    for(var i=0; i<len; i++){
        if(newStr.search(str[i])==-1){
            newStr = newStr + str[i];
        }
    }
    return newStr;
}

```

4）对象属性

```

function removeRepeatStr(str){
    var obj = {};
    var newStr = '';
    var len = str.length;
    for(var i=0; i<len; i++){
        if(!obj[str[i]]){
            newStr = newStr + str[i];
            obj[str[i]] = 1;//注意，这里的1是给对象属性赋值，这个值可以任意取。意思是把每个遍历的字符作为对象属性并赋值保存，保证该属性的唯一性
        }
    }
    return newStr;
}

```



### 数组去重几种方法是什么？

```
var array = [3, 7, 1, 3, 7, 1, 2, 4, 3, 7, 5];
```

### for循环遍历法

```
function has(array, val) {
     for (var i = 0, len = array.length; i < len; i++) {
          if (array[i] === val)
               return true;
     }
     return false;
}

var newArray = [];

for (var i = 0, len = array.length; i < len; i++) {
     var curr = array[i];

     if (!has(newArray, curr))
          newArray.push(curr);
}

console.log(newArray)
```

------

### IndexOf

```
var newArray = [];
for (var i = 0, len = array.length; i < len; i++) {
     if (newArray.indexOf(array[i]) === -1)
          newArray.push(array[i]);
}
console.log(newArray)
```

------

### includes

```
var newArray = array.reduce(function (result, current) {
     if (!result.includes(current))
          result.push(current);
     return result;
}, []);
console.log(newArray)
```

### New Set

------

```
var set = new Set();
array.forEach(function (curr) {
     set.add(curr);
});
var newArray = Array.from(set);
console.log(newArray);
```

------

### From

```
 function deleteRepeat(arr){
        const seen=new Map(){
            return arr.filter((a)=>!seen.has(a)&&seen.set(a,1));
        }
    }
    ------
var newArray = Array.from(new Set(array));
console.log(newArray);
```

请用原生js编实现，判断一个字符串出现次数最多的字符，并统计出现次数

<script type="text/javascript">
    //charAt()可返回指定的字符串
    var str = "hello world!";
    document.write(str.charAt(1)); //e
</script>

```
var str = "abcaaaaacdef";
//console.log(str.length);
var obj = {};
for(var i = 0; i < str.length; i++){
    if( !obj[str.charAt(i)]){
        obj[str.charAt(i)] = 1;
    }else{
        obj[str.charAt(i)]++;
    }
}
//存出现次数最多的值和次数
var number = '';
var num = 0;
console.log("obj为：",obj); //Object {a: 6, b: 1, c: 2, d: 1, e: 1…}
//i代表每一项
for(var i in obj){
    //console.log("i为：",i);
    //obj[i] 为每一项的值
    console.log("obj",obj[i]);
    if(obj[i]>num){
        num = obj[i];
        number = i;
    }
}
console.log("最终出现最多的字母是："+ number + ",次数为："+ num);//最终出现最多的字母是：a,次数为：6
————————————————
```



# CSS 初级 

### CSS如何让超宽文本替换为省略号 

```css
.div1{
height:200px;
width:500px;
border:5px dotted blue;
color:lightblue;
overflow:hidden;
white-space:nowrap;
}
.div2{
height:200px;
width:500px;
border:5px dotted blue;
color:lightpink;
overflow:hidden;
white-space:nowrap;
}
```

1.设置宽度width;

2.强制文本在一行显示white-space:nowrap;

3.隐藏溢出内容overflow:hidden;

4.溢出进行省略text-overflow：ellipsis;

### 如何用纯CSS绘制三角形

```
.div{
width:0;
height:0;
border:250px solid transparent;
brder-top-color:red;
display:inline-block;
}
```

###  如何用CSS把正方形变成圆形 

请简述CSS样式的覆盖规则

1、由于继承而发生样式冲突时，最近祖先优先。
2、继承的样式和直接指定的样式冲突时，直接指定的样式优先。
3、直接指定的样式发生冲突时，样式权重值高者优先。
4、样式权重值相同时，后者优先。
5、!important的样式属性不被覆盖。

请列举position的值，并说明作用

① static 默认 静态 正常文档流
② relative 相对 不脱离文档流 只改变自身位置
③ absolute 绝对 相对于最近仅为的父元素 脱离文档流
④ fixed 固定 相对于浏览器窗口 脱离文档流

让一个200x200的div在各个屏幕上下左右居中

```
.main{
    text-align: center; /*让div内部文字居中*/
    background-color: #fff;
    border-radius: 20px;
    width: 300px;
    height: 350px;
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}
```

```
.main{
    text-align: center;
    background-color: #fff;
    border-radius: 20px;
    width: 300px;
    height: 350px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
}

```

让DIV垂直居中

![这**重点内容**里写图片描述](https://img-blog.csdn.net/20170816164452724?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2ViX2h3Zw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# HTML 初级 

  从输入URL到页面加载发生了什么？

1. DNS解析
2. TCP连接
3. 发送HTTP请求
4. 服务器处理请求并返回HTTP报文
5. 浏览器解析渲染页面
6. 连接结束

DNS解析的过程进行递归查询、迭代查询

网址真正的解析过程为: . -> .com -> google.com. -> www.google.com

##### DNS优化（DNS缓存）

浏览器缓存，系统缓存，路由器缓存，IPS服务器缓存，根域名服务器缓存，顶级域名服务器缓存，主域名服务器缓存

chrome浏览器中输入:chrome://dns/→chrome浏览器的DNS缓存

##### DNS负载均衡

根据每台机器的负载量，该机器离用户地理位置的距离过程就是DNS负载均衡又名DNS重定向

CDN(Content Delivery Network)就是利用DNS的重定向技术。CDN节点的服务器负责响应用户的请求，提供所需的内容。

##### **TCP连接**

HTTP协议是使用TCP作为其传输层协议的，当TCP出现瓶颈时，HTTP也会受到影响。

#### HTTPS协议

HTTP报文是包裹在TCP报文中发送的，服务器端收到TCP报文时会解包提取出HTTP报文。HTTPS协议的本质就是HTTP + SSL(or TLS)。

#### HTTPS过程

HTTPS在传输数据之前需要客户端与服务器进行一个握手(TLS/SSL握手)，在握手过程中将确立双方加密传输数据的密码信息。

### HTTP请求

发送HTTP请求的过程就是构建HTTP请求报文并通过TCP协议中发送到服务器指定端口(HTTP协议80/8080, HTTPS协议443)。HTTP请求报文是由三部分组成: **请求行**, **请求报头**和**请求正文**。

#### 请求行

格式如下:
`Method Request-URL HTTP-Version CRLF`

```
eg: GET index.html HTTP/1.1
```

常用的方法有: GET, POST, PUT, DELETE, OPTIONS, HEAD。

TODO：

- GET和POST有什么区别？

```
- 1. GET使用URL或Cookie传参，而POST将数据放在BODY中
- 2. GET方式提交的数据有长度限制，则POST的数据则可以非常大
- 3. POST比GET安全，因为数据在地址栏上不可见
  4. GET和POST最大的区别主要是GET请求是幂等性的，POST请求不是。
```

#### 请求报头

请求报头允许客户端向服务器传递请求的附加信息和客户端自身的信息。
PS: 客户端不一定特指浏览器，有时候也可使用Linux下的CURL命令以及HTTP客户端测试工具等。
常见的请求报头有: Accept, Accept-Charset, Accept-Encoding, Accept-Language, Content-Type, Authorization, Cookie, User-Agent等。

#### 请求正文

使用POST, PUT等方法时，通常需要客户端向服务器传递数据时需要设置Content-Type: application/json

### 服务器处理请求并返回HTTP报文

HTTP响应报文也是由三部分组成: **状态码**, **响应报头**和**响应报文**。

#### 状态码

状态码是由3位数组成，第一个数字定义了响应的类别，且有五种可能取值:

- 1xx：指示信息–表示请求已接收，继续处理。
- 2xx：成功–表示请求已被成功接收、理解、接受。
- 3xx：重定向–要完成请求必须进行更进一步的操作。
- 4xx：客户端错误–请求有语法错误或请求无法实现。
- 5xx：服务器端错误–服务器未能实现合法的请求。
  平时遇到比较常见的状态码有:200, 204, 301, 302, 304, 400, 401, 403, 404, 422, 500(分别表示什么请自行查找)。

TODO:

- 301和302有什么区别？

  ```
  - 301表示永久跳转，302表示临时跳转
  - 共同点：url替换为了一个新的，然后发出请求。
  - 不同点：
  - 搜索引擎会抓取新的内容而保留旧的地址，服务器返回302
  - 搜索引擎在抓取新的内容的同时也将旧的网址替换为了重定向之后的网址301。
  ```

- HTTP缓存

- Http 缓存机制作为 web 性能优化的重要手段

  

Expires

> 响应头，代表该资源的过期时间。

Cache-Control

> 请求/响应头，缓存控制字段，精确控制缓存策略。

If-Modified-Since

> 请求头，资源最近修改时间，由浏览器告诉服务器

Last-Modified

> 响应头，资源最近修改时间，由服务器告诉浏览器。

Etag
响应头，资源标识，由服务器告诉浏览器。

If-None-Match

> 请求头，缓存资源标识，由浏览器告诉服务器。

匹配使用的字段

- If-Modified-Since 和 Last-Modified
- Etag 和 If-None-Match

js执行时间相比下载时间要快的多，如果能优化下载时间，用户体验会提升很多。

#### 响应报头

常见的响应报头字段有: Server, Connection...。

#### 响应报文

服务器返回给浏览器的文本信息，通常HTML, CSS, JS, 图片等文件就放在这一部分。

### 浏览器解析渲染页面

浏览器是一个边解析边渲染的过程。

首先浏览器解析HTML文件构建DOM树，然后解析CSS文件构建渲染树，等到渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上。reflow(回流)和repain(重绘)。

同步任务(synchronous)和异步任务(asynchronous)

## Web优化

DNS优化部分缩短连接时间对内容进行压缩，减少reflow的次数。

请简述一下cookies，sessionStorage和localStorage的区别？

--------------------

![clipboard.png](https://segmentfault.com/img/bVblgHE?w=1085&h=369)

请尝试用Canvas画一个等边 三角形

```js
<canvas class="myCanvas">
  <p>添加恰当的反馈信息。</p>
</canvas>
var canvas = document.querySelector('.myCanvas');
var width = canvas.width = window.innerWidth;
var height = canvas.height = window.innerHeight;
var ctx = canvas.getContext('2d');
function degToRad(degrees) {
  return degrees * Math.PI / 180;
};
ctx.fillStyle = 'rgb(255, 0, 0)';
ctx.beginPath();
ctx.moveTo(50, 50);
ctx.lineTo(150, 50);
var triHeight = 50 * Math.tan(degToRad(60));
ctx.lineTo(100, 50+triHeight);
ctx.lineTo(50, 50);
ctx.fill();
```

![img](https://gss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=c4d1b0677ec6a7efb973a020cdca8369/6a600c338744ebf8398c7187d1f9d72a6159a78c.jpg)

### 什么是服务器端渲染和客户端渲染？

TCP和UDP的区别？

1. TCP是面向连接的而UDP是面向非连接的
2. TCP提供可靠连接, UDP提供不可靠连接
3. TCP提供TCP报文，UDP提供用户数据报文  基于TCP的传输协议: HTTP Telnet FTP SMTP POP3  基于UDP的传输协议:  DNS   TFTP  