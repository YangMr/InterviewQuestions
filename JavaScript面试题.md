# JavaScript面试题

## 第一天（2019-12-05）

### 1. 简述同步与异步的区别

- 同步：浏览器访问服务器请求，用户看得到页面刷新，重新发请求,等请求完，页面刷新，新内容出现，用户看到新内容,进行下一步操作
- 异步：浏览器访问服务器请求，用户正常操作，浏览器后端进行请求。等请求完，页面不刷新，新内容也会出现，用户看到新内容

### 2. 怎么添加、移除、复制、创建、和查找节点

- 创建新节点

  - createDocumentFragment()  //创建一个DOM片段

  - createElement()  //创建一个具体的元素

  - createTextNode()  //创建一个文本节点

- 添加、移除、替换、插入

  - appendChild()
  - removeChild()
  - replaceChild()
  - insertBefore()

- 查找

  - getElementsByTagName()  //通过标签名称
  - getElementsByName()  //通过元素的Name属性的值
  - getElementById()  //通过元素Id，唯一性

### 3. 如何消除一个数组里面重复的元素

```javascript
function qc(arr1){
	let arr = [];
	for( let i = 0; i < arr1.length; i++) {
		if( arr.indexOf(arr1[i]) == -1) {
			arr.push(arr1[i])
		}
	}
	return arr;
}
arr1 = ["1","1","3","5","2","24","4","4","a","a","b"];
		
console.log(qc(arr1)); 
```

### 4. 什么是闭包？

​		**闭包**（closure）是Javascript语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现。闭包就是能够读取其他函数内部变量的函数。可以把闭包简单理解成”定义在一个函数内部的函数”。

### 5. 闭包的特性？

1. 函数嵌套函数;
2. 函数内部可以引用外部的参数和变量;
3. 参数和变量不会被垃圾回收机制回收。

### 6. 使用闭包有什么好处？

1. 希望一个变量长期驻扎在内存中
2. 避免全局变量的污染
3. 私有成员的存在

### 7. 写一个返回闭包的函数

​		闭包就是一个函数的返回值为另外一个函数，在outer外部可以通过这个返回的函数访问outer内的局部变量.

```javascript
function outer(){
     var val = 0;
     return function (){
           val += 1;
           document.write(val + "<br />");
     };
}
var outObj = outer();
outObj();//1，执行val += 1后，val还在
outObj();//2
outObj = null;//val 被回收
var outObj1 = outer();
outObj1();//1
outObj1();//2
```



### 8.  JavaScript垃圾回收原理？

- 在javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收；

- 如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。

### 9. JavaScript有哪几种数据类型？

#### 基本数据类型

- 字符串 String
- 数字 Number
- 布尔 Boolean

#### 复合数据类型

- 数组 Array
- 对象 Object

#### 特殊数据类型

- Null 空对象
- Undefined 未定义

### 10. 请描述值类型(基本数据类型)和引用类型的区别？

- 值类型
  - 占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，`基础变量的值是存储在栈中`，而引用变量存储在栈中的是`指向堆中的数组或者对象的地址`，这就是为何修改引用类型总会影响到其他指向这个地址的引用变量。）
  - 保存与复制的是值本身
  - 使用typeof检测数据的类型
  - 基本类型数据是值类型
- 引用类型
  - 占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象`依然不会被销毁`，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。）
  - 保存与复制的是指向对象的一个指针
  - 使用instanceof检测数据类型
  - 使用new()方法构造出的对象是引用型

参考地址： https://www.cnblogs.com/leiting/p/8081413.html

## 第二天（2019-12-06）

### 1. 如何正确判断 this？箭头函数的 this 是什么？

​		`this`是很多人会混淆的概念，但是其实它一点都不难，只是网上很多文章把简单的东西说复杂了。在这一小节中，你一定会彻底明白`this`这个概念的。

​		我们先来看几个函数调用的场景：

```javascript
function foo() {
  console.log(this.a)
}
var a = 1

foo()

const obj = {
  a: 2,
  foo: foo
}

obj.foo()

const c = new foo()
```

接下来我们一个个分析上面几个场景:

- 对于直接调用`foo`来说，不管`foo`函数被放在了什么地方，`this`一定是`window`
- 对于`obj.foo()`来说，我们只需要记住，谁调用了函数，谁就是`this`，所以在这个场景下`foo`函数中的`this`就是`obj`对象
- 对于`new`的方式来说，`this`被永远绑定在了`c`上面，不会被任何方式改变`this`

说完了以上几种情况，其实很多代码中的`this`应该就没什么问题了，下面让我们看看箭头函数中的`this`

```javascript
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
console.log(a()()())
```

首先箭头函数其实是没有`this`的，箭头函数中的`this`只取决包裹箭头函数的第一个普通函数的`this`。在这个例子中，因为包裹箭头函数的第一个普通函数是`a`，所以此时的`this`是`window`。另外对箭头函数使用`bind`这类函数是无效的。

最后种情况也就是`bind`这些改变上下文的 API 了，对于这些函数来说，`this`取决于第一个参数，如果第一个参数为空，那么就是`window`。

那么说到`bind`，不知道大家是否考虑过，如果对一个函数进行多次`bind`，那么上下文会是什么呢？

```javascript
let a = {}
let fn = function () { console.log(this) }
fn.bind().bind(a)() // => ?
```

如果你认为输出结果是`a`，那么你就错了，其实我们可以把上述代码转换成另一种形式:

```javascript
// fn.bind().bind(a) 等于
let fn2 = function fn1() {
  return function() {
    return fn.apply()
  }.apply(a)
}
fn2()
```

可以从上述代码中发现，不管我们给函数`bind`几次，`fn`中的`this`永远由第一次`bind`决定，所以结果永远是`window`。

```javascript
let a = { name: 'vue' }
function foo() {
  console.log(this.name)
}
foo.bind(a)() // => 'vue'
```

以上就是`this`的规则了，但是可能会发生多个规则同时出现的情况，这时候不同的规则之间会根据优先级最高的来决定`this`最终指向哪里。

### 2. 深拷贝和浅拷贝的区别？如何实现

**浅拷贝**只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。浅拷贝只复制对象的第一层属性
但**深拷贝**会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。对对象的属性进行递归复制

#### 浅拷贝 实现方法

**1、可以通过简单的赋值实现**

类似上面的例子，当然，我们也可以封装一个简单的函数，如下：

```
 function simpleClone(initalObj) {    
      var obj = {};    
      for ( var i in initalObj) {
        obj[i] = initalObj[i];
      }    
      return obj;
    }

    var obj = {
      a: "hello",
      b:{
          a: "world",
          b: 21
        },
      c:["Bob", "Tom", "Jenny"],
      d:function() {
          alert("hello world");
        }
    }
    var cloneObj = simpleClone(obj); 
    console.log(cloneObj.b); 
    console.log(cloneObj.c);
    console.log(cloneObj.d);

    cloneObj.b.a = "changed";
    cloneObj.c = [1, 2, 3];
    cloneObj.d = function() { alert("changed"); };
    console.log(obj.b);
    console.log(obj.c);
    console.log(obj.d);
```

**2、Object.assign()实现**

Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign() 进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。

```
var obj = { a: {a: "hello", b: 21} };

var initalObj = Object.assign({}, obj);

initalObj.a.a = "changed";

console.log(obj.a.a); //  "changed"
```

注意：当object只有一层的时候，是深拷贝，例如如下：

```
var obj1 = { a: 10, b: 20, c: 30 };
var obj2 = Object.assign({}, obj1);
obj2.b = 100;
console.log(obj1);
// { a: 10, b: 20, c: 30 } <-- 沒被改到
console.log(obj2);
// { a: 10, b: 100, c: 30 }
```

#### 深拷贝的实现方式

**1、对象只有一层的话可以使用上面的：Object.assign()函数**

**2、转成 JSON 再转回来**

```
var obj1 = { body: { a: 10 } };
var obj2 = JSON.parse(JSON.stringify(obj1));
obj2.body.a = 20;
console.log(obj1);
// { body: { a: 10 } } <-- 沒被改到
console.log(obj2);
// { body: { a: 20 } }
console.log(obj1 === obj2);
// false
console.log(obj1.body === obj2.body);
// false
```

用JSON.stringify把对象转成字符串，再用JSON.parse把字符串转成新的对象。

可以封装如下函数

```
var cloneObj = function(obj){
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if(window.JSON){
        str = JSON.stringify(obj), //系列化对象
        newobj = JSON.parse(str); //还原
    } else {
        for(var i in obj){
            newobj[i] = typeof obj[i] === 'object' ? 
            cloneObj(obj[i]) : obj[i]; 
        }
    }
    return newobj;
};
```

**3、递归拷贝**

```
function deepClone(initalObj, finalObj) {    
  var obj = finalObj || {};    
  for (var i in initalObj) {        
    var prop = initalObj[i];        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
    if(prop === obj) {            
      continue;
    }        
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : {};            
      arguments.callee(prop, obj[i]);
    } else {
      obj[i] = prop;
    }
  }    
  return obj;
}
var str = {};
var obj = { a: {a: "hello", b: 21} };
deepClone(obj, str);
console.log(str.a);
```

**4、使用Object.create()方法**

直接使用var newObj = Object.create(oldObj)，可以达到深拷贝的效果。

```
function deepClone(initalObj, finalObj) {    
  var obj = finalObj || {};    
  for (var i in initalObj) {        
    var prop = initalObj[i];        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
    if(prop === obj) {            
      continue;
    }        
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : Object.create(prop);
    } else {
      obj[i] = prop;
    }
  }    
  return obj;
}
```

**5、jquery**

jquery 有提供一个$.extend可以用来做 Deep Copy。

```
var $ = require('jquery');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = $.extend(true, {}, obj1);
console.log(obj1.b.f === obj2.b.f);
// false
```

**6、lodash**

另外一个很热门的函数库lodash，也有提供_.cloneDeep用来做 Deep Copy。

```
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);
// false
```

这个性能还不错，使用起来也很简单。

### 3.  对前端路由的理解？前后端路由的区别？

前端的路由和后端的路由在实现技术上不一样，但是原理都是一样的。在 HTML5 的 history API 出现之前，前端的路由都是通过 hash 来实现的，hash 能兼容低版本的浏览器。

```
http://10.0.0.1/
http://10.0.0.1/#/about
http://10.0.0.1/#/concat
```

**服务端路由**：每跳转到不同的URL，都是重新访问服务端，然后服务端返回页面，页面也可以是服务端获取数据，然后和模板组合，返回HTML，也可以是直接返回模板HTML，然后由前端JS再去请求数据，使用前端模板和数据进行组合，生成想要的HTML。

**前端路由**：每跳转到不同的URL都是使用前端的锚点路由，实际上只是JS根据URL来操作DOM元素，根据每个页面需要的去服务端请求数据，返回数据后和模板进行组合，当然模板有可能是请求服务端返回的，这就是 SPA 单页程序。

在js可以通过window.location.hash读取到路径加以解析之后就可以响应不同路径的逻辑处理。

history 是 HTML5 才有的新 API，可以用来操作浏览器的 session history (会话历史)。基于 history 来实现的路由可以和最初的例子中提到的路径规则一样。

H5还新增了一个hashchange事件，也是很有用途的一个新事件：

当页面hash(#)变化时，即会触发hashchange。锚点Hash起到引导浏览器将这次记录推入历史记录栈顶的作用，window.location对象处理“#”的改变并不会重新加载页面，而是将之当成新页面，放入历史栈里。并且，当前进或者后退或者触发hashchange事件时，我们可以在对应的事件处理函数中注册ajax等操作！
但是hashchange这个事件不是每个浏览器都有，低级浏览器需要用轮询检测URL是否在变化，来检测锚点的变化。当锚点内容(location.hash)被操作时，如果锚点内容发生改变浏览器才会将其放入历史栈中，如果锚点内容没发生变化，历史栈并不会增加，并且也不会触发hashchange事件。

### 4. 浏览器是如何渲染页面的？

渲染的流程如下：

1.解析HTML文件，创建DOM树。

自上而下，遇到任何样式（link、style）与脚本（script）都会阻塞（外部样式不阻塞后续外部脚本的加载）。

2.解析CSS。优先级：浏览器默认设置<用户设置<外部样式<内联样式<HTML中的style样式；

3.将CSS与DOM合并，构建渲染树（Render Tree）

4.布局和绘制，重绘（repaint）和重排（reflow）

### 5. 什么是JavaScript原型，原型链 ? 有什么特点？

- 每个对象都会在其内部初始化一个属性，就是`prototype`(原型)，当我们访问一个对象的属性时
- 如果这个对象内部不存在这个属性，那么他就会去`prototype`里找这个属性，这`个prototype`又会有自己的`prototype`，于是就这样一直找下去，也就是我们平时所说的原型链的概念
- 关系：`instance.constructor.prototype = instance.__proto__`
- 特点：
  - `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变
- 当我们需要一个属性的时，`Javascript`引擎会先看当前对象中是否有这个属性， 如果没有的
- 就会查找他的`Prototype`对象是否有这个属性，如此递推下去，一直检索到`Object`内建对象

### 6. 如何改变函数内部的this指针的指向？

#### 6.1 bind：

- fun.bind(thisArg[, arg1[, arg2[, …]]])
- 他是直接改变这个函数的this指向并且返回一个新的函数，之后再次调用这个函数的时候this都是指向bind绑定的第一个参数。bind传餐方式跟call方法一致。
- thisArg 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。
- arg1, arg2, … 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。

#### 6.2 call:

- fun.call(thisArg, arg1, arg2, …)
- call跟apply的用法几乎一样，唯一的不同就是传递的参数不同，call只能一个参数一个参数的传入。
- thisArg: 在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于非严格模式下，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
- arg1, arg2, … 指定的参数列表

#### 6.3 apply

- apply则只支持传入一个数组，哪怕是一个参数也要是数组形式。最终调用函数时候这个数组会拆成一个个参数分别传入。
- thisArg 在 fun 函数运行时指定的 this 值。需要注意的是，指定的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的自动包装对象。
- argsArray 一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。

#### 6.4 总结

- 当我们使用一个函数需要改变this指向的时候才会用到callapplybind
- 如果你要传递的参数不多，则可以使用fn.call(thisObj, arg1, arg2 …)
- 如果你要传递的参数很多，则可以用数组将参数整理好调用fn.apply(thisObj, [arg1, arg2 …])
  如果你想生成一个新的函数长期绑定某个函数给某个对象使用，则可以使用const newFn = fn.bind(thisObj); newFn(arg1, arg2…)
- call和apply第一个参数为null/undefined，函数this指向全局对象，在浏览器中是window，在node中是global

### 7. json和jsonp的区别?

json返回的是一串json格式数据；而jsonp返回的是脚本代码（包含一个函数调用）

jsonp的全名叫做json with padding，就是把json对象用符合js语法的形式包裹起来以使其他的网站可以请求到，也就是将json封装成js文件传过去。

### 8. 如何阻止冒泡？

冒泡型事件：事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发。

w3c的方法是e.stopPropagation()，IE则是使用e.cancelBubble = true。

```
//阻止冒泡行为 
function stopBubble(e) { 
//如果提供了事件对象，则这是一个非IE浏览器 
if ( e && e.stopPropagation ) 
    //因此它支持W3C的stopPropagation()方法 
    e.stopPropagation(); 
else 
    //否则，我们需要使用IE的方式来取消事件冒泡 
    window.event.cancelBubble = true; 
}
```

### 9. 如何阻止默认事件？

w3c的方法是e.preventDefault()，IE则是使用e.returnValue = false

```
//阻止浏览器的默认行为 
function stopDefault( e ) { 
    //阻止默认浏览器动作(W3C) 
    if ( e && e.preventDefault ) 
        e.preventDefault(); 
    //IE中阻止函数器默认动作的方式 
    else 
        window.event.returnValue = false; 
    return false; 
}
```

### 10. ES6新的特性有哪些？

1. 新增了块级作用域(let,const)
2. 提供了定义类的语法糖(class)
3. 新增了一种基本数据类型(Symbol)
4. 新增了变量的解构赋值
5. 函数参数允许设置默认值，引入了rest参数，新增了箭头函数
6. 数组新增了一些API，如 isArray / from / of 方法;数组实例新增了 entries()，keys() 和 values() 等方法
7. 对象和数组新增了扩展运算符
8. ES6 新增了模块化(import/export)
9. ES6 新增了 Set 和 Map 数据结构
10. ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例
11. ES6 新增了生成器(Generator)和遍历器(Iterator)