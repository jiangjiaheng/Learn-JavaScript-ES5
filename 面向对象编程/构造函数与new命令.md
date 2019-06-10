# 构造函数与new命令

<!-- TOC -->

- [构造函数与new命令](#构造函数与new命令)
    - [构造函数](#构造函数)
    - [new命令](#new命令)
        - [1. 基本用法](#1-基本用法)
        - [2. new命令的原理](#2-new命令的原理)
        - [3. new.target](#3-newtarget)

<!-- /TOC -->

## 构造函数

> JavaScript 语言使用构造函数（constructor）作为对象的模板。所谓”构造函数”，就是专门用来生成实例对象的函数。它就是对象的模板，描述实例对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有相同的结构。

构造函数就是一个普通的函数，但是有自己的特征和用法。

```obj
var Vehicle = function () {
  this.price = 1000;
};
```

上面代码中，Vehicle就是构造函数。为了与普通函数区别，构造函数名字的第一个字母通常大写。

构造函数的特点有两个。

- 函数体内部使用了this关键字，代表了所要生成的对象实例。
- 生成对象的时候，必须使用new命令。

## new命令

### 1. 基本用法

new命令的作用，就是执行构造函数，返回一个实例对象。

```obj
var Vehicle = function () {
  this.price = 1000;
};

var v = new Vehicle();
v.price // 1000
```

使用new命令时，根据需要，构造函数也可以接受参数。

```obj
var Vehicle = function (p) {
  this.price = p;
};

var v = new Vehicle(500);
```

如果忘了使用new命令，直接调用构造函数，构造函数就变成了普通函数，并不会生成实例对象。this这时代表全局对象，将造成一些意想不到的结果。

```obj
var Vehicle = function (){
  this.price = 1000;
};

var v = Vehicle();
v // undefined
price // 1000
```

### 2. new命令的原理

使用new命令时，它后面的函数依次执行下面的步骤。

1. 创建一个空对象，作为将要返回的对象实例。
2. 将这个空对象的原型，指向构造函数的prototype属性。
3. 将这个空对象赋值给函数内部的this关键字。
4. 开始执行构造函数内部的代码。

如果构造函数内部有return语句，而且return后面跟着一个对象，new命令会返回return语句指定的对象；否则，就会不管return语句，返回this对象。

```obj
var Vehicle = function () {
  this.price = 1000;
  return 1000;
};

(new Vehicle()) === 1000
// false
```

如果return语句返回的是一个跟this无关的新对象，new命令会返回这个新对象，而不是this对象。这一点需要特别引起注意。

```obj
var Vehicle = function (){
  this.price = 1000;
  return { price: 2000 };
};

(new Vehicle()).price
// 2000
```

另一方面，如果对普通函数（内部没有this关键字的函数）使用new命令，则会返回一个空对象。

```
function getMessage() {
  return 'this is a message';
}

var msg = new getMessage();

msg // {}
typeof msg // "object"
```

### 3. new.target

函数内部可以使用new.target属性。如果当前函数是new命令调用，new.target指向当前函数，否则为undefined。

```obj
function f() {
  console.log(new.target === f);
}

f() // false
new f() // true
```

使用这个属性，可以判断函数调用的时候，是否使用new命令。

```obj
function f() {
  if (!new.target) {
    throw new Error('请使用 new 命令调用！');
  }
  // ...
}

f() // Uncaught Error: 请使用 new 命令调用！
```