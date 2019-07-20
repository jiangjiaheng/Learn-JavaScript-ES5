# ParentNode 接口，ChildNode 接口

<!-- TOC -->

- [ParentNode 接口，ChildNode 接口](#parentnode-接口childnode-接口)
    - [ParentNode 接口](#parentnode-接口)
        - [1. ParentNode.children](#1-parentnodechildren)
        - [2. ParentNode.firstElementChild](#2-parentnodefirstelementchild)
        - [3. ParentNode.lastElementChild](#3-parentnodelastelementchild)
        - [4. ParentNode.childElementCount](#4-parentnodechildelementcount)
        - [5. ParentNode.append()，ParentNode.prepend()](#5-parentnodeappendparentnodeprepend)
    - [ChildNode 接口](#childnode-接口)
        - [1. ChildNode.remove()](#1-childnoderemove)
        - [2. ChildNode.before()，ChildNode.after()](#2-childnodebeforechildnodeafter)
        - [3. ChildNode.replaceWith()](#3-childnodereplacewith)

<!-- /TOC -->

## ParentNode 接口

如果当前节点是父节点，就会继承ParentNode接口。由于只有元素节点（element）、文档节点（document）和文档片段节点（documentFragment）拥有子节点，因此只有这三类节点会继承ParentNode接口。

### 1. ParentNode.children

children属性返回一个HTMLCollection实例，成员是当前节点的所有元素子节点。该属性只读。

下面是遍历某个节点的所有元素子节点的示例。

```node
for (var i = 0; i < el.children.length; i++) {
  // ...
}
```

注意，children属性只包括元素子节点，不包括其他类型的子节点（比如文本子节点）。如果没有元素类型的子节点，返回值HTMLCollection实例的length属性为0。

另外，HTMLCollection是动态集合，会实时反映 DOM 的任何变化。

### 2. ParentNode.firstElementChild

firstElementChild属性返回当前节点的第一个元素子节点。如果没有任何元素子节点，则返回null。

```node
document.firstElementChild.nodeName
// "HTML"
```

上面代码中，document节点的第一个元素子节点是`<HTML>`。

### 3. ParentNode.lastElementChild

lastElementChild属性返回当前节点的最后一个元素子节点，如果不存在任何元素子节点，则返回null。

```node
document.lastElementChild.nodeName
// "HTML"
```

上面代码中，document节点的最后一个元素子节点是<HTML>（因为document只包含这一个元素子节点）。

### 4. ParentNode.childElementCount

childElementCount属性返回一个整数，表示当前节点的所有元素子节点的数目。如果不包含任何元素子节点，则返回0。

```node
document.body.childElementCount // 13
```

### 5. ParentNode.append()，ParentNode.prepend()

append方法为当前节点追加一个或多个子节点，位置是最后一个元素子节点的后面。

该方法不仅可以添加元素子节点，还可以添加文本子节点。

```node
var parent = document.body;

// 添加元素子节点
var p = document.createElement('p');
parent.append(p);

// 添加文本子节点
parent.append('Hello');

// 添加多个元素子节点
var p1 = document.createElement('p');
var p2 = document.createElement('p');
parent.append(p1, p2);

// 添加元素子节点和文本子节点
var p = document.createElement('p');
parent.append('Hello', p);
```

## ChildNode 接口

### 1. ChildNode.remove()

remove方法用于从父节点移除当前节点。

```node
el.remove()
```

上面代码在 DOM 里面移除了el节点。

### 2. ChildNode.before()，ChildNode.after()

before方法用于在当前节点的前面，插入一个或多个同级节点。两者拥有相同的父节点。

注意，该方法不仅可以插入元素节点，还可以插入文本节点。

```node
var p = document.createElement('p');
var p1 = document.createElement('p');

// 插入元素节点
el.before(p);

// 插入文本节点
el.before('Hello');

// 插入多个元素节点
el.before(p, p1);

// 插入元素节点和文本节点
el.before(p, 'Hello');
```

after方法用于在当前节点的后面，插入一个或多个同级节点，两者拥有相同的父节点。用法与before方法完全相同。

### 3. ChildNode.replaceWith()

replaceWith方法使用参数节点，替换当前节点。参数可以是元素节点，也可以是文本节点。

```node
var span = document.createElement('span');
el.replaceWith(span);
```

上面代码中，el节点将被span节点替换。
