# 关键字 this

<!-- TOC -->

- [关键字 this](#关键字-this)
    - [涵义](#涵义)
        - [1. 定义](#1-定义)
        - [2. this的可变性](#2-this的可变性)
        - [3. 网页编程的应用](#3-网页编程的应用)
    - [实质](#实质)
        - [1. 对象的内存地址](#1-对象的内存地址)
        - [2. 函数的内存地址](#2-函数的内存地址)
        - [3. 函数的运行环境](#3-函数的运行环境)
        - [4. this的出现](#4-this的出现)
    - [使用场合](#使用场合)
        - [1. 全局环境](#1-全局环境)
        - [2. 构造函数](#2-构造函数)
        - [3. 对象的方法](#3-对象的方法)
    - [使用注意点](#使用注意点)
        - [1. 避免多层this](#1-避免多层this)
        - [2. 避免数组处理方法中的 this](#2-避免数组处理方法中的-this)
        - [3. 避免回调函数中的 this](#3-避免回调函数中的-this)
    - [绑定 this 的方法](#绑定-this-的方法)
        - [1. call](#1-call)
        - [2. apply](#2-apply)
        - [3. bind](#3-bind)

<!-- /TOC -->

## 涵义

### 1. 定义

this就是属性或方法“当前”所在的对象。

```this
this.property
```

上面代码中，this就代表property属性当前所在的对象。

下面是一个实际的例子。

```this
var person = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

person.describe()
// "姓名：张三"
```

### 2. this的可变性

由于对象的属性可以赋给另一个对象，所以属性所在的当前对象是可变的，即this的指向是可变的。

```this
var A = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

var B = {
  name: '李四'
};

B.describe = A.describe;
B.describe()
// "姓名：李四"
```

稍稍重构这个例子，this的动态指向就能看得更清楚。

```this
function f() {
  return '姓名：'+ this.name;
}

var name='王五';

var A = {
  name: '张三',
  describe: f
};

var B = {
  name: '李四',
  describe: f
};

A.describe() // "姓名：张三"
B.describe() // "姓名：李四"
f() // "姓名：王五"
```

### 3. 网页编程的应用

再看一个网页编程的例子。

```this
<input type="text" name="age" size=3 onChange="validate(this, 18, 99);">

<script>
function validate(obj, lowval, hival){
  if ((obj.value < lowval) || (obj.value > hival))
    console.log('Invalid Value!');
}
</script>
```

> 总结一下，JavaScript 语言之中，一切皆对象，运行环境也是对象，所以函数都是在某个对象之中运行，this就是函数运行时所在的对象（环境）。

## 实质

### 1. 对象的内存地址

JavaScript 语言之所以有 this 的设计，跟内存里面的数据结构有关系。

```this
var obj = { foo:  5 };
```

上面的代码将一个对象赋值给变量obj。JavaScript 引擎会先在内存里面，生成一个对象{ foo: 5 }，然后把这个对象的内存地址赋值给变量obj。也就是说，变量obj是一个地址（reference）。后面如果要读取obj.foo，引擎先从obj拿到内存地址，然后再从该地址读出原始的对象，返回它的foo属性。

原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的foo属性，实际上是以下面的形式保存的。

```this
{
  foo: {
    [[value]]: 5
    [[writable]]: true
    [[enumerable]]: true
    [[configurable]]: true
  }
}
```

注意，foo属性的值保存在属性描述对象的value属性里面。

### 2. 函数的内存地址

这样的结构是很清晰的，问题在于属性的值可能是一个函数。

```this
var obj = { foo: function () {} };
```

这时，引擎会将函数单独保存在内存中，然后再将函数的地址赋值给foo属性的value属性。

```this
{
  foo: {
    [[value]]: 函数的地址
    ...
  }
}
```

### 3. 函数的运行环境

由于函数是一个单独的值，所以它可以在不同的环境（上下文）执行。

```this
var f = function () {};
var obj = { f: f };

// 单独执行
f()

// obj 环境执行
obj.f()
```

JavaScript 允许在函数体内部，引用当前环境的其他变量。

```this
var f = function () {
  console.log(x);
};
```

上面代码中，函数体里面使用了变量x。该变量由运行环境提供。

### 4. this的出现

现在问题就来了，由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（context）。所以，this就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。

```this
var f = function () {
  console.log(this.x);
}
```

上面代码中，函数体里面的this.x就是指当前运行环境的x。

```this
var f = function () {
  console.log(this.x);
}

var x = 1;
var obj = {
  f: f,
  x: 2,
};

// 单独执行
f() // 1

// obj 环境执行
obj.f() // 2
```

## 使用场合

### 1. 全局环境

全局环境使用this，它指的就是顶层对象window。

```this
this === window // true

function f() {
  console.log(this === window);
}
f() // true
```

上面代码说明，不管是不是在函数内部，只要是在全局环境下运行，this就是指顶层对象window。

### 2. 构造函数

构造函数中的this，指的是实例对象。

```this
var Obj = function (p) {
  this.p = p;
};
```

上面代码定义了一个构造函数Obj。由于this指向实例对象，所以在构造函数内部定义this.p，就相当于定义实例对象有一个p属性。

```this
var o = new Obj('Hello World!');
o.p // "Hello World!"
```

### 3. 对象的方法

如果对象的方法里面包含this，this的指向就是方法运行时所在的对象。该方法赋值给另一个对象，就会改变this的指向。

但是，这条规则很不容易把握。请看下面的代码。

```this
var obj ={
  foo: function () {
    console.log(this);
  }
};

obj.foo() // obj
```

上面代码中，obj.foo方法执行时，它内部的this指向obj。

但是，下面这几种用法，都会改变this的指向。

```this
// 情况一
(obj.foo = obj.foo)() // window
// 情况二
(false || obj.foo)() // window
// 情况三
(1, obj.foo)() // window
```

上面三种情况等同于下面的代码。

```this
// 情况一
(obj.foo = function () {
  console.log(this);
})()
// 等同于
(function () {
  console.log(this);
})()

// 情况二
(false || function () {
  console.log(this);
})()

// 情况三
(1, function () {
  console.log(this);
})()
```

如果this所在的方法不在对象的第一层，这时this只是指向当前一层的对象，而不会继承更上面的层。

```this
var a = {
  p: 'Hello',
  b: {
    m: function() {
      console.log(this.p);
    }
  }
};

a.b.m() // undefined
```

上面代码中，a.b.m方法在a对象的第二层，该方法内部的this不是指向a，而是指向a.b，因为实际执行的是下面的代码。

```this
var b = {
  m: function() {
   console.log(this.p);
  }
};

var a = {
  p: 'Hello',
  b: b
};

(a.b).m() // 等同于 b.m()
```

如果要达到预期效果，只有写成下面这样。

```this
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};
```

如果这时将嵌套对象内部的方法赋值给一个变量，this依然会指向全局对象。

```this
var a = {
  b: {
    m: function() {
      console.log(this.p);
    },
    p: 'Hello'
  }
};

var hello = a.b.m;
hello() // undefined
```

上面代码中，m是多层对象内部的一个方法。为求简便，将其赋值给hello变量，结果调用时，this指向了顶层对象。为了避免这个问题，可以只将m所在的对象赋值给hello，这样调用时，this的指向就不会变。

```this
var hello = a.b;
hello.m() // Hello
```

## 使用注意点

### 1. 避免多层this

由于this的指向是不确定的，所以切勿在函数中包含多层的this。

```this
var o = {
  f1: function () {
    console.log(this);
    var f2 = function () {
      console.log(this);
    }();
  }
}

o.f1()
// Object
// Window
```

上面代码包含两层this，结果运行后，第一层指向对象o，第二层指向全局对象，因为实际执行的是下面的代码。

```this
var temp = function () {
  console.log(this);
};

var o = {
  f1: function () {
    console.log(this);
    var f2 = temp();
  }
}
```

一个解决方法是在第二层改用一个指向外层this的变量。

```this
var o = {
  f1: function() {
    console.log(this);
    var that = this;
    var f2 = function() {
      console.log(that);
    }();
  }
}

o.f1()
// Object
// Object
```

### 2. 避免数组处理方法中的 this

数组的map和foreach方法，允许提供一个函数作为参数。这个函数内部不应该使用this。

```this
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    this.p.forEach(function (item) {
      console.log(this.v + ' ' + item);
    });
  }
}

o.f()
// undefined a1
// undefined a2
```

上面代码中，foreach方法的回调函数中的this，其实是指向window对象，因此取不到o.v的值。原因跟上一段的多层this是一样的，就是内层的this不指向外部，而指向顶层对象。

解决这个问题的一种方法，就是前面提到的，使用中间变量固定this。

```this
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    var that = this;
    this.p.forEach(function (item) {
      console.log(that.v+' '+item);
    });
  }
}

o.f()
// hello a1
// hello a2
```

另一种方法是将this当作foreach方法的第二个参数，固定它的运行环境。

```this
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    this.p.forEach(function (item) {
      console.log(this.v + ' ' + item);
    }, this);
  }
}

o.f()
// hello a1
// hello a2
```

### 3. 避免回调函数中的 this

回调函数中的this往往会改变指向，最好避免使用。

```this
var o = new Object();
o.f = function () {
  console.log(this === o);
}

// jQuery 的写法
$('#button').on('click', o.f);
```

## 绑定 this 的方法

### 1. call

函数实例的call方法，可以指定函数内部this的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数。

```this
var obj = {};

var f = function () {
  return this;
};

f() === window // true
f.call(obj) === obj // true
```

call方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象。

```this
var n = 123;
var obj = { n: 456 };

function a() {
  console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(obj) // 456
```

如果call方法的参数是一个原始值，那么这个原始值会自动转成对应的包装对象，然后传入call方法。

```this
var f = function () {
  return this;
};

f.call(5)
// Number {[[PrimitiveValue]]: 5}
```

call方法还可以接受多个参数。

```this
func.call(thisValue, arg1, arg2, ...)
```

call的第一个参数就是this所要指向的那个对象，后面的参数则是函数调用时所需的参数。

```this
function add(a, b) {
  return a + b;
}

add.call(this, 1, 2) // 3
```

call方法的一个应用是调用对象的原生方法。

```this
var obj = {};
obj.hasOwnProperty('toString') // false

// 覆盖掉继承的 hasOwnProperty 方法
obj.hasOwnProperty = function () {
  return true;
};
obj.hasOwnProperty('toString') // true

Object.prototype.hasOwnProperty.call(obj, 'toString') // false
```

### 2. apply

apply方法的作用与call方法类似，也是改变this指向，然后再调用该函数。唯一的区别就是，它接收一个数组作为函数执行时的参数，使用格式如下。

```this
func.apply(thisValue, [arg1, arg2, ...])
```

apply方法的第一个参数也是this所要指向的那个对象，如果设为null或undefined，则等同于指定全局对象。第二个参数则是一个数组，该数组的所有成员依次作为参数，传入原函数。原函数的参数，在call方法中必须一个个添加，但是在apply方法中，必须以数组形式添加。

```this
function f(x, y){
  console.log(x + y);
}

f.call(null, 1, 1) // 2
f.apply(null, [1, 1]) // 2
```

（1）找出数组最大元素

JavaScript 不提供找出数组最大元素的函数。结合使用apply方法和Math.max方法，就可以返回数组的最大元素。

```this
var a = [10, 2, 4, 15, 9];
Math.max.apply(null, a) // 15
```

（2）将数组的空元素变为undefined

通过apply方法，利用Array构造函数将数组的空元素变成undefined。

```this
Array.apply(null, ['a', ,'b'])
// [ 'a', undefined, 'b' ]
```

（3）转换类似数组的对象

另外，利用数组对象的slice方法，可以将一个类似数组的对象（比如arguments对象）转为真正的数组。

```this
Array.prototype.slice.apply({0: 1, length: 1}) // [1]
Array.prototype.slice.apply({0: 1}) // []
Array.prototype.slice.apply({0: 1, length: 2}) // [1, undefined]
Array.prototype.slice.apply({length: 1}) // [undefined]
```

（4）绑定回调函数的对象

前面的按钮点击事件的例子，可以改写如下。

```this
var o = new Object();

o.f = function () {
  console.log(this === o);
}

var f = function (){
  o.f.apply(o);
  // 或者 o.f.call(o);
};

// jQuery 的写法
$('#button').on('click', f);
```

### 3. bind

bind方法用于将函数体内的this绑定到某个对象，然后返回一个新函数。

```this
var d = new Date();
d.getTime() // 1481869925657

var print = d.getTime;
print() // Uncaught TypeError: this is not a Date object.
```

bind方法可以解决这个问题。

```this
var print = d.getTime.bind(d);
print() // 1481869925657
```

bind方法的参数就是所要绑定this的对象，下面是一个更清晰的例子。

```this
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};

var func = counter.inc.bind(counter);
func();
counter.count // 1
```

this绑定到其他对象也是可以的。

```this
var counter = {
  count: 0,
  inc: function () {
    this.count++;
  }
};

var obj = {
  count: 100
};
var func = counter.inc.bind(obj);
func();
obj.count // 101
```

bind还可以接受更多的参数，将这些参数绑定原函数的参数。

```this
var add = function (x, y) {
  return x * this.m + y * this.n;
}

var obj = {
  m: 2,
  n: 2
};

var newAdd = add.bind(obj, 5);
newAdd(5) // 20
```

如果bind方法的第一个参数是null或undefined，等于将this绑定到全局对象，函数运行时this指向顶层对象（浏览器为window）。

```this
function add(x, y) {
  return x + y;
}

var plus5 = add.bind(null, 5);
plus5(10) // 15
```

bind方法有一些使用注意点。

（1）每一次返回一个新函数

bind方法每运行一次，就返回一个新函数，这会产生一些问题。比如，监听事件的时候，不能写成下面这样。

```this
element.addEventListener('click', o.m.bind(o));
```

上面代码中，click事件绑定bind方法生成的一个匿名函数。这样会导致无法取消绑定，所以，下面的代码是无效的。

```this
element.removeEventListener('click', o.m.bind(o));
```

正确的方法是写成下面这样：

```this
var listener = o.m.bind(o);
element.addEventListener('click', listener);
//  ...
element.removeEventListener('click', listener);
```

（2）结合回调函数使用

回调函数是 JavaScript 最常用的模式之一，但是一个常见的错误是，将包含this的方法直接当作回调函数。解决方法就是使用bind方法，将counter.inc绑定counter。

```this
var counter = {
  count: 0,
  inc: function () {
    'use strict';
    this.count++;
  }
};

function callIt(callback) {
  callback();
}

callIt(counter.inc.bind(counter));
counter.count // 1
```

还有一种情况比较隐蔽，就是某些数组方法可以接受一个函数当作参数。这些函数内部的this指向，很可能也会出错。

```this
var obj = {
  name: '张三',
  times: [1, 2, 3],
  print: function () {
    this.times.forEach(function (n) {
      console.log(this.name);
    });
  }
};

obj.print()
// 没有任何输出
```

稍微改动一下，就可以看得更清楚。

```this
obj.print = function () {
  this.times.forEach(function (n) {
    console.log(this === window);
  });
};

obj.print()
// true
// true
// true
```

解决这个问题，也是通过bind方法绑定this。

```this
obj.print = function () {
  this.times.forEach(function (n) {
    console.log(this.name);
  }.bind(this));
};

obj.print()
// 张三
// 张三
// 张三
```

（3）结合call方法使用

利用bind方法，可以改写一些 JavaScript 原生方法的使用形式，以数组的slice方法为例。

```this
[1, 2, 3].slice(0, 1) // [1]
// 等同于
Array.prototype.slice.call([1, 2, 3], 0, 1) // [1]
```

call方法实质上是调用Function.prototype.call方法，因此上面的表达式可以用bind方法改写。

```this
var slice = Function.prototype.call.bind(Array.prototype.slice);
slice([1, 2, 3], 0, 1) // [1]
```

类似的写法还可以用于其他数组方法。

```this
var push = Function.prototype.call.bind(Array.prototype.push);
var pop = Function.prototype.call.bind(Array.prototype.pop);

var a = [1 ,2 ,3];
push(a, 4)
a // [1, 2, 3, 4]

pop(a)
a // [1, 2, 3]
```

如果再进一步，将Function.prototype.call方法绑定到Function.prototype.bind对象，就意味着bind的调用形式也可以被改写。

```this
function f() {
  console.log(this.v);
}

var o = { v: 123 };
var bind = Function.prototype.call.bind(Function.prototype.bind);
bind(f, o)() // 123
```
