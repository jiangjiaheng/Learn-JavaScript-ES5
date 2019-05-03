# null && undefined && boolean

## null && undefined

在`if`语句中，null和undefined都会被自动转为`false`

```null
if (!undefined) {
  console.log('undefined is false');
}
// undefined is false

if (!null) {
  console.log('null is false');
}
// null is false
```

null表示空值，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入null，表示该参数为空。

```null
Number(null) // 0
5 + null // 5

var i=null
i
// null
```

undefined是一个表示"此处无定义"的原始值，转为数值时为`NaN`

```undefined
Number(undefined) // NaN
5 + undefined // NaN
```

```undefined
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

## boolean

返回布尔值的运算符：

- 前置逻辑运算符： ! (Not)

- 相等运算符：===，!==，==，!=

- 比较运算符：>，>=，<，<=

条件判读自动转为false的值：

- undefined

- null

- false

- 0

- NaN

- '' "" (空字符串)

```boolean
if ('') {
  console.log('true');
}
// 没有任何输出

if ([]) {
  console.log('true');
}
// true

if ({}) {
  console.log('true');
}
// true
```