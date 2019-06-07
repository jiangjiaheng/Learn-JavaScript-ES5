# JSON对象

<!-- TOC -->

- [JSON对象](#json对象)
    - [JSON格式](#json格式)
    - [JSON 对象](#json-对象)
    - [stringify](#stringify)
        - [1. 基本用法](#1-基本用法)
        - [2. 第二个参数](#2-第二个参数)
        - [3. 第三个参数](#3-第三个参数)
        - [4. toJSON](#4-tojson)
    - [parse](#parse)

<!-- /TOC -->

## JSON格式

JSON 格式（JavaScript Object Notation 的缩写）是一种用于数据交换的文本格式，2001年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式。

JSON 对值的类型和格式有严格的规定。

> 1.复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。<br><br>
2.原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。<br><br>
3.字符串必须使用双引号表示，不能使用单引号。<br><br>
4.对象的键名必须放在双引号里面。<br><br>
5.数组或对象最后一个成员的后面，不能加逗号。

以下都是合法的 JSON。

```js
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```

以下都是不合法的 JSON。

```js
{ name: "张三", 'age': 32 }  // 属性名必须使用双引号

[32, 64, 128, 0xFFF] // 不能使用十六进制值

{ "name": "张三", "age": undefined } // 不能使用 undefined

{ "name": "张三",
  "birthday": new Date('Fri, 26 Aug 2011 07:13:10 GMT'),
  "getName": function () {
      return this.name;
  }
} // 属性值不能使用函数和日期对象
```

注意，null、空数组和空对象都是合法的 JSON 值。

## JSON 对象

JSON对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：JSON.stringify()和JSON.parse()。

## stringify

### 1. 基本用法

JSON.stringify方法用于将一个值转为 JSON 字符串。该字符串符合 JSON 格式，并且可以被JSON.parse方法还原。

```js
JSON.stringify('abc') // ""abc""
JSON.stringify(1) // "1"
JSON.stringify(false) // "false"
JSON.stringify([]) // "[]"
JSON.stringify({}) // "{}"

JSON.stringify([1, "false", false])
// '[1,"false",false]'

JSON.stringify({ name: "张三" })
// '{"name":"张三"}'
```

上面代码将各种类型的值，转成 JSON 字符串。

注意，对于原始类型的字符串，转换结果会带双引号。

```js
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```

### 2. 第二个参数

JSON.stringify方法还可以接受一个数组，作为第二个参数，指定需要转成字符串的属性。

```js
var obj = {
  'prop1': 'value1',
  'prop2': 'value2',
  'prop3': 'value3'
};

var selectedProperties = ['prop1', 'prop2'];

JSON.stringify(obj, selectedProperties)
// "{"prop1":"value1","prop2":"value2"}"
```

第二个参数还可以是一个函数，用来更改JSON.stringify的返回值。

```js
function f(key, value) {
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}

JSON.stringify({ a: 1, b: 2 }, f)
// '{"a": 2,"b": 4}'
```

### 3. 第三个参数

JSON.stringify还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过10个）；如果是字符串（不超过10个字符），则该字符串会添加在每行前面。

```js
JSON.stringify({ p1: 1, p2: 2 }, null, 2);
/*
"{
  "p1": 1,
  "p2": 2
}"
*/

JSON.stringify({ p1:1, p2:2 }, null, '|-');
/*
"{
|-"p1": 1,
|-"p2": 2
}"
*/
```

### 4. toJSON

如果参数对象有自定义的toJSON方法，那么JSON.stringify会使用这个方法的返回值作为参数，而忽略原对象的其他属性。

下面是一个普通的对象。

```js
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  }
};

JSON.stringify(user)
// "{"firstName":"三","lastName":"张","fullName":"张三"}"
```

现在，为这个对象加上toJSON方法。

```js
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  },

  toJSON: function () {
    return {
      name: this.lastName + this.firstName
    };
  }
};

JSON.stringify(user)
// "{"name":"张三"}"
```

Date对象就有一个自己的toJSON方法。

```js
var date = new Date('2015-01-01');
date.toJSON() // "2015-01-01T00:00:00.000Z"
JSON.stringify(date) // ""2015-01-01T00:00:00.000Z""
```

## parse

JSON.parse方法用于将 JSON 字符串转换成对应的值。

```js
JSON.parse('{}') // {}
JSON.parse('true') // true
JSON.parse('"foo"') // "foo"
JSON.parse('[1, 5, "false"]') // [1, 5, "false"]
JSON.parse('null') // null

var o = JSON.parse('{"name": "张三"}');
o.name // 张三
```

如果传入的字符串不是有效的 JSON 格式，JSON.parse方法将报错。

```js
JSON.parse("'String'") // illegal single quotes
// SyntaxError: Unexpected token ILLEGAL
```

上面代码中，双引号字符串中是一个单引号字符串，因为单引号字符串不符合 JSON 格式，所以报错。

为了处理解析错误，可以将JSON.parse方法放在try...catch代码块中。

```js
try {
  JSON.parse("'String'");
} catch(e) {
  console.log('parsing error');
}
```

JSON.parse方法可以接受一个处理函数，作为第二个参数，用法与JSON.stringify方法类似。

```js
function f(key, value) {
  if (key === 'a') {
    return value + 10;
  }
  return value;
}

JSON.parse('{"a": 1, "b": 2}', f)
// {a: 11, b: 2}
```

上面代码中，JSON.parse的第二个参数是一个函数，如果键名是a，该函数会将键值加上10。
