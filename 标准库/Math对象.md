# Math对象

<!-- TOC -->

- [Math对象](#math对象)
    - [概述](#概述)
    - [静态属性](#静态属性)
    - [静态方法](#静态方法)
        - [1. abs](#1-abs)
        - [2. max, min](#2-max-min)
        - [3. floor, ceil](#3-floor-ceil)
        - [4. round](#4-round)
        - [5. pow](#5-pow)
        - [6. sqrt](#6-sqrt)
        - [7. log](#7-log)
        - [8. exp](#8-exp)
        - [9. random](#9-random)
        - [10. 三角函数方法](#10-三角函数方法)

<!-- /TOC -->

## 概述

Math是 JavaScript 的原生对象，提供各种数学功能。该对象不是构造函数，不能生成实例，所有的属性和方法都必须在Math对象上调用。

## 静态属性

Math对象的静态属性，提供以下一些数学常数。

- Math.E：常数e。
- Math.LN2：2 的自然对数。
- Math.LN10：10 的自然对数。
- Math.LOG2E：以 2 为底的e的对数。
- Math.LOG10E：以 10 为底的e的对数。
- Math.PI：常数π。
- Math.SQRT1_2：0.5 的平方根。
- Math.SQRT2：2 的平方根。

```ma
Math.E // 2.718281828459045
Math.LN2 // 0.6931471805599453
Math.LN10 // 2.302585092994046
Math.LOG2E // 1.4426950408889634
Math.LOG10E // 0.4342944819032518
Math.PI // 3.141592653589793
Math.SQRT1_2 // 0.7071067811865476
Math.SQRT2 // 1.4142135623730951
```

这些属性都是只读的，不能修改。

## 静态方法

### 1. abs

Math.abs方法返回参数值的绝对值。

```ma
Math.abs(1) // 1
Math.abs(-1) // 1
```

### 2. max, min

Math.max方法返回参数之中最大的那个值，Math.min返回最小的那个值。如果参数为空, Math.min返回Infinity, Math.max返回-Infinity。

```ma
Math.max(2, -1, 5) // 5
Math.min(2, -1, 5) // -1
Math.min() // Infinity
Math.max() // -Infinity
```

### 3. floor, ceil

Math.floor方法返回小于参数值的最大整数（地板值）。

```ma
Math.floor(3.2) // 3
Math.floor(-3.2) // -4
```

Math.ceil方法返回大于参数值的最小整数（天花板值）。

```ma
Math.ceil(3.2) // 4
Math.ceil(-3.2) // -3
```

这两个方法可以结合起来，实现一个总是返回数值的整数部分的函数。

```ma
function ToInteger(x) {
  x = Number(x);
  return x < 0 ? Math.ceil(x) : Math.floor(x);
}

ToInteger(3.2) // 3
ToInteger(3.5) // 3
ToInteger(3.8) // 3
ToInteger(-3.2) // -3
ToInteger(-3.5) // -3
ToInteger(-3.8) // -3
```

### 4. round

Math.round方法用于四舍五入。

```ma
Math.round(0.1) // 0
Math.round(0.5) // 1
Math.round(0.6) // 1

// 等同于
Math.floor(x + 0.5)
```

注意，它对负数的处理（主要是对0.5的处理）。

```ma
Math.round(-1.1) // -1
Math.round(-1.5) // -1
Math.round(-1.6) // -2
```

### 5. pow

Math.pow方法返回以第一个参数为底数、第二个参数为幂的指数值。

```ma
// 等同于 2 ** 2
Math.pow(2, 2) // 4
// 等同于 2 ** 3
Math.pow(2, 3) // 8
```

下面是计算圆面积的方法。

```ma
var radius = 20;
var area = Math.PI * Math.pow(radius, 2);
```

### 6. sqrt

Math.sqrt方法返回参数值的平方根。如果参数是一个负值，则返回NaN。

```ma
Math.sqrt(4) // 2
Math.sqrt(-4) // NaN
```

### 7. log

Math.log方法返回以e为底的自然对数值。

```ma
Math.log(Math.E) // 1
Math.log(10) // 2.302585092994046
```

如果要计算以10为底的对数，可以先用Math.log求出自然对数，然后除以Math.LN10；求以2为底的对数，可以除以Math.LN2。

```ma
Math.log(100)/Math.LN10 // 2
Math.log(8)/Math.LN2 // 3
```

### 8. exp

Math.exp方法返回常数e的参数次方。

```ma
Math.exp(1) // 2.718281828459045
Math.exp(3) // 20.085536923187668
```

### 9. random

Math.random()返回0到1之间的一个伪随机数，可能等于0，但是一定小于1。

```ma
Math.random() // 0.7151307314634323
```

任意范围的随机数生成函数如下。

```ma
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}

getRandomArbitrary(1.5, 6.5)
// 2.4942810038223864
```

任意范围的随机整数生成函数如下。

```ma
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

getRandomInt(1, 6) // 5
```

返回随机字符的例子如下。

```ma
function random_str(length) {
  var ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  ALPHABET += 'abcdefghijklmnopqrstuvwxyz';
  ALPHABET += '0123456789-_';
  var str = '';
  for (var i = 0; i < length; ++i) {
    var rand = Math.floor(Math.random() * ALPHABET.length);
    str += ALPHABET.substring(rand, rand + 1);
  }
  return str;
}

random_str(6) // "NdQKOr"
```

### 10. 三角函数方法

Math对象还提供一系列三角函数方法。

- Math.sin()：返回参数的正弦（参数为弧度值）
- Math.cos()：返回参数的余弦（参数为弧度值）
- Math.tan()：返回参数的正切（参数为弧度值）
- Math.asin()：返回参数的反正弦（返回值为弧度值）
- Math.acos()：返回参数的反余弦（返回值为弧度值）
- Math.atan()：返回参数的反正切（返回值为弧度值）

```ma
Math.sin(0) // 0
Math.cos(0) // 1
Math.tan(0) // 0

Math.sin(Math.PI / 2) // 1

Math.asin(1) // 1.5707963267948966
Math.acos(1) // 0
Math.atan(1) // 0.7853981633974483
```

