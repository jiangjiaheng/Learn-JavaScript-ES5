# String 对象

<!-- TOC -->

- [String 对象](#string-对象)
    - [概述](#概述)
    - [静态方法](#静态方法)
    - [实例属性](#实例属性)
    - [实例方法](#实例方法)
        - [1. charAt](#1-charat)
        - [2. charCodeAt](#2-charcodeat)
        - [3. concat](#3-concat)
        - [4. slice](#4-slice)
        - [5. substring](#5-substring)
        - [6. substr](#6-substr)
        - [7. indexOf lastIndexOf](#7-indexof-lastindexof)
        - [8. trim](#8-trim)
        - [9. toLowerCase toUpperCase](#9-tolowercase-touppercase)
        - [10. match](#10-match)
        - [11. search replace](#11-search-replace)
        - [12. split](#12-split)
        - [13. localeCompare](#13-localecompare)
    - [总结](#总结)
        - [1. 与数组类似的实例方法](#1-与数组类似的实例方法)
        - [2. 功能相近的实例方法](#2-功能相近的实例方法)
        - [3. 与正则表达式合作的实例方法](#3-与正则表达式合作的实例方法)
        - [4. 把字符串转换为数组的实例方法](#4-把字符串转换为数组的实例方法)

<!-- /TOC -->

## 概述

String对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。

```str
var s1 = 'abc';
var s2 = new String('abc');

typeof s1 // "string"
typeof s2 // "object"

s2.valueOf() // "abc"
```

字符串对象是一个类似数组的对象（很像数组，但不是数组）。

```str
new String('abc')
// String {0: "a", 1: "b", 2: "c", length: 3}

(new String('abc'))[1] // "b"
```

除了用作构造函数，String对象还可以当作工具方法使用，将任意类型的值转为字符串。

```str
String(true) // "true"
String(5) // "5"
```

## 静态方法

String对象提供的静态方法，主要是String.fromCharCode()。该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这些码点组成的字符串。

```str
String.fromCharCode() // ""
String.fromCharCode(97) // "a"
String.fromCharCode(104, 101, 108, 108, 111)
// "hello"
```

## 实例属性

字符串实例的length属性返回字符串的长度。

```str
'abc'.length // 3
```

## 实例方法

### 1. charAt

charAt方法返回指定位置的字符，参数是从0开始编号的位置。

```str
var s = new String('abc');

s.charAt(1) // "b"
s.charAt(s.length - 1) // "c"
```

这个方法完全可以用数组下标替代。

```str
'abc'.charAt(1) // "b"
'abc'[1] // "b"
```

如果参数为负数，或大于等于字符串的长度，charAt返回空字符串。

```str
'abc'.charAt(-1) // ""
'abc'.charAt(3) // ""
```

### 2. charCodeAt

charCodeAt方法返回字符串指定位置的 Unicode 码点（十进制表示），相当于String.fromCharCode()的逆操作。

```str
'abc'.charCodeAt(1) // 98
```


如果没有任何参数，charCodeAt返回首字符的 Unicode 码点。

```str
'abc'.charCodeAt() // 97
```

如果参数为负数，或大于等于字符串的长度，charCodeAt返回NaN。

```str
'abc'.charCodeAt(-1) // NaN
'abc'.charCodeAt(4) // NaN
```

### 3. concat

concat方法用于连接两个字符串，返回一个新字符串，不改变原字符串。

```str
var s1 = 'abc';
var s2 = 'def';

s1.concat(s2) // "abcdef"
s1 // "abc"
```

该方法可以接受多个参数。

```str
'a'.concat('b', 'c') // "abc"
```

如果参数不是字符串，concat方法会将其先转为字符串，然后再连接。

```str
var one = 1;
var two = 2;
var three = '3';

''.concat(one, two, three) // "123"
one + two + three // "33"
```

### 4. slice

slice方法用于从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）。

```str
'JavaScript'.slice(0, 4) // "Java"
```

如果省略第二个参数，则表示子字符串一直到原字符串结束。

```str
'JavaScript'.slice(4) // "Script"
```

如果参数是负值，表示从结尾开始倒数计算的位置，即该负值加上字符串长度。

```str
'JavaScript'.slice(-6) // "Script"
'JavaScript'.slice(0, -6) // "Java"
'JavaScript'.slice(-2, -1) // "p"
```

如果第一个参数大于第二个参数，slice方法返回一个空字符串。

```str
'JavaScript'.slice(2, 1) // ""
```

### 5. substring

substring方法用于从原字符串取出子字符串并返回，不改变原字符串，跟slice方法很相像。它的第一个参数表示子字符串的开始位置，第二个位置表示结束位置（返回结果不含该位置）。

```str
'JavaScript'.substring(0, 4) // "Java"
```

如果省略第二个参数，则表示子字符串一直到原字符串的结束。

```str
'JavaScript'.substring(4) // "Script"
```

如果第一个参数大于第二个参数，substring方法会自动更换两个参数的位置。

```str
'JavaScript'.substring(10, 4) // "Script"
// 等同于
'JavaScript'.substring(4, 10) // "Script"
```

上面代码中，调换substring方法的两个参数，都得到同样的结果。

如果参数是负数，substring方法会自动将负数转为0。

```str
'JavaScript'.substring(-3) // "JavaScript"
'JavaScript'.substring(4, -3) // "Java"
```

### 6. substr

substr方法的第一个参数是子字符串的开始位置（从0开始计算），第二个参数是子字符串的长度。

```str
'JavaScript'.substr(4, 6) // "Script"
```

如果省略第二个参数，则表示子字符串一直到原字符串的结束。

```str
'JavaScript'.substr(4) // "Script"
```

如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将被自动转为0，因此会返回空字符串。

```str
'JavaScript'.substr(-6) // "Script"
'JavaScript'.substr(4, -1) // ""
```

### 7. indexOf lastIndexOf

indexOf方法用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置。如果返回-1，就表示不匹配。

```str
'hello world'.indexOf('o') // 4
'JavaScript'.indexOf('script') // -1
```

indexOf方法还可以接受第二个参数，表示从该位置开始向后匹配。

```str
'hello world'.indexOf('o', 6) // 7
```

lastIndexOf方法的用法跟indexOf方法一致，主要的区别是lastIndexOf从尾部开始匹配，indexOf则是从头部开始匹配。

```str
'hello world'.lastIndexOf('o') // 7
```

另外，lastIndexOf的第二个参数表示从该位置起向前匹配。

```str
'hello world'.lastIndexOf('o', 6) // 4
```

### 8. trim

trim方法用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。

```str
'  hello world  '.trim()
// "hello world"
```

该方法去除的不仅是空格，还包括制表符（\t、\v）、换行符（\n）和回车符（\r）。

```str
'\r\nabc \t'.trim() // 'abc'
```

### 9. toLowerCase toUpperCase

toLowerCase方法用于将一个字符串全部转为小写，toUpperCase则是全部转为大写。它们都返回一个新字符串，不改变原字符串。

```str
'Hello World'.toLowerCase()
// "hello world"

'Hello World'.toUpperCase()
// "HELLO WORLD"
```

### 10. match

match方法用于确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串。如果没有找到匹配，则返回null。

```str
'cat, bat, sat, fat'.match('at') // ["at"]
'cat, bat, sat, fat'.match('xt') // null
```

返回的数组还有index属性和input属性，分别表示匹配字符串开始的位置和原始字符串。

```str
var matches = 'cat, bat, sat, fat'.match('at');
matches.index // 1
matches.input // "cat, bat, sat, fat
```

### 11. search replace

search方法的用法基本等同于match，但是返回值为匹配的第一个位置。如果没有找到匹配，则返回-1。

```str
'cat, bat, sat, fat'.search('at') // 1
```

search方法还可以使用正则表达式作为参数，详见《正则表达式》一节。

replace方法用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有g修饰符的正则表达式）。

```str
'aaa'.replace('a', 'b') // "baa"
```

### 12. split

split方法按照给定规则分割字符串，返回一个由分割出来的子字符串组成的数组。

```str
'a|b|c'.split('|') // ["a", "b", "c"]
```

如果分割规则为空字符串，则返回数组的成员是原字符串的每一个字符。

```str
'a|b|c'.split('') // ["a", "|", "b", "|", "c"]
```

如果省略参数，则返回数组的唯一成员就是原字符串。

```str
'a|b|c'.split() // ["a|b|c"]
```

如果满足分割规则的两个部分紧邻着（即两个分割符中间没有其他字符），则返回数组之中会有一个空字符串。

```str
'a||c'.split('|') // ['a', '', 'c']
```

如果满足分割规则的部分处于字符串的开头或结尾（即它的前面或后面没有其他字符），则返回数组的第一个或最后一个成员是一个空字符串。

```str
'|b|c'.split('|') // ["", "b", "c"]
'a|b|'.split('|') // ["a", "b", ""]
```

split方法还可以接受第二个参数，限定返回数组的最大成员数。

```str
'a|b|c'.split('|', 0) // []
'a|b|c'.split('|', 1) // ["a"]
'a|b|c'.split('|', 2) // ["a", "b"]
'a|b|c'.split('|', 3) // ["a", "b", "c"]
'a|b|c'.split('|', 4) // ["a", "b", "c"]
```

### 13. localeCompare

localeCompare方法用于比较两个字符串。它返回一个整数，如果小于0，表示第一个字符串小于第二个字符串；如果等于0，表示两者相等；如果大于0，表示第一个字符串大于第二个字符串。

```str
'apple'.localeCompare('banana') // -1
'apple'.localeCompare('apple') // 0
```

该方法的最大特点，就是会考虑自然语言的顺序。举例来说，正常情况下，大写的英文字母小于小写字母。

```str
'B' > 'a' // false
```

上面代码中，字母B小于字母a。因为 JavaScript 采用的是 Unicode 码点比较，B的码点是66，而a的码点是97。

但是，localeCompare方法会考虑自然语言的排序情况，将B排在a的前面。

```str
'B'.localeCompare('a') // 1
```

上面代码中，localeCompare方法返回整数1，表示B较大。

localeCompare还可以有第二个参数，指定所使用的语言（默认是英语），然后根据该语言的规则进行比较。

```str
'ä'.localeCompare('z', 'de') // -1
'ä'.localeCompare('z', 'sv') // 1
```

上面代码中，de表示德语，sv表示瑞典语。德语中，ä小于z，所以返回-1；瑞典语中，ä大于z，所以返回1。

## 总结

### 1. 与数组类似的实例方法

- concat

- slice

- indexOf

- lastIndexOf

### 2. 功能相近的实例方法

- chartAt, chartCodeAt

- slice, substring, substr

- match, search

### 3. 与正则表达式合作的实例方法

- match

- search

- replace

### 4. 把字符串转换为数组的实例方法

```str
var str='hello';
str.split('');
// ["h", "e", "l", "l", "o"]
```
