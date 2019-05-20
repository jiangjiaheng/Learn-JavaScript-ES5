# Object 对象

<!-- TOC -->

- [Object 对象](#object-对象)
    - [概述](#概述)
    - [Object()](#object)
    - [Object构造函数](#object构造函数)
    - [Object静态方法](#object静态方法)
        - [1. keys(), getOwnPropertyNames()](#1-keys-getownpropertynames)
        - [2. 其他方法](#2-其他方法)
    - [Object实例方法](#object实例方法)
        - [1. valueOf()](#1-valueof)
        - [2. toString()](#2-tostring)
        - [3. toString判断数据类型](#3-tostring判断数据类型)
        - [4. toLocaleString()](#4-tolocalestring)
        - [5. hasOwnProperty()](#5-hasownproperty)

<!-- /TOC -->

## 概述

- JavaScript 的所有其他对象都继承自Object对象，即那些对象都是Object的实例。

- Object对象的原生方法分成两类：Object本身的方法与Object的实例方法。

所谓“本身的方法”就是直接定义在Object对象的方法。

```object
Object.print = function (o) { console.log(o) };
```

所谓实例方法就是定义在Object原型对象Object.prototype上的方法。它可以被Object实例直接使用。

```object
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // Object
```

## Object()

Object本身是一个函数，可以当作工具方法使用，将任意值转为对象。

如果参数为空（或者为undefined和null），Object()返回一个空对象。

```object
var obj = Object();
// 等同于
var obj = Object(undefined);
var obj = Object(null);

obj instanceof Object // true
```

如果参数是原始类型的值，Object方法将其转为对应的包装对象的实例。

```object
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true
```

如果Object方法的参数是一个对象，它总是返回该对象，即不用转换。

```object
var arr = [];
var obj = Object(arr); // 返回原数组
obj === arr // true

var value = {};
var obj = Object(value) // 返回原对象
obj === value // true

var fn = function () {};
var obj = Object(fn); // 返回原函数
obj === fn // true
```

利用这一点，可以写一个判断变量是否为对象的函数。

```object
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```

## Object构造函数

Object构造函数的首要用途，是直接通过它来生成新对象。

```object
var obj = new Object();
```

>注意，通过var obj = new Object()的写法生成新对象，与字面量的写法var obj = {}是等价的。或者说，后者只是前者的一种简便写法。

```object
var o1 = {a: 1};
var o2 = new Object(o1);
o1 === o2 // true

var obj = new Object(123);
obj instanceof Number // true
```

## Object静态方法

### 1. keys(), getOwnPropertyNames()

Object.keys方法的参数是一个对象，返回一个数组。

```object
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj) // ["p1", "p2"]
```

Object.getOwnPropertyNames方法与Object.keys类似。

```object
var obj = {
  p1: 123,
  p2: 456
};

Object.getOwnPropertyNames(obj) // ["p1", "p2"]
```

数组的length属性是不可枚举的属性，所以只出现在Object.getOwnPropertyNames方法的返回结果中。

```object
var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]
```

由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。

```object
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj).length // 2
Object.getOwnPropertyNames(obj).length // 2
```

### 2. 其他方法

（1）对象属性模型的相关方法

- Object.getOwnPropertyDescriptor()：获取某个属性的描述对象。

- Object.defineProperty()：通过描述对象，定义某个属性。

- Object.defineProperties()：通过描述对象，定义多个属性。

（2）控制对象状态的方法

- Object.preventExtensions()：防止对象扩展。

- Object.isExtensible()：判断对象是否可扩展。

- Object.seal()：禁止对象配置。

- Object.isSealed()：判断一个对象是否可配置。

- Object.freeze()：冻结一个对象。

- Object.isFrozen()：判断一个对象是否被冻结。

（3）原型链相关方法

- Object.create()：该方法可以指定原型对象和属性，返回一个新的对象。

- Object.getPrototypeOf()：获取对象的Prototype对象。

## Object实例方法

### 1. valueOf()

valueOf方法的作用是返回一个对象的“值”，默认情况下返回对象本身。

```object
var obj = new Object();
obj.valueOf() === obj // true
```

valueOf方法的主要用途是，JavaScript 自动类型转换时会默认调用这个方法。

```object
var obj = new Object();
1 + obj // "1[object Object]"
```

```object
var obj = new Object();
obj.valueOf = function () {
  return 2;
};

1 + obj // 3
```

### 2. toString()

toString方法的作用是返回一个对象的字符串形式，默认情况下返回类型字符串。

```object
var o1 = new Object();
o1.toString() // "[object Object]"

var o2 = {a:1};
o2.toString() // "[object Object]"
```

字符串[object Object]本身没有太大的用处，但是通过自定义toString方法，可以让对象在自动类型转换时，得到想要的字符串形式。

```object
var obj = new Object();

obj.toString = function () {
  return 'hello';
};

obj + ' ' + 'world' // "hello world"
```

### 3. toString判断数据类型

Object.prototype.toString方法返回对象的类型字符串，因此可以用来判断一个值的类型。

```object
var obj = {};
obj.toString() // "[object Object]"
```

>上面代码调用空对象的toString方法，结果返回一个字符串object Object，其中第二个Object表示该值的构造函数。这是一个十分有用的判断数据类型的方法。

这就是说，Object.prototype.toString可以看出一个值到底是什么类型。

```object
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(Math) // "[object Math]"
Object.prototype.toString.call({}) // "[object Object]"
Object.prototype.toString.call([]) // "[object Array]"
```

### 4. toLocaleString()

Object.prototype.toLocaleString方法与toString的返回结果相同，也是返回一个值的字符串形式。

```object
var obj = {};
obj.toString(obj) // "[object Object]"
obj.toLocaleString(obj) // "[object Object]"
```

这个方法的主要作用是留出一个接口，让各种不同的对象实现自己版本的toLocaleString，用来返回针对某些地域的特定的值。

```object
var date = new Date();
date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM"
```

### 5. hasOwnProperty()

Object.prototype.hasOwnProperty方法接受一个字符串作为参数，返回一个布尔值，表示该实例对象自身是否具有该属性。

```object
var obj = {
  p: 123
};

obj.hasOwnProperty('p') // true
obj.hasOwnProperty('toString') // false
```