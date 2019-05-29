# Number对象

<!-- TOC -->

- [Number对象](#number对象)
    - [概述](#概述)
    - [静态属性](#静态属性)
    - [实例方法](#实例方法)
        - [1. toString](#1-tostring)
        - [2. toFixed](#2-tofixed)
        - [3. toExponential](#3-toexponential)
        - [4. toPrecision](#4-toprecision)
    - [自定义方法](#自定义方法)

<!-- /TOC -->

## 概述

Number对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

作为构造函数时，它用于生成值为数值的对象。

```n
var n = new Number(1);
typeof n // "object"
```

作为工具函数时，它可以将任何类型的值转为数值。

```n
Number(true) // 1
```

## 静态属性

Number对象拥有以下一些静态属性。

- Number.POSITIVE_INFINITY：正的无限，指向Infinity。

- Number.NEGATIVE_INFINITY：负的无限，指向-Infinity。

- Number.NaN：表示非数值，指向NaN。

- Number.MIN_VALUE：表示最小的正数（即最接近0的正数，在64位浮点数体系中为5e-324），相应的，最接近0的负数为-Number.MIN_VALUE。

- Number.MAX_SAFE_INTEGER：表示能够精确表示的最大整数，即9007199254740991。

- Number.MIN_SAFE_INTEGER：表示能够精确表示的最小整数，即-9007199254740991。

```n
Number.POSITIVE_INFINITY // Infinity
Number.NEGATIVE_INFINITY // -Infinity
Number.NaN // NaN

Number.MAX_VALUE
// 1.7976931348623157e+308
Number.MAX_VALUE < Infinity
// true

Number.MIN_VALUE
// 5e-324
Number.MIN_VALUE > 0
// true

Number.MAX_SAFE_INTEGER // 9007199254740991
Number.MIN_SAFE_INTEGER // -9007199254740991
```

## 实例方法

### 1. toString

Number对象部署了自己的toString方法，用来将一个数值转为字符串形式。

```n
(10).toString() // "10"
```

toString方法可以接受一个参数，表示输出的进制。如果省略这个参数，默认将数值先转为十进制，再输出字符串；否则，就根据参数指定的进制，将一个数字转化成某个进制的字符串。

```n
(10).toString(2) // "1010"
(10).toString(8) // "12"
(10).toString(16) // "a"
```

除了为10加上括号，还可以在10后面加两个点，JavaScript 会把第一个点理解成小数点（即10.0），把第二个点理解成调用对象属性，从而得到正确结果。

```n
10..toString(2)
// "1010"

// 其他方法还包括
10 .toString(2) // "1010"
10.0.toString(2) // "1010"
```

这实际上意味着，可以直接对一个小数使用toString方法。

```n
10.5.toString() // "10.5"
10.5.toString(2) // "1010.1"
10.5.toString(8) // "12.4"
10.5.toString(16) // "a.8"
```

通过方括号运算符也可以调用toString方法。

```n
10['toString'](2) // "1010"
```

### 2. toFixed

toFixed()方法先将一个数转为指定位数的小数，然后返回这个小数对应的字符串。

```n
(10).toFixed(2) // "10.00"
10.005.toFixed(2) // "10.01"
```

由于浮点数的原因，小数5的四舍五入是不确定的，使用的时候必须小心。

```n
(10.055).toFixed(2) // 10.05
(10.005).toFixed(2) // 10.01
```

### 3. toExponential

toExponential方法用于将一个数转为科学计数法形式。

```n
(10).toExponential()  // "1e+1"
(10).toExponential(1) // "1.0e+1"
(10).toExponential(2) // "1.00e+1"

(1234).toExponential()  // "1.234e+3"
(1234).toExponential(1) // "1.2e+3"
(1234).toExponential(2) // "1.23e+3"
```

toExponential方法的参数是小数点后有效数字的位数，范围为0到20，超出这个范围，会抛出一个 RangeError 错误。

### 4. toPrecision

toPrecision方法用于将一个数转为指定位数的有效数字。

```n
(12.34).toPrecision(1) // "1e+1"
(12.34).toPrecision(2) // "12"
(12.34).toPrecision(3) // "12.3"
(12.34).toPrecision(4) // "12.34"
(12.34).toPrecision(5) // "12.340"
```

toPrecision方法的参数为有效数字的位数，范围是1到21，超出这个范围会抛出 RangeError 错误。

toPrecision方法用于四舍五入时不太可靠，跟浮点数不是精确储存有关。

```n
(12.35).toPrecision(3) // "12.3"
(12.25).toPrecision(3) // "12.3"
(12.15).toPrecision(3) // "12.2"
(12.45).toPrecision(3) // "12.4"
```

## 自定义方法

与其他对象一样，Number.prototype对象上面可以自定义方法，被Number的实例继承。

```n
Number.prototype.add = function (x) {
  return this + x;
};

8['add'](2) // 10
```

由于add方法返回的还是数值，所以可以链式运算。

```n
Number.prototype.subtract = function (x) {
  return this - x;
};

(8).add(2).subtract(4)
// 6
```

我们还可以部署更复杂的方法。

```n
Number.prototype.iterate = function () {
  var result = [];
  for (var i = 0; i <= this; i++) {
    result.push(i);
  }
  return result;
};

(8).iterate()
// [0, 1, 2, 3, 4, 5, 6, 7, 8]
```

注意，数值的自定义方法，只能定义在它的原型对象Number.prototype上面，数值本身是无法自定义属性的。

```n
var n = 1;
n.x = 1;
n.x // undefined
```