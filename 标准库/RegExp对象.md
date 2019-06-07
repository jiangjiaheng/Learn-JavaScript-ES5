# RegExp对象

<!-- TOC -->

- [RegExp对象](#regexp对象)
    - [概述](#概述)
    - [实例属性](#实例属性)
        - [1. 修饰符相关](#1-修饰符相关)
        - [2. 修饰符无关](#2-修饰符无关)
    - [实例方法](#实例方法)
        - [1. test](#1-test)
        - [2. exec](#2-exec)
    - [字符串相关](#字符串相关)
        - [1. match](#1-match)
        - [2. search](#2-search)
        - [3. replace](#3-replace)
        - [4. split](#4-split)

<!-- /TOC -->

## 概述

新建正则表达式有两种方法。一种是使用字面量，以斜杠表示开始和结束。

```re
var regex = /xyz/;
```

另一种是使用RegExp构造函数。

```re
var regex = new RegExp('xyz');
```

RegExp构造函数还可以接受第二个参数，表示修饰。

```re
var regex = new RegExp('xyz', 'i');
// 等价于
var regex = /xyz/i;
```

## 实例属性

### 1. 修饰符相关

- RegExp.prototype.ignoreCase：返回一个布尔值，表示是否设置了i修饰符。
- RegExp.prototype.global：返回一个布尔值，表示是否设置了g修饰符。
- RegExp.prototype.multiline：返回一个布尔值，表示是否设置了m修饰符。
- RegExp.prototype.flags：返回一个字符串，包含了已经设置的所有修饰符，按字母排序。

上面四个属性都是只读的。

```re
var r = /abc/igm;

r.ignoreCase // true
r.global // true
r.multiline // true
r.flags // 'gim'
```

### 2. 修饰符无关

- RegExp.prototype.lastIndex：返回一个整数，表示下一次开始搜索的位置。该属性可读写，但是只在进行连续搜索时有意义，详细介绍请看后文。
- RegExp.prototype.source：返回正则表达式的字符串形式（不包括反斜杠），该属性只读。

```re
var r = /abc/igm;

r.lastIndex // 0
r.source // "abc"
```

## 实例方法

### 1. test

正则实例对象的test方法返回一个布尔值，表示当前模式是否能匹配参数字符串。

```re
/cat/.test('cats and dogs') // true
```

上面代码验证参数字符串之中是否包含cat，结果返回true。

如果正则表达式带有g修饰符，则每一次test方法都从上一次结束的位置开始向后匹配。

```re
var r = /x/g;
var s = '_x_x';

r.lastIndex // 0
r.test(s) // true

r.lastIndex // 2
r.test(s) // true

r.lastIndex // 4
r.test(s) // false
```

### 2. exec

正则实例对象的exec方法，用来返回匹配结果。如果发现匹配，就返回一个数组，成员是匹配成功的子字符串，否则返回null。

```re
var s = '_x_x';
var r1 = /x/;
var r2 = /y/;

r1.exec(s) // ["x"]
r2.exec(s) // null
```

exec方法的返回数组还包含以下两个属性：

- input：整个原字符串。
- index：整个模式匹配成功的开始位置（从0开始计数）。

```re
var r = /a(b+)a/;
var arr = r.exec('_abbba_aba_');

arr // ["abbba", "bbb"]

arr.index // 1
arr.input // "_abbba_aba_"
```

如果正则表达式加上g修饰符，则可以使用多次exec方法，下一次搜索的位置从上一次匹配成功结束的位置开始。

```re
var reg = /a/g;
var str = 'abc_abc_abc'

var r1 = reg.exec(str);
r1 // ["a"]
r1.index // 0
reg.lastIndex // 1

var r2 = reg.exec(str);
r2 // ["a"]
r2.index // 4
reg.lastIndex // 5

var r3 = reg.exec(str);
r3 // ["a"]
r3.index // 8
reg.lastIndex // 9

var r4 = reg.exec(str);
r4 // null
reg.lastIndex // 0
```

## 字符串相关

### 1. match

字符串实例对象的match方法对字符串进行正则匹配，返回匹配结果。

```re
var s = '_x_x';
var r1 = /x/;
var r2 = /y/;

s.match(r1) // ["x"]
s.match(r2) // null
```

### 2. search

字符串对象的search方法，返回第一个满足条件的匹配结果在整个字符串中的位置。如果没有任何匹配，则返回-1。

```re
'_x_x'.search(/x/)
// 1
```

### 3. replace

字符串对象的replace方法可以替换匹配的值。它接受两个参数，第一个是正则表达式，表示搜索模式，第二个是替换的内容。

```re
str.replace(search, replacement)
```

正则表达式如果不加g修饰符，就替换第一个匹配成功的值，否则替换所有匹配成功的值。

```re
'aaa'.replace('a', 'b') // "baa"
'aaa'.replace(/a/, 'b') // "baa"
'aaa'.replace(/a/g, 'b') // "bbb"
```

replace方法的一个应用，就是消除字符串首尾两端的空格。

```re
var str = '  #id div.class  ';

str.replace(/^\s+|\s+$/g, '')
// "#id div.class"
```

replace方法的第二个参数可以使用美元符号$，用来指代所替换的内容。

- $&：匹配的子字符串。
- $`：匹配结果前面的文本。
- $'：匹配结果后面的文本。
- $n：匹配成功的第n组内容，n是从1开始的自然数。
- $$：指代美元符号$。

```re
'hello world'.replace(/(\w+)\s(\w+)/, '$2 $1')
// "world hello"

'abc'.replace('b', '[$`-$&-$\']')
// "a[a-b-c]c"
```

replace方法的第二个参数还可以是一个函数，将每一个匹配内容替换为函数返回值。

```re
'3 and 5'.replace(/[0-9]+/g, function (match) {
  return 2 * match;
})
// "6 and 10"

var a = 'The quick brown fox jumped over the lazy dog.';
var pattern = /quick|brown|lazy/ig;

a.replace(pattern, function replacer(match) {
  return match.toUpperCase();
});
// The QUICK BROWN fox jumped over the LAZY dog.
```

### 4. split

字符串对象的split方法按照正则规则分割字符串，返回一个由分割后的各个部分组成的数组。

```re
str.split(separator, [limit])
```

该方法接受两个参数，第一个参数是正则表达式，表示分隔规则，第二个参数是返回数组的最大成员数。

```re
// 非正则分隔
'a,  b,c, d'.split(',')
// [ 'a', '  b', 'c', ' d' ]

// 正则分隔，去除多余的空格
'a,  b,c, d'.split(/, */)
// [ 'a', 'b', 'c', 'd' ]

// 指定返回数组的最大成员
'a,  b,c, d'.split(/, */, 2)
[ 'a', 'b' ]
```
