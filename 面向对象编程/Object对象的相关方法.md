# Object对象的相关方法

<!-- TOC -->

- [Object对象的相关方法](#object对象的相关方法)
    - [静态方法](#静态方法)
        - [1. getPrototypeOf](#1-getprototypeof)
        - [2. setPrototypeOf](#2-setprototypeof)
        - [3. create](#3-create)
        - [4. getOwnPropertyNames](#4-getownpropertynames)
    - [实例方法](#实例方法)
        - [1. isPrototypeOf](#1-isprototypeof)
        - [2. _proto_](#2-_proto_)
        - [3. hasOwnProperty](#3-hasownproperty)
    - [获取原型对象方法的比较](#获取原型对象方法的比较)
    - [in运算符和for……in循环](#in运算符和forin循环)
    - [对象的拷贝](#对象的拷贝)

<!-- /TOC -->

## 静态方法

### 1. getPrototypeOf

Object.getPrototypeOf方法返回参数对象的原型。这是获取原型对象的标准方法。

```obj
var F = function () {};
var f = new F();
Object.getPrototypeOf(f) === F.prototype // true
```

下面是几种特殊对象的原型。

```obj
// 空对象的原型是 Object.prototype
Object.getPrototypeOf({}) === Object.prototype // true

// Object.prototype 的原型是 null
Object.getPrototypeOf(Object.prototype) === null // true

// 函数的原型是 Function.prototype
function f() {}
Object.getPrototypeOf(f) === Function.prototype // true
```

### 2. setPrototypeOf

Object.setPrototypeOf方法为参数对象设置原型，返回该参数对象。它接受两个参数，第一个是现有对象，第二个是原型对象。

```obj
var a = {};
var b = {x: 1};
Object.setPrototypeOf(a, b);

Object.getPrototypeOf(a) === b // true
a.x // 1
```

new命令可以使用Object.setPrototypeOf方法模拟。

```obj
var F = function () {
  this.foo = 'bar';
};

var f = new F();
// 等同于
var f = Object.setPrototypeOf({}, F.prototype);
F.call(f);
```

### 3. create

JavaScript 提供了Object.create方法，用来满足这种需求。该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性。

```obj
// 原型对象
var A = {
  print: function () {
    console.log('hello');
  }
};

// 实例对象
var B = Object.create(A);

Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true
```

下面三种方式生成的新对象是等价的。

```obj
var obj1 = Object.create({});
var obj2 = Object.create(Object.prototype);
var obj3 = new Object();
```

如果想要生成一个不继承任何属性（比如没有toString和valueOf方法）的对象，可以将Object.create的参数设为null。

```obj
var obj = Object.create(null);

obj.valueOf()
// TypeError: Object [object Object] has no method 'valueOf'
```

使用Object.create方法的时候，必须提供对象原型，即参数不能为空，或者不是对象，否则会报错。

```obj
Object.create()
// TypeError: Object prototype may only be an Object or null
Object.create(123)
// TypeError: Object prototype may only be an Object or null
```

Object.create方法生成的新对象，动态继承了原型。在原型上添加或修改任何方法，会立刻反映在新对象之上。

```obj
var obj1 = { p: 1 };
var obj2 = Object.create(obj1);

obj1.p = 2;
obj2.p // 2
```

除了对象的原型，Object.create方法还可以接受第二个参数。该参数是一个属性描述对象，它所描述的对象属性，会添加到实例对象，作为该对象自身的属性。

```obj
var obj = Object.create({}, {
  p1: {
    value: 123,
    enumerable: true,
    configurable: true,
    writable: true,
  },
  p2: {
    value: 'abc',
    enumerable: true,
    configurable: true,
    writable: true,
  }
});

// 等同于
var obj = Object.create({});
obj.p1 = 123;
obj.p2 = 'abc';
```

Object.create方法生成的对象，继承了它的原型对象的构造函数。

```obj
function A() {}
var a = new A();
var b = Object.create(a);

b.constructor === A // true
b instanceof A // true
```

### 4. getOwnPropertyNames

Object.getOwnPropertyNames方法返回一个数组，成员是参数对象本身的所有属性的键名，不包含继承的属性键名。

```obj
Object.getOwnPropertyNames(Date)
// ["parse", "arguments", "UTC", "caller", "name", "prototype", "now", "length"]
```

上面代码中，Object.getOwnPropertyNames方法返回Date所有自身的属性名。

对象本身的属性之中，有的是可以遍历的（enumerable），有的是不可以遍历的。Object.getOwnPropertyNames方法返回所有键名，不管是否可以遍历。只获取那些可以遍历的属性，使用Object.keys方法。

```obj
Object.keys(Date) // []
```

上面代码表明，Date对象所有自身的属性，都是不可以遍历的。

## 实例方法

### 1. isPrototypeOf

实例对象的isPrototypeOf方法，用来判断该对象是否为参数对象的原型。

```obj
var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);

o2.isPrototypeOf(o3) // true
o1.isPrototypeOf(o3) // true
```

上面代码中，o1和o2都是o3的原型。这表明只要实例对象处在参数对象的原型链上，isPrototypeOf方法都返回true。

```obj
Object.prototype.isPrototypeOf({}) // true
Object.prototype.isPrototypeOf([]) // true
Object.prototype.isPrototypeOf(/xyz/) // true
Object.prototype.isPrototypeOf(Object.create(null)) // false
```

### 2. _proto_

实例对象的__proto__属性（前后各两个下划线），返回该对象的原型。该属性可读写。

```obj
var obj = {};
var p = {};

obj.__proto__ = p;
Object.getPrototypeOf(obj) === p // true
```

原型链可以用__proto__很直观地表示。

```obj
var A = {
  name: '张三'
};
var B = {
  name: '李四'
};

var proto = {
  print: function () {
    console.log(this.name);
  }
};

A.__proto__ = proto;
B.__proto__ = proto;

A.print() // 张三
B.print() // 李四

A.print === B.print // true
A.print === proto.print // true
B.print === proto.print // true
```

### 3. hasOwnProperty

对象实例的hasOwnProperty方法返回一个布尔值，用于判断某个属性定义在对象自身，还是定义在原型链上。

```obj
Date.hasOwnProperty('length') // true
Date.hasOwnProperty('toString') // false
```

上面代码表明，Date.length（构造函数Date可以接受多少个参数）是Date自身的属性，Date.toString是继承的属性。

另外，hasOwnProperty方法是 JavaScript 之中唯一一个处理对象属性时，不会遍历原型链的方法。

## 获取原型对象方法的比较

如前所述，__proto__属性指向当前对象的原型对象，即构造函数的prototype属性。

```obj
var obj = new Object();

obj.__proto__ === Object.prototype
// true
obj.__proto__ === obj.constructor.prototype
// true
```

因此，获取实例对象obj的原型对象，有三种方法。

- obj.__proto__
- obj.constructor.prototype
- Object.getPrototypeOf(obj)

上面三种方法之中，前两种都不是很可靠。__proto__属性只有浏览器才需要部署，其他环境可以不部署。而obj.constructor.prototype在手动改变原型对象时，可能会失效。

```obj
var P = function () {};
var p = new P();

var C = function () {};
C.prototype = p;
var c = new C();

c.constructor.prototype === p // false
```

上面代码中，构造函数C的原型对象被改成了p，但是实例对象的c.constructor.prototype却没有指向p。所以，在改变原型对象时，一般要同时设置constructor属性。

```obj
C.prototype = p;
C.prototype.constructor = C;

var c = new C();
c.constructor.prototype === p // true
```

因此，推荐使用第三种Object.getPrototypeOf方法，获取原型对象。

## in运算符和for……in循环

in运算符返回一个布尔值，表示一个对象是否具有某个属性。它不区分该属性是对象自身的属性，还是继承的属性。

```obj
'length' in Date // true
'toString' in Date // true
```

获得对象的所有可遍历属性（不管是自身的还是继承的），可以使用for...in循环。

```obj
var o1 = { p1: 123 };

var o2 = Object.create(o1, {
  p2: { value: "abc", enumerable: true }
});

for (p in o2) {
  console.info(p);
}
// p2
// p1
```

为了在for...in循环中获得对象自身的属性，可以采用hasOwnProperty方法判断一下。

```obj
for ( var name in object ) {
  if ( object.hasOwnProperty(name) ) {
    /* loop code */
  }
}
```

获得对象的所有属性（不管是自身的还是继承的，也不管是否可枚举），可以使用下面的函数。

```obj
function inheritedPropertyNames(obj) {
  var props = {};
  while(obj) {
    Object.getOwnPropertyNames(obj).forEach(function(p) {
      props[p] = true;
    });
    obj = Object.getPrototypeOf(obj);
  }
  return Object.getOwnPropertyNames(props);
}
```

下面是一个例子，列出Date对象的所有属性。

```obj
inheritedPropertyNames(Date)
// [
//  "caller",
//  "constructor",
//  "toString",
//  "UTC",
//  ...
// ]
```

## 对象的拷贝

如果要拷贝一个对象，需要做到下面两件事情。

- 确保拷贝后的对象，与原对象具有同样的原型。
- 确保拷贝后的对象，与原对象具有同样的实例属性。

下面就是根据上面两点，实现的对象拷贝函数。

```obj
function copyObject(orig) {
  return Object.create(
    Object.getPrototypeOf(orig),
    Object.getOwnPropertyDescriptors(orig)
  );
}
```
