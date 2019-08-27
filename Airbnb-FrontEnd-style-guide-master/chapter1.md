# JavaScript编码规范

## 类型

### 1.1 基本类型：直接存取基本类型。

* 字符串
* 数值
* 布尔类型
* null
* undefined

```js
const foo = 1
let bar = foo

bar = 9

console.log(foo, bar)   // => 1, 9
```

### 1.2 复杂类型：通过引用的方式存取复杂类型。

* 对象
* 数组
* 函数

```js
const foo = [1, 2]
const bar = foo

bar[0] = 9

console.log(foo[0], bar[0])  // => 9, 9
```

## 引用

### 2.1 对所有的引用使用 const ，不要使用 var 。这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

```js
// bad
var a = 1
var b = 2

// good
const a = 1
const b = 2
```

### 2.2 如果你一定需要可变动的引用，使用 let 代替 var 。因为 let 是块级作用域，而 var 是函数作用域。

```js
// bad
var count = 1
if (true) {
    count += 1
}

// good, use the let
let count = 1
if (true) {
    count += 1
}
```

### 2.3 注意 let 和 const 都是块级作用域。

```js
// const 和 let 只存在于它们被定义的区块内
{
    let a = 1
    const b = 1
}
console.log(a)  // ReferenceError
console.log(b)  // ReferenceError
```

## 对象

### 3.1 使用字面值创建对象。

```js
// bad
const item = new Object()

// good
const item = {}
```

### 3.2 如果你的代码在浏览器环境下执行，别使用保留字作为键值。这样的话在IE8不会运行。但在 ES6 模块和服务器端中使用没有问题。

```js
// bad
const superman = {
    default: { clark: 'kent' },
    private: true,
}

// good
const superman = {
    defaults: { clark: 'kent' },
    hidden: true,
}
```

### 3.3 使用同义词替换需要使用的保留字。

```js
// bad
const superman = {
    class: 'alien',
}

// bad
const superman = {
    klass: 'alien',
}

// good
const superman = {
    type: 'alien',
}
```

### 3.4 创建有动态属性名的对象时，使用可被计算的属性名称。因为这样可以让你在一个地方定义所有的对象属性。

```js
function  getKey(k) {
    return `a key named ${k}`
}

// bad
const obj = {
    id: 5,
    name: 'San Francisco',
}
obj[getKey('enabled')] = true

// good
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
}
```

### 3.5 使用对象方法的简写。

```js
// bad
const atom = {
    value: 1,

    addValue: function (value) {
        return atom.value + value
    }
}

// good
const atom = {
    value: 1,

    addValue(value) {
        return atom.value + value
    }
}
```

### 3.6 使用对象属性值的简写。因为这样更短更有描述性。

```js
const lukeSkywalker = 'Luke Skywalker'

// bad
const obj = {
    lukeSkywalker: lukeSkywalker,
}

// good
const obj = {
    lukeSkywalker,
}
```

### 3.7 在对象属性声明前把简写的属性分组。因为这样能清楚地看出哪些属性使用了简写。

```js
const anakinSkywalker = 'Anakin Skywalker'
const lukeSkywalker = 'Luke Skywalker'

// bad
const obj = {
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
}

// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
}
```

## 数组

### 4.1 使用字面值创建数组。

```js
// bad
const items = new Array()

// good
const items = []
```

### 4.2 向数组添加元素时使用 Array\#push 替代直接赋值。

```js
const someStack = []

// bad
someStack[someStack.length] = 'abracadabra'

// good
someStack.push('abracadabra')
```

### 4.3 使用拓展运算符 ... 赋值数组。

```js
// bad
const len = items.length
const itemCopy = []
let i

for (i = 0; i < len; i++) {
    itemCopy[i] = items[i]
}

// good
const itemsCopy = [...itmes]
```

### 4.4 使用 Array\#from 把一个类数组对象转换成数组。

```js
const foo = document.querySelectorAll('.foo')
const nodes = Array.from(foo)
```

## 解构

### 5.1 使用解构存取和使用多属性对象。因为解构能减少临时引用属性。

```js
// bad
function getFullName(user) {
    const firstName = user.firstName
    const lastName = user.lastName

    return `${firstName} ${lastName}`
}

// good
function getFullName(obj) {
    cosnt { firstName, lastName } = obj
    return `${firstName} ${lastName}`
}

// best
function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`
}
```

### 5.2 对数组使用解构赋值。

```js
const arr = [1, 2, 3, 4]

// bad
const first = arr[0]
const second = arr[1]

// good
const [first, second] = arr
```

### 5.3 需要回传多个值时，使用对象解构，而不是数组解构。增加属性或者改变排序不会改变调用时的位置。

```js
// bad
function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom]
}

// 调用时需要考虑回调数据的顺序
const [left, _, top] = processsInput(input)

// good
function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom }
}

// 调用时只选择需要的数据
const { left, right } = processInput(input字符串
```

## 字符串

### 6.1 字符串使用单引号 ' ' 。

```js
// bad
const name = "Capt. Janeway"

// good
const name = 'Capt. Janeway'
```

### 6.2 字符串超过80个字节应该使用字符串连接符号换行。

### 6.3 过度使用字符串连接符号可能会对性能造成影响。

```js
// bad
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'

// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.'

// good
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.'
```

### 6.4 程序化生成字符串时， 使用模板字符串代替字符串连接。因为模板字符串更为简洁，更具可读性。

```js
// bad
function sayHi(name) {
    return 'How are you, ' + name + '?'
}

// bad
function sayHi(name) {
    return ['How are you, ', name, '?'].join()
}

// good
function sayHi(name) {
    return `How are you, ${name}?`
}
```

## 函数

### 7.1 使用函数声明代替函数表达式。因为函数声明是可命名的，所以他们在调用栈中更容易被识别。此外，函数声明会把整个函数提升（hoisted），而函数表达式只会把函数的引用变量名提升。这条规则使得箭头函数可以取代函数表达式。

```js
// bad
const foo = function () {
}

// good
function foo() {
}
```

### 7.2 函数表达式。

```js
// 立即调用的函数表达式（IIFE）
(() => {
    console.log('Welcome to the Internet. Please follow me.')
})()
```

### 7.3 永远不要在一个非函数代码块（if、while等）中声明一个函数，把那个函数赋给一个变量。浏览器允许你这么做，但它们的解析表现不一致。

### 7.4 ECMA-262把 block定义为一组语句。函数声明不是语句。

```js
// bad
if (currentUser) {
    function test() {
        console.log('Nope.')
    }
}

// good
let test
if (currentUser) {
    test = () => {
        console.log('Yup.')
    }
}
```

### 7.5 永远不要把参数命名为 arguments 。这将取代原来函数作用域内的 arguments 对象。

```js
// bad
function nope(name, options, arguments) {
    // ...stuff...
}

// good
function yup(name, options, args) {
    // ...stuff...
}
```

### 7.6 不要使用 arguments ，可以选择 rest 语法 ... 替代。因为使用 ... 能明确你要传入的参数。另外 rest参数是一个真正的数组，而 arguments 是一个类数组。

```js
// bad
funciton concatenateAll() {
    const args = Array.prototype.slice.call(arguments)
    return args.join('')
}


// good
function concatenateAll(...args) {
    return args.join('')
}
```

### 7.7 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

```js
// really bad
function handleThings(opts) {
    // 不！我们不应该改变函数参数
    // 更加糟糕：如果参数 opts 是 false 的话，它就会被设定为一个对象
    // 但这样的写法会造成一些 Bugs
    // 例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象
    opts = opts || {}
    // ...
}

// still bad
function handleThings(opts) {
    if (opts === void 0) {
        opts = {}
    }
    // ...
}

// good
function handleThings(opts = {}) {
    // ...
}
```

### 7.8 直接给函数参数赋值时需要避免副作用。因为这样的写法让人感到很困惑。

```js
var  b = 1;
// bad
function count(a = b++) {
    console.log(a)
}
count()  // 1
count()  // 2
count(3) // 3
count()  // 3
```

## 箭头函数

### 8.1 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。因为箭头函数创造了新的一个 this 执行环境，通常情况下都满足你的需求，而且这样的写法更为简洁。

```js
// bad
[1, 2, 3].map(function (x) {
    return x * x
})

// good
[1, 2, 3].map((x) => {
    return x * x
})
```

### 8.2 如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 return 都省略掉。如果不是，那就不要省略。

```js
// good
[1, 2, 3].map(x => x * x)

// good
[1, 2, 3].reduce((total, n) => {
    return total + n
}, 0)
```

## 构造器

### 9.1 总是使用 class ，避免直接操作 prototype 。因为 class 语法更为简洁更易读。

```js
// bad
function Queue(contents = []) {
    this._queue = [...contents]
}
Queue.prototype.pop = function() {
    const value = this._queue[0]
    this._queue.splice(0, 1)
    return value
}

// good
class Queue {
    constructor(contents = []) {
        this._queue = [...contents]
    }
    pop() {
        const value = this._queue[0]
        this._queue.splice(0, 1)
        return value
    }
}
```

### 9.2 使用 extends 继承。因为 extends 是一个内建的原型继承方法并且不会破坏 instanceof。

```js
// bad
const inherits = require('inherits')
function PeekableQueue(contents) {
    Queue.apply(this, contents)
}
inherits(PeekableQueue, Queue)
PeekableQueue.prototype.peek = function() {
    return this._queue[0]
}

// good
class PeekableQueue extends Queue {
    peek() {
        return this._queue[0]
    }
}
```

### 9.3 方法可以返回 this 来帮助链式调用。

```js
// bad
Jedi.prototype.jump = function() {
    this.jumping = true
    return true
}

Jedi.prototype.setHeight = function(height) {
    this.height = heigh
}

const luke = new Jedi()
luke.jump()    // => true
luke.setHeight(20)    // => undefined

// good
class Jedi {
    jump() {
        this.jumping = true
        return this
    }

    setHeight(height) {
        this.height = height
        return this
    }
}

const luke = new Jedi()

luke.jump()
    .setHeight(20)
```

### 9.4 可以写一个自定义的 toString\(\) 方法，但要确保它能正常运行并且不会引起副作用。

```js
class Jedi {
    constructor(options = {}) {
        this.name = options.name || 'no name'
    }

    getName() {
        return this.name
    }

    toString() {
        return `Jedi - ${this.getName()}`
    }
}
```

## 模块

### 10.1 总是使用模块 \(import / export \) 而不是其他非标准模块系统。

```js
// bad 
const AirbnbStyleGuide = require('./AirbnbStyleGuide')
module.exports = AirbnbStyleGuide.es6

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide'
export default AirbnbStyleGuide.es6

// best
import { es6 } from './AirbnbStyleGuide'
export default es6
```

### 10.2 不要使用通配符 import。这样能确保你只有一个默认 export。

```js
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide'

// good
import AirbnbStyleGuide from './AirbnbStyleGuide'
```

10.3 不要从 import 中直接 export。

```js
// bad 
// filename es6.js
export { es6 as default } from './AirbnbStyleGuide'

// good
// filename es6.js
import { es6 } from './AirbnbStyleGuide'
export default es6
```

## Iterators and Generators

### 11.1 不要使用 iterators。使用高阶函数例如 map\(\) 和 reduce\(\) 替代 for-of。这加强了我们不变的规则。处理纯函数的回调值更易读，这比它带来的副作用更重要。

```js
const numbers = [1, 2, 3, 4, 5]

// bad
let sum = 0
for (let num of numbers) {
    sum += num
}

sum === 15

// good
let sum = 0
numbers.forEach((num) => sum += num)
sum === 15

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0)
sum === 15
```

## 属性

### 12.1 使用 . 来访问对象的属性。

```js
const luke = {
    jedi: true,
    age: 28,
}

// bad 
const isJedi = luke['jedi']

// good
const isJedi = luke.jedi
```

12.2 当通过变量访问属性时使用中括号 \[\]

```js
const luke = {
    jedi: true,
    age: 28,
}

function getProp(prop) {
    return luke[prop]
}

const isJedi = getProp('jedi')
```



