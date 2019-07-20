# NodeList 接口，HTMLCollection 接口

<!-- TOC -->

- [NodeList 接口，HTMLCollection 接口](#nodelist-接口htmlcollection-接口)
    - [NodeList 接口](#nodelist-接口)
        - [1. NodeList概述](#1-nodelist概述)
        - [2. NodeList.prototype.length](#2-nodelistprototypelength)
        - [3. NodeList.prototype.forEach()](#3-nodelistprototypeforeach)
        - [4. NodeList.prototype.item()](#4-nodelistprototypeitem)
        - [5. NodeList.prototype.keys()，NodeList.prototype.values()，NodeList.prototype.entries()](#5-nodelistprototypekeysnodelistprototypevaluesnodelistprototypeentries)
    - [HTMLCollection 接口](#htmlcollection-接口)
        - [1. HTMLCollection概述](#1-htmlcollection概述)
        - [2. HTMLCollection.prototype.length](#2-htmlcollectionprototypelength)
        - [3. HTMLCollection.prototype.item()](#3-htmlcollectionprototypeitem)
        - [4. HTMLCollection.prototype.namedItem()](#4-htmlcollectionprototypenameditem)

<!-- /TOC -->

## NodeList 接口

### 1. NodeList概述

NodeList实例是一个类似数组的对象，它的成员是节点对象。通过以下方法可以得到NodeList实例。

- Node.childNodes
- document.querySelectorAll()等节点搜索方法

```node
document.body.childNodes instanceof NodeList // true
```

NodeList实例很像数组，可以使用length属性和forEach方法。但是，它不是数组，不能使用pop或push之类数组特有的方法。

```node
var children = document.body.childNodes;

Array.isArray(children) // false

children.length // 34
children.forEach(console.log)
```

如果NodeList实例要使用数组方法，可以将其转为真正的数组。

```node
var children = document.body.childNodes;
var nodeArr = Array.prototype.slice.call(children);
```

除了使用forEach方法遍历 NodeList 实例，还可以使用for循环。

```node
var children = document.body.childNodes;

for (var i = 0; i < children.length; i++) {
  var item = children[i];
}
```

注意，NodeList 实例可能是动态集合，也可能是静态集合。所谓动态集合就是一个活的集合，DOM 删除或新增一个相关节点，都会立刻反映在 NodeList 实例。目前，只有Node.childNodes返回的是一个动态集合，其他的 NodeList 都是静态集合。

```node
var children = document.body.childNodes;
children.length // 18
document.body.appendChild(document.createElement('p'));
children.length // 19
```

### 2. NodeList.prototype.length

length属性返回 NodeList 实例包含的节点数量。

```node
document.querySelectorAll('xxx').length
// 0
```

### 3. NodeList.prototype.forEach()

forEach方法用于遍历 NodeList 的所有成员。它接受一个回调函数作为参数，每一轮遍历就执行一次这个回调函数，用法与数组实例的forEach方法完全一致。

```node
var children = document.body.childNodes;
children.forEach(function f(item, i, list) {
  // ...
}, this);
```

### 4. NodeList.prototype.item()

item方法接受一个整数值作为参数，表示成员的位置，返回该位置上的成员。

```node
document.body.childNodes.item(0)
```

上面代码中，item(0)返回第一个成员。

所有类似数组的对象，都可以使用方括号运算符取出成员。一般情况下，都是使用方括号运算符，而不使用item方法。

```node
document.body.childNodes[0]
```

### 5. NodeList.prototype.keys()，NodeList.prototype.values()，NodeList.prototype.entries()

这三个方法都返回一个 ES6 的遍历器对象，可以通过for...of循环遍历获取每一个成员的信息。区别在于，keys()返回键名的遍历器，values()返回键值的遍历器，entries()返回的遍历器同时包含键名和键值的信息。

```node
var children = document.body.childNodes;

for (var key of children.keys()) {
  console.log(key);
}
// 0
// 1
// 2
// ...

for (var value of children.values()) {
  console.log(value);
}
// #text
// <script>
// ...

for (var entry of children.entries()) {
  console.log(entry);
}
// Array [ 0, #text ]
// Array [ 1, <script> ]
// ...
```

## HTMLCollection 接口

### 1. HTMLCollection概述

HTMLCollection是一个节点对象的集合，只能包含元素节点（element），不能包含其他类型的节点。它的返回值是一个类似数组的对象，但是与NodeList接口不同，HTMLCollection没有forEach方法，只能使用for循环遍历。

返回HTMLCollection实例的，主要是一些Document对象的集合属性，比如document.links、document.forms、document.images等。

```node
document.links instanceof HTMLCollection // true
```

HTMLCollection实例都是动态集合，节点的变化会实时反映在集合中。

如果元素节点有id或name属性，那么HTMLCollection实例上面，可以使用id属性或name属性引用该节点元素。如果没有对应的节点，则返回null。

```node
// HTML 代码如下
// <img id="pic" src="http://example.com/foo.jpg">

var pic = document.getElementById('pic');
document.images.pic === pic // true
```

上面代码中，document.images是一个HTMLCollection实例，可以通过`<img>`元素的id属性值，从HTMLCollection实例上取到这个元素。

### 2. HTMLCollection.prototype.length

length属性返回HTMLCollection实例包含的成员数量。

```node
document.links.length // 18
```

### 3. HTMLCollection.prototype.item()

item方法接受一个整数值作为参数，表示成员的位置，返回该位置上的成员。

```node
var c = document.images;
var img0 = c.item(0);
```

### 4. HTMLCollection.prototype.namedItem()

namedItem方法的参数是一个字符串，表示id属性或name属性的值，返回对应的元素节点。如果没有对应的节点，则返回null。

```node
// HTML 代码如下
// <img id="pic" src="http://example.com/foo.jpg">

var pic = document.getElementById('pic');
document.images.namedItem('pic') === pic // true
```
