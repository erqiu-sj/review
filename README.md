# H5 新增

简单介绍以下，包括但不限于

- 语义化标签
- 增强性表单
- Dom 拓展
- 原声拖放
- 媒体查询
- Web socket
- Web Storage
- 地理位置
- Canvas 绘图

## 语义化标签

HTML 语义化是指仅仅从 HTML 元素上就能看出页面的大致结构，比如需要强调的内容可以放在 `<strong>`标签中，而不是通过样式设置 `<span>`标签去做。不同浏览器对 HTML 元素的解析可能有差异，HTML 语义化便是在抛开样式之后，页面能有一个友好的展示效果。我们力求让页面有良好的结构，让页面的元素有含义，同时利于被搜索引擎解析，利于 SEO

- `<header>` 标签通常放在页面或页面某个区域的顶部，用来设置页眉；

- `<nav>` 标签可以用来定义导航链接的集合，点击链接可以跳转到其他页面；

- `<article>` 标签中的内容比较独立，可以是一篇新闻报道，一篇博客，它可以独立于页面的其他内容进行阅读；

- `<section>` 标签表示页面中的一个区域，通常对页面进行分块或对内容进行分段，<section>标签和 <article>标签可以互相嵌套；

- `<aside>` 标签用来表示除页面主要内容之外的内容，比如侧边栏；

- `<footer>` 标签位于页面或页面某个区域的底部，用来设置页脚，通常包含版权信息，联系方式等。

## 增强性表单

新的表单中添加了很多输入型控件，比如：number、url、email、range、color、date 等，通过 input 的 type 属性使用

同时，还添加了 placeholder、required、pattern、min、max、height、width 等表单属性。

## getElementByClassName(),Dom 拓展

可以通过 document 对象及所有 HTML 元素调用该方法。使用这个方法可以更方便地为带有某些类的元素添加事件处理程序，而不必再局限于使用 ID 或标签名(getElementsByTagName)

HTML5 规定可以为元素添加非标准的属性，但要加前缀 data-，为元素提供与渲染无关的信息。

`<div id="div" data-age="2019" data-name="James"></div>`

可以通过元素的 dataset 属性访问自定义属性的值。

`var age = div.dataset.age;`

HTML5 还为 DOM 作了其他扩展，包括 classList 属性、焦点管理、HTMLDocument 变化、字符集属性、插入标记等

## 原声拖放

拖放事件可以控制拖放相关的各个方面，拖动某元素时，将依次触发下列事件

- dragstart
- drag
- dragend

默认情况下，图像、链接和文本是可以拖动的，HTML5 为所有 HTML 元素规定了一个 draggable 属性，表示元素是否可以拖动

## 媒体元素

这两个标签就是`<audio>`和`<video>`。

使用这两个元素时，至少要在标签中包含 src 属性，指向要加载的媒体文件。并非所有的浏览器都支持所有媒体格式，所以可以指定多个不同的媒体来源，此时使用`<source>`元素而不用指定 src 属性。

`<audio>`和`<video>`包含很多属性，包括 autuplay、controls、src 等，还可以触发很多事件，

## web socket

Web Sockets 的目标是在一个单独的持久连接上提供全双工、双向通信。使用标准的 HTTP 服务器无法实现 Web Sockets，只有支持这种协议的专门服务器才能正常工作。

未加密的连接不再是http://而是ws://，加密的连接也不再是https://而是wss://。使用自定义协议而不是HTTP协议的好处是，能够在客户端和服务器之间发送非常少量的数据，而不必担心HTTP那样字节级的开销。

# Js

## 原型

JavaScript 中的对象都有一个特殊的 prototype 内置属性，其实就是对其他对象的引用
几乎所有的对象在创建时 prototype 属性都会被赋予一个非空的值，我们可以把这个属性当作一个备用的仓库
当试图引用对象的属性时会出发 get 操作，第一步时检查对象本身是否有这个属性，如果有就使用它，没有就去原型中查找。一层层向上直到 Object.prototype 顶层

引用《 JavaScript 权威指南》的一段描述：

`每个JavaScript对象都有另一个 与之关联的JavaScript对象（或null，但这很少见）。第二个对象称为原型，第一个对象从原型继承属性。`

翻译出来就是每个 JS 对象一定对应一个原型对象，并从原型对象继承属性和方法。好啦，既然有这么一个原型对象，那么对象怎么和它对应的?

对象 `proto` 属性的值就是它所对应的原型对象：

```
var one = {x: 1};
var two = new Object();
one.__proto__ === Object.prototype // true
two.__proto__ === Object.prototype // true
one.toString === one.__proto__.toString // true
```

`prototype`
不像每个对象都有`proto`属性来标识自己所继承的原型，只有函数才有 `prototype` 属性。

当你创建函数时，JS 会为这个函数自动添加 `prototype` 属性，值是空对象 值是一个有 `constructor` 属性的对象，不是空对象。而一旦你把这个函数当作构造函数 `constructor` 调用（即通过 `new` 关键字调用），那么 JS 就会帮你创建该构造函数的实例，实例继承构造函数 `prototype` 的所有属性和方法（实例通过设置自己的`proto`指向承构造函数的 `prototype` 来实现这种继承

JS 正是通过`proto` 和 `prototype` 的合作实现了原型链

对象的`__proto__`指向自己构造函数的 `prototype` `obj.__proto__.__proto__...`的原型链由此产生，包括我们的操作符 `instanceof` 正是通过探测 `obj.__proto__.__proto__... === Constructor.prototype` 来验证 `obj` 是否是 `Constructor` 的实例。

回到开头的代码，`two = new Object()`中 `Object` 是构造函数，所以 `two.__proto__`就是 `Object.prototype`至于 `one`，ES 规范定义对象字面量的原型就是 `Object.prototype`

**但！**
`Object` 就是一个构造函数，继承了 `Function.prototype`；也是 `Function` 对象，继承了 `Object.prototype`这里就有一个鸡和蛋的问题

```
Object  instanceof  Function  // true
函数 instanceof  Object  // true
```

ES 规范是怎么说的

```
函数本身就是函数，`Function.__proto__`是标准的内置对象`Function.prototype`

Function.prototype.__proto__`是标准的内置对象 `Object.prototype`
```

```
`Function.prototype` 和 `Function.__proto__`都指向 `Function.prototype`，这就是鸡和蛋的问题怎么出现的。

`Object.prototype.__proto__ === null`，说明原型链到 `Object.prototype` 终止
```

## Es6 新特性

- ES6 引入来严格模式

变量必须声明后在使用

不能对只读属性赋值, 否则报错

函数的参数不能有同名属性, 否则报错

禁止 this 指向全局对象

增加了保留字（比如 protected、static 和 interface）

- 关于 let 和 const 新增的变量声明
- 变量的解构赋值
- 字符串的扩展
  includes()：返回布尔值，表示是否找到了参数字符串。
  startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
  endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
- 数值的扩展
  Number.isFinite()用来检查一个数值是否为有限的（finite）。
  Number.isNaN()用来检查一个值是否为 NaN。
- 函数参数指定默认值
- 扩展运算符
- 对象的解构
- 新增 symbol 数据类型

- Set 和 Map 数据结构
  Set:它类似于数组，但是成员的值都是唯一的，没有重复的值。 Set 本身是一个构造函数，用来生成 Set 数据结构。
  Map:它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
- Proxy
  Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问
  都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。
  Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
  Vue3.0 使用了 proxy

- Promise
  Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。
  特点是：
  对象的状态不受外界影响。
  一旦状态改变，就不会再变，任何时候都可以得到这个结果。
- async await

  async 函数对 Generator 函数的区别：
  （1）内置执行器。
  Generator 函数的执行必须靠执行器，而 async 函数自带执行器。也就是说，async 函数的执行，与普通函数一模一样，只要一行。
  （2）更好的语义。
  async 和 await，比起星号和 yield，语义更清楚了。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。
  （3）正常情况下，await 命令后面是一个 Promise 对象。如果不是，会被转成一个立即 resolve 的 Promise 对象。
  （4）返回值是 Promise。
  async 函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用 then 方法指定下一步的操作。

- Class
  class 跟 let、const 一样：不存在变量提升、不能重复声明...
  ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能
  ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

- module
  ES6 的模块自动采用严格模式，不管你有没有在模块头部加上"use strict";

## SessionStorage 和 localStorage 还有 cookie

共同点：都是保存在浏览器端、且同源的

不同点：

1.cookie 数据始终在同源的 http 请求中携带（即使不需要），即 cookie 在浏览器和服务器间来回传递。
cookie 数据还有路径（path）的概念，可以限制 cookie 只属于某个路径下
sessionStorage 和 localStorage 不会自动把数据发送给服务器，仅在本地保存。

2.存储大小限制也不同，cookie 数据不能超过 4K，sessionStorage 和 localStorage 可以达到 5M

3.sessionStorage：仅在当前浏览器窗口关闭之前有效；
localStorage：始终有效，窗口或浏览器关闭也一直保存，本地存储，因此用作持久数据；
cookie：只在设置的 cookie 过期时间之前有效，即使窗口关闭或浏览器关闭 4.作用域不同
sessionStorage：不在不同的浏览器窗口中共享，即使是同一个页面；
localstorage：在所有同源窗口中都是共享的；也就是说只要浏览器不关闭，数据仍然存在
cookie: 也是在所有同源窗口中都是共享的.也就是说只要浏览器不关闭，数据仍然存在

## 闭包

闭包是一个可以访问外部作用域的内部函数，即使这个外部作用域已经执行结束

- 作用域
  作用域决定这个变量的生命周期及其可见性。 当我们创建了一个函数或者 {} 块，就会生成一个新的作用域。需要注意的是，通过 var 创建的变量只有函数作用域，而通过 let 和 const 创建的变量既有函数作用域，也有块作用域
- 外部函数作用域
  内部函数可以访问外部函数中的变量，即使外部函数已经执行完毕。如下：

````

(function autorun(){
let x = 1;
setTimeout(function log(){
console.log(x);
}, 10000);
})();

```

并且，内部函数可以访问外部函数中定义的刑参，如下：

```

(function autorun(p){
let x = 1;
setTimeout(function log(){
console.log(x);//1
console.log(p);//10
}, 10000);
})(10);

```

- 词法作用域(静态作用域)

静态作用域只关心函数在何处被定义，当前的词法作用域是指内部函数在定义的时候就决定了其外部作用域。

- 作用域链
  每一个作用域都有对其父作用域的引用，当我们使用一个变量的时候，javascript 引擎会通过变量名在当前作用域查找，如果没找到就会沿着作用域链向上查找，直到全局作用域

```

let x0 = 0;
(function autorun1(){
let x1 = 1;

(function autorun2(){
let x2 = 2;

(function autorun3(){
let x3 = 3;

     console.log(x0 + " " + x1 + " " + x2 + " " + x3);//0 1 2 3
    })();

})();
})();

```

闭包可以访问其外部(父)作用域中的定义的所有变量。

变量的生命周期取决于闭包的生命周期。被闭包引用的外部作用域中的变量将一直存活直到闭包函数被销毁。如果一个变量被多个闭包所引用，那么直到所有的闭包被垃圾回收后，该变量才会被销毁。

- 闭包 Vs 纯函数
  闭包就是那些引用了外部作用域中变量的函数。

  为了更好的理解，我们将外部函数拆分成闭包和纯函数两个方面。

  闭包是那些引用了外部作用域中变量的函数。
  纯函数是那些没有引用外部作用域中变量的函数，它们通常返回一个值并且没有副作用。

## this

执行上下文的创建阶段，会分别生成变量对象，建立作用域链，确定 this 指向
毋庸置疑的是，this 指向在函数被调用的时候确定的，也就是执行上下文被创建时确定的

- 全局对象中的 this

  全局环境中的 this，指向它本身。因此，这也相对简单，没有那么多复杂的情况需要考虑。

- 函数中的 this

  在一个函数上下文中，this 由调用者提供，由调用函数的方式来决定。如果调用者函数，被某一个对象所拥有，那么该函数在调用时，内部的 this 指向该对象。如果函数独立调用，那么该函数内部的 this，则指向 undefined。但是在非严格模式中，当 this 指向 undefined 时，它会被自动指向全局对象。

```

function foo() {
console.log(this.a) //window
}

function active(fn) {
fn(); // 真实调用者，为独立调用,window
}

var a = 20;
var obj = {
a: 10,
getA: foo
//this 指向 obj
}

active(obj.getA); //window 20

```

- 构造函数与原型方法上的 this

```

function Person(name, age) {

    // 这里的this指向了谁？
    this.name = name;
    this.age = age;

}

Person.prototype.getName = function() {

    // 这里的this又指向了谁？
    return this.name;

}

// 上面的 2 个 this，是同一个吗，他们是否指向了原型对象？

var p1 = new Person('Nick', 20);
p1.getName();

```

我们已经知道，this，是在函数调用过程中确定，因此，搞明白 new 的过程中到底发生了什么就变得十分重要。

通过 new 操作符调用构造函数，会经历以下 4 个阶段。

创建一个新的对象；
将构造函数的 this 指向这个新对象；
指向构造函数的代码，为这个对象添加属性，方法等；
返回新对象。
因此，当 new 操作符调用构造函数时，this 其实指向的是这个新创建的对象，最后又将新的对象返回出来，被实例对象 p1 接收。因此，我们可以说，这个时候，构造函数的 this，指向了新的实例对象：p1。

```

```

```
````

## Virtual DOM

Virtual DOM 就是用一个原生的 JS 对象去描述一个 DOM 实例，所以它比创建一个 DOM 的代价要小很多

在 Vue.js 中，Virtual DOM 是用 VNode 这么一个类去描述

其实 VNode 是对真实 DOM 的一种抽象描述，它的核心定义无非就几个关键属性，标签名，数据，子例程，键值等，其他属性都是扩展的 VNode 的且实现了一些特殊由于 VNode 只是用于映射到真实 DOM 的渲染，不需要包含操作 DOM 的方法，因此它是非常轻量和简单的。

虚拟 DOM 除了它的数据结构的定义，映射到真实的 DOM 必须要经历 VNode 的 create，diff，patch 等过程。然后在 Vue.js 中，VNode 的 create 是通过之前提到的 createElement 方法创建的

## React 和 Vue 的区别

- 设计思想
  vue 的官网中说它是一款渐进式框架，采用自底向上增量开发的设计。

  react 主张函数式编程，所以推崇纯组件，数据不可变，单向数据流，当然需要双向的地方也可以手动实现，
  比如借助 onChange 和 setState 来实现一个双向的数据流。

- 数据绑定
  在 vue 中，使用了双向绑定技术，就是 View 的变化能实时让 Model 发生变化，而 Model 的变化也能实时更新到 View。

  react 是单向数据流，react 中属性是不允许更改的，状态是允许更改的。
  react 中组件不允许通过 this.state 这种方式直接更改组件的状态。自身设置的状态，可以通过 setState 来进行更改。
  (注意：React 中 setState 是异步的，导致获取 dom 可能拿的还是之前的内容，
  所以我们需要在 setState 第二个参数（回调函数）)

- diff 算法
  vue 中 diff 算法实现流程：

  - 在内存中构建虚拟 dom 树 。
  - 将内存中虚拟 dom 树渲染成真实 dom 结构
  - 数据改变的时候，将之前的虚拟 dom 树结合新的数据生成新的虚拟 dom 树
  - 将此次生成好的虚拟 dom 树和上一次的虚拟 dom 树进行一次比对(diff 算法进行比对)，来更新只需要被替换的 DOM，
    而不是全部重绘。在 Diff 算法中，只平层的比较前后两棵 DOM 树的节点，没有进行深度的遍历。
  - 会将对比出来的差异进行重新渲染

  react 中 diff 算法实现流程:

  DOM 结构发生改变-----直接卸载并重新 create
  DOM 结构一样-----不会卸载,但是会 update 变化的内容
  所有同一层级的子节点.他们都可以通过 key 来区分-----同时遵循 1.2 两点
  (其实这个 key 的存在与否只会影响 diff 算法的复杂度,换言之,你不加 key 的情况下,
  diff 算法就会以暴力的方式去根据一二的策略更新,但是你加了 key,diff 算法会引入一些另外的操作)

## 事件循环

首先执行同步代码
当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行
执行所有微任务
当执行完所有微任务后，如有必要会渲染页面
然后开始下一轮 Event Loop，执行宏任务中的异步代码，也就是 setTimeout 中的回调函数

## 执行栈

可以把执行栈认为是一个存储函数调用的栈结构，它遵从先进后出的原则。
当开始执行 JS 代码时，首先会执行一个 main 函数，然后执行我们的代码。根据先进后出的原则，后执行的函数会先弹出栈。
