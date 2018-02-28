<!-- TOC -->

- [事件委托](#事件委托)
- [this在javascript中是如何工作的](#this在javascript中是如何工作的)
- [原型链](#原型链)
- [AMD vs CommonJS](#amd-vs-commonjs)
- [`function foo(){ }()`报错的原因以及解决方案](#function-foo-报错的原因以及解决方案)
- [null,undefined之间的区别？以及怎样检查这些状态](#nullundefined之间的区别以及怎样检查这些状态)
        - [undefined](#undefined)
        - [null](#null)
- [什么是闭包以及使用场景](#什么是闭包以及使用场景)
- [.forEach 和 .map() 的区别以及选择其中之一的理由](#foreach-和-map-的区别以及选择其中之一的理由)
        - [forEach](#foreach)
        - [.map()](#map)
- [匿名函数的使用场景](#匿名函数的使用场景)
        - [生成局部变量，防止污染全局](#生成局部变量防止污染全局)
        - [作为回调函数，一次性使用](#作为回调函数一次性使用)
        - [函数式编程结构中的参数](#函数式编程结构中的参数)
- [宿主对象和原生对象的区别](#宿主对象和原生对象的区别)
- [`function Person(){}`, `var person = Person()`, and `var person = new Person()`之间的区别](#function-person-var-person--person-and-var-person--new-person之间的区别)
- [call和apply区别](#call和apply区别)
- [说明Function.prototype.bind](#说明functionprototypebind)
- [特征检测，特征推断与UA字符串](#特征检测特征推断与ua字符串)
- [详细的介绍ajax](#详细的介绍ajax)
    - [优点](#优点)
    - [劣势](#劣势)
    - [为啥jsonp并不是ajax](#为啥jsonp并不是ajax)
- [变量提升](#变量提升)
- [attribute与property区别](#attribute与property区别)
- [`load` 事件 和 `DOMContentLoaded`事件的区别?](#load-事件-和-domcontentloaded事件的区别)
- [严格模式的优点](#严格模式的优点)
- [遍历对象与数组](#遍历对象与数组)
        - [对象](#对象)
        - [数组](#数组)
- [高阶函数](#高阶函数)
- [柯里化](#柯里化)
        - [参数复用](#参数复用)
        - [提前返回](#提前返回)
        - [相关扩展](#相关扩展)

<!-- /TOC -->

## 事件委托

在父元素上增加监听事件而不是在每个子元素上增加监听，通过事件的冒泡进行相应的处理

优势
- 内存占用下降（只监听了一个元素）
- 移除元素的时候不需要解绑，新增元素的时候不需要重新绑定


## this在javascript中是如何工作的

- new一个构造函数，函数内部的this指向新生成的那个对象
- 使用了apply，call或者bind，则函数内部的this指向该方法传入的第一个参数
- 对象内部的方法，通过对象去调用（`obj.method`），方法内的this则指向该对象
- 函数内部的this（非以上情况） 在浏览器环境下指向window对象，在严格模式下，则会指向`undefined`
- 如果是箭头函数，则忽略上述规则，this为创建时确认的值


## 原型链

- 所有的javascript对象都有一个prototype属性
- 当一个对象的属性被访问时，如果没有找到该属性，则会去对象的prototype上查找，如果再没找到，会通过原型链一层一层向上查找,直到找到该属性，或者到达原型链的终端


## AMD vs CommonJS

- 都是模块化处理方案
- 模块加载时，CommonJs同步，AMD异步（CommonJs偏向后台服务，AMD偏向浏览器）
- ES2015支持同步和异步加载（ES6大法好）

## `function foo(){ }()`报错的原因以及解决方案

- JavaScript解析`function foo(){ }()`时，会解析成`function foo(){ }` and `()`。前者是一个函数声明，后者（一对括号）是尝试调用一个函数，但没有指定名称，因此它会引发语法错误`Uncaught SyntaxError：Unexpected token）`。
- 解决方案 `(function foo(){ })()` and `(function foo(){ }())`


## null,undefined之间的区别？以及怎样检查这些状态

稳定的检查方案
```javascript
// 注：null == undefined 返回true
Object.prototype.toString.call(null)       // "[object Null]"
Object.prototype.toString.call(undefined)  // "[object Undefined]"
```

####  undefined
- 变量被声明了但是没有赋值
- 当函数没有返回值时的执行结果
- 检查方式：typeof比较undefined字符串，三等号比较

####  null
- 一个变量被明确的赋予了null这个值
- 检查方式：三等号比较

```javascript
var foo;
console.log(foo); // undefined
console.log(foo === undefined); // true
console.log(typeof foo === 'undefined'); // true

console.log(foo == null); // true. Wrong, don't use this to check!
console.log(foo === null); // false

function bar() {}
var baz = bar();
console.log(baz); // undefined
```

## 什么是闭包以及使用场景

- 在外部函数返回后，通过闭包依然可以访问函数内部的变量，该变量不会被自动回收。
- 场景：
 1. 模拟私有方法或者私有变量
 2. 柯里化函数

## .forEach 和 .map() 的区别以及选择其中之一的理由

####  forEach
- 遍历数组的每个元素
- 对每个元素执行回调函数
- 不返回值

####  .map()
- 遍历数组的每个元素
- 对每个元素调用函数生成一个新的结果数组

## 匿名函数的使用场景

####  生成局部变量，防止污染全局

```javascript
(function() {
  // Some code here.
})();
```

####  作为回调函数，一次性使用
```javascript
setTimeout(function() {
  console.log('Hello world!');
}, 1000);
```

####  函数式编程结构中的参数
```javascript
const arr = [1, 2, 3];
const double = arr.map(function(el) {
  return el * 2;
});
console.log(double); // [2, 4, 6]
```

## 宿主对象和原生对象的区别

- 原生对象是指js语言定义的对象，如`String`,`Math`,`RegExp`,`Object`,`Function`等
- 宿主对象则是通过运行环境提供（browser或者Node），如`window`,`XMLHTTPRequest`,`global`等

## `function Person(){}`, `var person = Person()`, and `var person = new Person()`之间的区别

- `function Person(){}`只是一个普通的函数声明，使用大驼峰命名是一种约定（预想这是一个构造函数）
- `var person = Person()` 将`Person`视作一个方法而不是一个构造函数。通常来说，构造函数不返回值，因此，这样做会返回`undefined`作为变量被赋值
- `var person = new Person()`通过`new`关键词生成一个`Person`对象的实例，并继承`Person.prototype`

```javascript
function Person(name) {
  this.name = name;
}

var person = Person('John');
console.log(person); // undefined
console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined

var person = new Person('John');
console.log(person); // Person { name: "John" }
console.log(person.name); // "john"
```

## call和apply区别
看代码吧

```javascript
function add(a, b) {
  return a + b;
}

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
```

## 说明Function.prototype.bind

参数传递方式参考call，创建了一个新的函数，可以指定函数的this与默认参数（偏函数）

## 特征检测，特征推断与UA字符串

- 特征检测: 确实存在该方法 `if('localStorage' in window){ localStorage.setItem('x','y') }`
- 特征推断：如果x功能存在，那么我们假定y功能也存在
```javascript
if('localStorage' in window){
  window.sessionStorage.setItem("this-should-exist-too", 1);
}
```

- 通过`navigator.userAgent`获取浏览器的信息，可编辑

## 详细的介绍ajax

通过使用`XMLHTTPRequest`或者`fetch`API向服务器发送一个异步请求，取回服务端数据,不会重新渲染整个页面

### 优点
- 无需重新渲染整个页面
- 减少服务器的请求，不需要重新请求样式和脚本
- 因为页面不会重新加载，所以js变量，dom状态将会被保留

### 劣势
- 想要支持书签需要进行额外处理
- js被关闭时，失效
- SEO

### 为啥jsonp并不是ajax

- 原理：先在全局设置一个方法，通过script跨域获取数据，将获取的数据作为参数传入全局方法内，最后执行该方法
- 替代方案CORS，区别
1. jsonp只能是get请求，cors不限制
2. 安全性：jsonp使用的get请求幂等，不会修改服务器数据，cors可以对域名进行限制
3. cors错误可以通过onerror捕获
4. 兼容性方面jsonp更优

```javascript
// https://mydomain.com
function printData(data) {
  console.log(`My name is ${data.name}!`);
}

<script src="https://example.com?callback=printData"></script>
// File loaded from https://example.com?callback=printData
printData({ name: 'Yang Shun' });
```

写个粗糙的jsonp
```javascript
var jsonp = function(name,callback){
  window[name] = function(data){
    callback(data)
    window[name] = null
  }
}

jsonp('printData',function(data){
  console.log(data)
})

printData({ name: 'Yang Shun' });
```

## 变量提升

```javascript
function x(foo){
  // ƒ foo() {console.log('FOOOOO');}
  // 如果去掉方法声明，则foo等于参数123
  // 再然后等于undefined
  console.log(foo); 
  var foo = 'a'
  function foo() {
    console.log('FOOOOO');
  }
  // 最终等于a
  console.log(foo)
}

x('123')


// 换种方式理解，上面那段代码等于
function x(foo){
  var foo
  foo = '参数' // 这里懂这个意思就行了
  function foo() {
    console.log('FOOOOO');
  }
  console.log(foo)
  foo = 'a'
  console.log(foo)
}

x('123')
```

## attribute与property区别

Attributes 通过HTML标记定义,而properties通过dom定义

```javascript
const input = document.querySelector('input');
console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello

// 改变输入后getAttribute获取的依旧是html上的属性，而.方法会获取dom属性
console.log(input.getAttribute('value')); // Hello
console.log(input.value); // Hello World!
```

## `load` 事件 和 `DOMContentLoaded`事件的区别?

- DOMContentLoaded 事件只要等页面加载解析完就会触发，不必等待样式图片完成加载
- load 事件只有在所有依赖资源都加载完之后才会触发

tip:
- css不阻塞页面的解析
- css会阻塞页面的渲染
- css会阻塞js的执行

## 严格模式的优点

建议查看[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)
- 防止意外创建全局变量 (不通过var的变量赋值)
- 将原有的一些静默失败抛出
- delete删除失败的情况会抛出错误（var a=1;delete a）
- 方法参数名称唯一
- this没有定义且指向全局
- 禁用了某些糟糕的功能

## 遍历对象与数组

#### 对象
```javascript
var obj = {a:1}
// for in 会遍历到继承到的属性
for(let key in obj){
  if(obj.hasOwnProperty(key)){
    console.log(key)
  }
}

// Object.keys(obj) 所有可枚举的属性
Object.keys(obj).forEach((item)=>{
  console.log(item)
})

// Object.getOwnPropertyNames(obj) 可枚举与不可枚举属性
Object.getOwnPropertyNames(obj).forEach((item)=>{
  console.log(item)
})
```

#### 数组
```javascript
// for 更灵活，可以使用break终止循环或者修改自增量
for (let i = 0; i < arr.length; i++)

// forEach 如果有终止需求，可以使用every或者some
arr.forEach(function (el, index) { ... })

// for ... of，不知道为啥原文没有
for (let item of arr)
```

## 高阶函数

高阶函数是将一个或多个函数作为参数的任何函数，常见例子：`map`

```javascript
// 自己实现个map
Array.prototype.myMap = function(func){
  var result = []
  for(var i=0;i<this.length;i++){
    var temp = func.call(null,this[i])
    result.push(temp)
  }
  
  return result
}

var f = [1,2,3].myMap(function(item){
  return item*2
})

console.log(f)
```

## 柯里化
参考：http://www.zhangxinxu.com/wordpress/2013/02/js-currying/

#### 参数复用
```javascript
var currying = function(fn) {
    // fn 指官员消化老婆的手段
    var args = [].slice.call(arguments, 1);
    // args 指的是那个合法老婆
    return function() {
        // 已经有的老婆和新搞定的老婆们合成一体，方便控制
        var newArgs = args.concat([].slice.call(arguments));
        // 这些老婆们用 fn 这个手段消化利用，完成韦小宝前辈的壮举并返回
        return fn.apply(null, newArgs);
    };
};

// 下为官员如何搞定7个老婆的测试
// 获得合法老婆
var getWife = currying(function() {
    var allWife = [].slice.call(arguments);
    // allwife 就是所有的老婆的，包括暗渡陈仓进来的老婆
    console.log(allWife.join(";"));
}, "合法老婆");

// 获得其他6个老婆
getWife("大老婆","小老婆","俏老婆","刁蛮老婆","乖老婆","送上门老婆");

// 换一批老婆
getWife("超越韦小宝的老婆");
```

#### 提前返回
```javascript
// bad
var addEvent = function(el, type, fn, capture) {
    if (window.addEventListener) {
        el.addEventListener(type, function(e) {
            fn.call(el, e);
        }, capture);
    } else if (window.attachEvent) {
        el.attachEvent("on" + type, function(e) {
            fn.call(el, e);
        });
    } 
};

// good
var addEvent = (function(){
    if (window.addEventListener) {
        return function(el, sType, fn, capture) {
            el.addEventListener(sType, function(e) {
                fn.call(el, e);
            }, (capture));
        };
    } else if (window.attachEvent) {
        return function(el, sType, fn, capture) {
            el.attachEvent("on" + sType, function(e) {
                fn.call(el, e);
            });
        };
    }
})();
```

#### 相关扩展
```javascript
// 实现个bind，参数复用
Function.prototype.myBind = function(context){
  var args = Array.prototype.slice.call(arguments,1);
  var that = this;
  return function(){
    args = args.concat(Array.prototype.slice.call(arguments))
    that.apply(context,args)
  }
}


var obj = {
  name : 'y'
}

var fn = function(val1,val2){
  console.log(this.name)
  console.log(val1)
  console.log(val2)
}.myBind(obj,'frist')

fn('second')
```