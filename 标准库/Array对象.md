# Array 对象

<!-- TOC -->

- [Array 对象](#array-对象)
    - [构造函数](#构造函数)
    - [静态方法](#静态方法)
    - [实例方法](#实例方法)
        - [1. valueOf, toString](#1-valueof-tostring)
        - [2. push, pop](#2-push-pop)
        - [3. shift, unshift](#3-shift-unshift)
        - [4. join](#4-join)
        - [5. concat](#5-concat)
        - [6. reverse](#6-reverse)
        - [7. slice](#7-slice)
        - [8. splice](#8-splice)
        - [9. sort](#9-sort)
        - [10. map](#10-map)
        - [11 forEach](#11-foreach)
        - [12. filter](#12-filter)
        - [13. some, every](#13-some-every)
        - [14. reduce, reduceRight](#14-reduce-reduceright)
        - [15. indexOf, lastIndexOf](#15-indexof-lastindexof)
        - [16. 链式使用](#16-链式使用)
    - [总结](#总结)
        - [1. 改变原数组的实例方法](#1-改变原数组的实例方法)
        - [2. 返回新数组的实例方法](#2-返回新数组的实例方法)
        - [3. 返回其他值的实例方法](#3-返回其他值的实例方法)
        - [4. 不返回值的实例方法](#4-不返回值的实例方法)

<!-- /TOC -->

## 构造函数

不建议使用Array生成新数组，直接使用数组字面量是更好的做法。

```ar
// bad
var arr = new Array(1, 2);

// good
var arr = [1, 2];
```

## 静态方法

Array.isArray方法返回一个布尔值，表示参数是否为数组。它可以弥补typeof运算符的不足。

```ar
var arr = [1, 2, 3];

typeof arr // "object"
Array.isArray(arr) // true
```

## 实例方法

### 1. valueOf, toString

- 数组的valueOf方法返回数组本身。

```ar
var arr = [1, 2, 3];
arr.valueOf() // [1, 2, 3]
```

- 数组的toString方法返回数组的字符串形式。

```ar
var arr = [1, 2, 3];
arr.toString() // "1,2,3"

var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"
```

### 2. push, pop

push方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```ar
var arr = [];

arr.push(1) // 1
arr.push('a') // 2
arr.push(true, {}) // 4
arr // [1, 'a', true, {}]
```

pop方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组。

```ar
var arr = ['a', 'b', 'c'];

arr.pop() // 'c'
arr // ['a', 'b']
```

### 3. shift, unshift

shift()方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。

```ar
var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
```

unshift()方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

```ar
var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']

var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```

### 4. join

join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。如果不提供参数，默认用逗号分隔。

```ar
var a = [1, 2, 3, 4];

a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

如果数组成员是undefined或null或空位，会被转成空字符串。

```ar
[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
```

通过call方法，这个方法也可以用于字符串或类似数组的对象。

```ar
Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"

var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```

### 5. concat

concat方法用于多个数组的合并。它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。

```ar
['hello'].concat(['world'])
// ["hello", "world"]

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]

[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[2].concat({a: 1})
// [2, {a: 1}]
```

除了数组作为参数，concat也接受其他类型的值作为参数，添加到目标数组尾部。

```ar
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]
```

### 6. reverse

reverse方法用于颠倒排列数组元素，返回改变后的数组。注意，该方法将改变原数组。

```ar
var a = ['a', 'b', 'c'];

a.reverse() // ["c", "b", "a"]
a // ["c", "b", "a"]
```

### 7. slice

slice方法用于提取目标数组的一部分，返回一个新数组，原数组不变。

```ar
arr.slice(start, end);
```

它的第一个参数为起始位置（从0开始），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。

```ar
var a = ['a', 'b', 'c'];

a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```

上面代码中，最后一个例子slice没有参数，实际上等于返回一个原数组的拷贝。

如果slice方法的参数是负数，则表示倒数计算的位置。

```ar
var a = ['a', 'b', 'c'];
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```

### 8. splice

splice方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。注意，该方法会改变原数组。

```ar
arr.splice(start, count, addElement1, addElement2, ...);
```

splice的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

```ar
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2) // ["e", "f"]
a // ["a", "b", "c", "d"]
```

上面代码从原数组4号位置，删除了两个数组成员。

```ar
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

起始位置如果是负数，就表示从倒数位置开始删除。

```ar
var a = ['a', 'b', 'c', 'd', 'e', 'f'];
a.splice(-4, 2) // ["c", "d"]
```

### 9. sort

sort方法对数组成员进行排序，默认是按照字典顺序排序。排序后，原数组将被改变。

```ar
['d', 'c', 'b', 'a'].sort()
// ['a', 'b', 'c', 'd']

[4, 3, 2, 1].sort()
// [1, 2, 3, 4]

[11, 101].sort()
// [101, 11]

[10111, 1101, 111].sort()
// [10111, 1101, 111]
```

如果想让sort方法按照自定义方式排序，可以传入一个函数作为参数。

```arr
[10111, 1101, 111].sort(function (a, b) {
  return a - b;
})
// [111, 1101, 10111]
```

### 10. map

map方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回。

```ar
var numbers = [1, 2, 3];

numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]

numbers
// [1, 2, 3]
```

### 11 forEach

forEach的用法与map方法一致，参数是一个函数，该函数同样接受三个参数：当前值、当前位置、整个数组。

```ar
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

forEach方法也会跳过数组的空位。

```ar
var log = function (n) {
  console.log(n + 1);
};

[1, undefined, 2].forEach(log)
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1, , 2].forEach(log)
// 2
// 3
```

上面代码中，forEach方法不会跳过undefined和null，但会跳过空位。

### 12. filter

filter方法用于过滤数组成员，满足条件的成员组成一个新数组返回。

它的参数是一个函数，所有数组成员依次执行该函数，返回结果为true的成员组成一个新数组返回。该方法不会改变原数组。

```ar
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]
```

filter方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组。

```ar
[1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```

### 13. some, every

some方法是只要一个成员的返回值是true，则整个some方法的返回值就是true，否则返回false。

```ar
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true
```

every方法是所有成员的返回值都是true，整个every方法才返回true，否则返回false。

```ar
var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
```

### 14. reduce, reduceRight

reduce方法和reduceRight方法依次处理数组的每个成员，最终累计为一个值。它们的差别是，reduce是从左到右处理（从第一个成员到最后一个成员），reduceRight则是从右到左（从最后一个成员到第一个成员），其他完全一样。

```ar
[1, 2, 3, 4, 5].reduce(function (a, b) {
  console.log(a, b);
  return a + b;
})
// 1 2
// 3 3
// 6 4
// 10 5
//最后结果：15
```

下面是一个reduceRight方法的例子。

```ar
function subtract(prev, cur) {
  return prev - cur;
}

[3, 2, 1].reduce(subtract) // 0
[3, 2, 1].reduceRight(subtract) // -4
```

### 15. indexOf, lastIndexOf

indexOf方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回-1。

```ar
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```

indexOf方法还可以接受第二个参数，表示搜索的开始位置。

```ar
['a', 'b', 'c'].indexOf('a', 1) // -1
```

lastIndexOf方法返回给定元素在数组中最后一次出现的位置，如果没有出现则返回-1。

```ar
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1
```

注意，这两个方法不能用来搜索NaN的位置，即它们无法确定数组成员是否包含NaN。

```ar
[NaN].indexOf(NaN) // -1
[NaN].lastIndexOf(NaN) // -1
```

### 16. 链式使用

上面这些数组方法之中，有不少返回的还是数组，所以可以链式使用。

```ar
var users = [
  {name: 'tom', email: 'tom@example.com'},
  {name: 'peter', email: 'peter@example.com'}
];

users
.map(function (user) {
  return user.email;
})
.filter(function (email) {
  return /^t/.test(email);
})
.forEach(function (email) {
  console.log(email);
});
// "tom@example.com"
```

## 总结

### 1. 改变原数组的实例方法

- push

- pop

- shift

- unshift

- reverse

- splice

- sort

### 2. 返回新数组的实例方法

- concat

- slice

- map

-filter

### 3. 返回其他值的实例方法

- valueOf

- toString

- join

- some

- every

- reduce

- reduceRight

- indexOf

- lastIndexOf

### 4. 不返回值的实例方法

- forEach