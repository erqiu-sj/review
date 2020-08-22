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

## 重绘和回流

1. 浏览器使用流式布局模型 (Flow Based Layout)。
2. 浏览器会把 HTML 解析成 DOM，把 CSS 解析成 CSSOM，DOM 和 CSSOM 合并就产生了 Render Tree。
3. 有了 RenderTree，我们就知道了所有节点的样式，然后计算他们在页面上的大小和位置，最后把节点绘制到页面上。
4. 由于浏览器使用流式布局，对 Render Tree 的计算通常只需要遍历一次就可以完成，但 table 及其内部元素除外，他们可能需要多次计算，通常要花 3 倍于同等元素的时间，这也是为什么要避免使用 table 布局的原因之一。

**一句话：回流必将引起重绘，重绘不一定会引起回流。**

- 回流

当 Render Tree 中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。

会导致回流的操作：

页面首次渲染
浏览器窗口大小发生改变
元素尺寸或位置发生改变
元素内容变化（文字数量或图片大小等等）
元素字体大小变化
添加或者删除可见的 DOM 元素
激活 CSS 伪类（例如：:hover）
查询某些属性或调用某些方法

一些常用且会导致回流的属性和方法：

clientWidth、clientHeight、clientTop、clientLeft
offsetWidth、offsetHeight、offsetTop、offsetLeft
scrollWidth、scrollHeight、scrollTop、scrollLeft
scrollIntoView()、scrollIntoViewIfNeeded()
getComputedStyle()
getBoundingClientRect()
scrollTo()

- 重绘

当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility 等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。

- 性能影响
  回流比重绘的代价要更高。
  有时即使仅仅回流一个单一的元素，它的父元素以及任何跟随它的元素也会产生回流。
  现代浏览器会对频繁的回流或重绘操作进行优化：
  浏览器会维护一个队列，把所有引起回流和重绘的操作放入队列中，如果队列中的任务数量或者时间间隔达到一个阈值的，浏览器就会将队列清空，进行一次批处理，这样可以把多次回流和重绘变成一次。

- CSS
  避免使用 table 布局。
  尽可能在 DOM 树的最末端改变 class。
  避免设置多层内联样式。
  将动画效果应用到 position 属性为 absolute 或 fixed 的元素上。
  避免使用 CSS 表达式（例如：calc()）。

- JavaScript
  避免频繁操作样式，最好一次性重写 style 属性，或者将样式列表定义为 class 并一次性更改 class 属性。
  避免频繁操作 DOM，创建一个 documentFragment，在它上面应用所有 DOM 操作，最后再把它添加到文档中。
  也可以先为元素设置 display: none，操作结束后再把它显示出来。因为在 display 属性为 none 的元素上进行的 DOM 操作不会引发回流和重绘。
  避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
  对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。

# Vue

## Vue 的优点

- 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。

- 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。

- 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计

## Vue 生命周期作用是什么

它的生命周期中有多个事件钩子，让我们在控制整个 Vue 实例的过程时更容易形成好的逻辑

## vue-loader 是什么？使用它的用途有哪些？

解析.vue 文件的一个加载器，跟 template/js/style 转换成 js 模块。 用途：js 可以写 es6、style 样式可以 scss 或 less、template 可以加 jade 等

## vuex 是什么

vue 框架中状态管理

## 哪种功能场景使用它（指 Vuex

场景有：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车

## vue 的双向绑定的原理是什么

vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

## vuex 的 store 特性是什么

- state 里面存放的数据是响应式的，vue 组件从 store 读取数据，若是 store 中的数据发生改变，依赖这相数据的组件也会发生更新
- 它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性

## 不用 vuex 会带来什么问题

- 可维护性会下降，你要修改数据，你得维护 3 个地方
- 可读性下降，因为一个组件里的数据，你根本就看不出来是从哪里来的
- 增加耦合，大量的上传派发，会让耦合性大大的增加，本来 Vue 用 Component 就是为了减少耦合，现在这么用，和组件化的初衷相背

# React

## React 性能优化

- 在页面中使用了 setTimout()、addEventListener()等，要及时在 componentWillUnmount()中销毁在页面中使用了 setTimout()、addEventListener()等，要及时在 componentWillUnmount()中销毁
- 使用异步组件
- 使用 React-loadable 动态加载组件
- shouldComponentUpdate(简称 SCU )、React.PureComponent、React.memo
- 不可变值 ImmutableJS

## PureComponent 和 memo

- class 类组件中用 PureComponent，无状态组件(无状态)中用 memo
- PureComponent, SCU 中实现了浅比较 s
- 浅比较已使用大部分情况（尽量不要做深度比较）

> PureComponent 与普通 Component 不同的地方在于，PureComponent 自带了一个 shouldComponentUpdate()，并且进行了浅比较

## 组件生命周期

### Initialization 初始化

- constructor() : class 的构造函数，并非 React 独有

### Mounting 挂载

- componentWillMount() : 在组件即将被挂载到页面的时刻自动执行；
- render() : 页面挂载；
- componentDidMount() : 组件被挂载到页面之后自动执行；

componentWillMount() 和 componentDidMount()，只会在页面第一次挂载的时候执行，state 变化时，不会重新执行

### Updation 组件更新

- shouldComponentUpdate() : 该生命周期要求返回一个 bool 类型的结果，如果返回 true 组件正常更新，如果返回 false 组件将不会更新；
- componentWillUpdate() : 组件被更新之前执行，如果 shouldComponentUpdate()返回 false，将不会被被执行；
- componentDidUpdate() : 组件更新完成之后执行；

- componentWillReceiveProps() : props 独有的生命周期，执行条件如下：

- 组件要从父组件接收参数；
  只要父组件的 render()被执行了，子组件的该生命周期就会执行；
  如果这个组件第一次存在于父组件中，不会执行；
  如果这个组件之前已经存在于父组件中，才会执行；

### Unmounting 组件卸载

- componentWillUnmount() : 当组件即将被从页面中剔除的时候，会被执行

### 重点 shouldComponentUpdate()

`使用 shouldComponentUpdate()防止页面进行不必要的渲染`

```
# 用生命周期进行性能优化
shouldComponentUpdate () {
    if (nextProps.content !== this.props.content) {
      return true;
    }
    return false;
}
```

## 无状态组件(函数组件)

当一个组件只有一个 render()函数时，我们就可将这个组件定义为无状态组件，无状态组件只有一个函数。
无状态组件的性能比较高，因为它仅是一个函数，而普通组件是一个 class。

## 异步组件

```
// 引入需要异步加载的组件
const LazyComponent = React.lazy(() => import('./lazyDemo') )

// 使用异步组件，异步组件加载中时，显示fallback中的内容
<React.Suspense fallback={<div>异步组件加载中</div>}>
    <LazyComponent />
</React.Suspense>

```

## Render Props

Render Props 核心思想：通过一个函数将 class 组件的 state 作为 props 传递给纯函数组件

```
class Factory extends React.Component {
    constructor () {
        this.state = {
            /* 这里 state 即多个组件的公共逻辑的数据 */
        }
    }
    /* 修改 state */
    render () {
        return <div>{this.props.render(this.state)}</div>
    }
}

const App = () => {
    /* render 是一个函数组件 */
    <Factory render={
        (props) => <p>{props.a} {props.b}...</p>
    } />
}

```

## JSX 本质

- JSX 等同于 Vue 模板
- Vue 模板不是 html
- JSX 也不是 JS

讲 JSX 语法，通过 React.createElement()编译成 Dom，BABEL 可以编译 JSX
流程：JSX => React.createElement() => 虚拟 DOM (JS 对象) => 真实 DOM
React 底层会通过 React.createElement() 这个方法，将 JSX 语法转成 JS 对象，React.createElement() 可以接收三个参数，第一个为标签名称，第二参数为属性，第三个参数为内容

```
createElement() 根据首字母大小写来区分是组件还是HTML标签，React规定组件首字母必须大写，HTML规定标签首字母必须小写

// 第一个参数为 标签(tag) 可为 'div'标签名 或 List组件
// 第二个参数为：属性(props)
// 第三个参数之后都为子节点(child)，可以在第三个参数传一个数组，也可以在第三、四、五....参数中传入
React.createElement('tag', null, [child1, chlild2, child3])
或者
React.createElement('tag', { className: 'class1' }, child1, chlild2, child3)

```

## Url 到页面发生了什么

先是 URl-》Dns 解析获取服务器 ip 地址，端口-》利用 ip 地址和服务器建立 Tcp 连接，构建请求头信息，发送请求头信息-》HTTp-》响应解析（SSr，spa）,再到浏览器渲染

## React_setState

- 生命周期和合成事件中
  在 React 的生命周期和合成事件中， React 仍然处于他的更新机制中，这时无论调用多少次 setState，都会不会立即执行更新，而是将要更新的·存入 \_pendingStateQueue，将要更新的组件存入 dirtyComponent(脏组件)。
  当上一次更新机制执行完毕，以生命周期为例，所有组件，即最顶层组件 didmount 后会将批处理标志设置为 false。这时将取出 dirtyComponent 中的组件以及 \_pendingStateQueue 中的 state 进行更新。这样就可以确保组件不会被重新渲染多次。

  ```
    componentDidMount() {
  this.setState({
      index: this.state.index + 1
  })
    console.log('state', this.state.index);
  }
  ```

  所以，如上面的代码，当我们在执行 setState 后立即去获取 state，这时是获取不到更新后的 state 的，因为处于 React 的批处理机制中， state 被暂存起来，待批处理机制完成之后，统一进行更新。
  所以。setState 本身并不是异步的，而是 React 的批处理机制给人一种异步的假象

- 异步代码和原生事件中

```
componentDidMount() {
    setTimeout(() => {
      console.log('调用setState');
this.setState({
        index: this.state.index + 1
})
      console.log('state', this.state.index);
}, 0);
}
```

如上面的代码，当我们在异步代码中调用 setState 时，根据 JavaScript 的异步机制，会将异步代码先暂存，等所有同步代码执行完毕后在执行，这时 React 的批处理机制已经走完，处理标志设被设置为 false，这时再调用 setState 即可立即执行更新，拿到更新后的结果。
在原生事件中调用 setState 并不会出发 React 的批处理机制，所以立即能拿到最新结

- 最佳实践
  setState 的第二个参数接收一个函数，该函数会在 React 的批处理机制完成之后调用，所以你想在调用 setState 后立即获取更新后的值，请在该回调函数中获取。
  ```
  this.setState({ index: this.state.index + 1 }, () => {
      console.log(this.state.index);
  })
  ```

## React 如何实现自己的事件机制

React 事件并没有绑定在真实的 Dom 节点上，而是通过事件代理，在最外层的 document 上对事件进行统一分发。

- 组件挂载、更新时
  通过 lastProps、 nextProps 判断是否新增、删除事件分别调用事件注册、卸载方法。
  调用 EventPluginHub 的 enqueuePutListener 进行事件存储
  获取 document 对象。
  根据事件名称（如 onClick、 onCaptureClick）判断是进行冒泡还是捕获。
  判断是否存在 addEventListener 方法，否则使用 attachEvent（兼容 IE）。
  给 document 注册原生事件回调为 dispatchEvent(统一的事件分发机制）
- 事件初始化
  EventPluginHub 负责管理 React 合成事件的 callback，它将 callback 存储在 listenerBank 中，另外还存储了负责合成事件的 Plugin。
  获取绑定事件的元素的唯一标识 key。
  将 callback 根据事件类型，元素的唯一标识 key 存储在 listenerBank 中。
  listenerBank 的结构是： listenerBank[registrationName][key]。
- 触发事件时
  触发 document 注册原生事件的回调 dispatchEvent
  获取到触发这个事件最深一级的元素
  遍历这个元素的所有父元素，依次对每一级元素进行处理。
  构造合成事件。
  将每一级的合成事件存储在 eventQueue 事件队列中。
  遍历 eventQueue。
  通过 isPropagationStopped 判断当前事件是否执行了阻止冒泡方法。
  如果阻止了冒泡，停止遍历，否则通过 executeDispatch 执行合成事件。
  释放处理完成的事件。
  `React在自己的合成事件中重写了 stopPropagation方法，将 isPropagationStopped设置为 true，然后在遍历每一级事件的过程中根据此遍历判断是否继续执行。这就是 React自己实现的冒泡机制。`

## 为何 React 事件要自己绑定 this？

在上面提到的事件处理流程中， React 在 document 上进行统一的事件分发， dispatchEvent 通过循环调用所有层级的事件来模拟事件冒泡和捕获。
在 React 源码中，当具体到某一事件处理函数将要调用时，将调用 invokeGuardedCallback 方法。

```
function invokeGuardedCallback(name, func, a) {
try {
    func(a);
} catch (x) {
if (caughtError === null) {
      caughtError = x;
}
}
}
```

可见，事件处理函数是直接调用的，并没有指定调用的组件，所以不进行手动绑定的情况下直接获取到的 this 是不准确的，所以我们需要手动将当前组件绑定到 this 上

## 原生事件和 React 事件的区别？

React 事件使用驼峰命名，而不是全部小写。
通过 JSX , 你传递一个函数作为事件处理程序，而不是一个字符串。
在 React 中你不能通过返回 false 来阻止默认行为。必须明确调用 preventDefault。

## React 和原生事件的执行顺序是什么？可以混用吗

React 的所有事件都通过 document 进行统一分发。当真实 Dom 触发事件后冒泡到 document 后才会对 React 事件进行处理。
所以原生的事件会先执行，然后执行 React 合成事件，最后执行真正在 document 上挂载的事件
React 事件和原生事件最好不要混用。原生事件中如果执行了 stopPropagation 方法，则会导致其他 React 事件失效。因为所有元素的事件将无法冒泡到 document 上，导致所有的 React 事件都将无法被触发。。

## 虚拟 Dom 比普通 Dom 更快吗

很多文章说 VitrualDom 可以提升性能，这一说法实际上是很片面的。
直接操作 DOM 是非常耗费性能的，这一点毋庸置疑。但是 React 使用 VitrualDom 也是无法避免操作 DOM 的。
如果是首次渲染， VitrualDom 不具有任何优势，甚至它要进行更多的计算，消耗更多的内存。
VitrualDom 的优势在于 React 的 Diff 算法和批处理策略， React 在页面更新之前，提前计算好了如何进行更新和渲染 DOM。实际上，这个计算过程我们在直接操作 DOM 时，也是可以自己判断和实现的，但是一定会耗费非常多的精力和时间，而且往往我们自己做的是不如 React 好的。所以，在这个过程中 React 帮助我们"提升了性能"。
所以，我更倾向于说， VitrualDom 帮助我们提高了开发效率，在重复渲染时它帮助我们计算如何更高效的更新，而不是它比 DOM 操作更快。

## 列表渲染为何要用 key

- diff 算法中通过 key 判断，是否是同一个节点
- 减少渲染次数，提升渲染性能

## React 组件的渲染流程是什么？

- 使用 React.createElement 或 JSX 编写 React 组件，实际上所有的 JSX 代码最后都会转换成 React.createElement(...)， Babel 帮助我们完成了这个转换的过程。

- createElement 函数对 key 和 ref 等特殊的 props 进行处理，并获取 defaultProps 对默认 props 进行赋值，并且对传入的孩子节点进行处理，最终构造成一个 ReactElement 对象（所谓的虚拟 DOM）。

- ReactDOM.render 将生成好的虚拟 DOM 渲染到指定容器上，其中采用了批处理、事务等机制并且对特定浏览器进行了性能优化，最终转换为真实 DOM。

## 为什么 React 组件首字母必须大写？

babel 在编译时会判断 JSX 中组件的首字母，当首字母为小写时，其被认定为原生 DOM 标签， createElement 的第一个变量被编译为字符串；当首字母为大写时，其被认定为自定义组件， createElement 的第一个变量被编译为对象；

## 函数组件 和 class 组件区别

- 纯函数，输入 props，输出 JSX
- 没有实力，没有生命周期，没有 state
- 不能扩展其它方法

## React 性能优化

- 渲染列表时加 Key
- 自定义事件、DOM 事件及时销毁
- 合理使用异步组件
- 减少函数 bind this 的次数
- 合理使用 shouldComponentUpdate、PureComponent 和 memo
- 合理使用 ImmutableJS
- webpack 层面优化
- 前端通用是能优化，如图片懒加载
- 使用 SSR

## React 和 Vue 的区别

### 相同点

- 都支持组件化
- 都是数据驱动视图
- 都是用 vdom 操作 DOM

### 不同点

- React 使用 JSX 拥抱 JS，Vue 使用模板拥抱 html
- React 函数式编程，Vue 声明式编程
- React 更多需要自力更生，Vue 把想要的都给你

## 值类型和引用类型的区别

引用类型的本质是相同的内存地址，出于性能问题考虑，所以 JS 对象使用引用类型，为了避免这种情况所以需要深拷贝

## viewport 设置

`<meta name='viewport' content='width=device-width,initial-scale=1,user-scale=no' />`

## 为什么要设置 viewport

viewport 的设置不会对 PC 页面产生影响，但对于移动页面却很重要。下面我们举例来说明：

媒体查询 @media

- 响应式布局中，会根据媒体查询功能来适配多端布局，必须对 viewport 进行设置，否则根据查询到的尺寸无法正确匹配视觉宽度而导致布局混乱。如不设置 viewport 参数，多说移动端媒体查询的结果将是 980px 这个节点布局的参数，而非我们通常设置的 768px 范围内的这个布局参数
- 由于目前多数手机的 dpr 都不再是 1，为了产出高保真页面，我们一般会给出 750px 的设计稿，那么就需要通过设置 viewport 的参数来进行整体换算，而不是在每次设置尺寸时进行长度的换算。

```

```
