# Javascript的基本语法

## 语句

语句（statement）是为了完成某种任务而进行的操作，比如下面就是一行赋值语句

```var
var a = 1 + 3;
```

`var`是变量的声明符号，`1+3`是表达式，`;`表示语句结束。

## 变量

变量是对“值”的具名引用。变量就是为“值”起名，然后引用这个名字，就等同于引用这个值。变量的名字就是变量名。

```var
var a=1;

var a; a=1;

var a; a // undefined

var a, b

var a=1; a='hello'

var x=1; var x=2

console.log(a); var a=1

// 这种语句叫变量提升，但是在ES6中，如果用`let`定义变量，变量提升将会报错
```

## 标识符

标识符（identifier）指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及后面要提到的函数名。JavaScript 语言的标识符对大小写敏感，所以a和A是两个不同的标识符。

简单说，标识符命名规则如下：

- 第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）。

- 第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字0-9。

- 合法标识符：`arg0 _tmp $elem π 中文`

- 不合法标识符：`1a 23 *** a+b -d`

- Javascript的保留字，也不能用作标识符：`break case catch try` 等

## 注释

- 行注释：`// 行注释`

- 块注释：`/* 块注释 */`

## 区块

JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。

对于var命令来说，JavaScript 的区块不构成单独的作用域（scope）。

```var
{var a=1;} a //1
```

区块往往用来构成其他更复杂的语法结构，比如for、if、while、function等。

在ES6中，`let`命令可以构成单独的作用域。

## 条件语句

### 1. if

```if
if(m===3)
m=m+1

if(m===3){
    m+=1;
}
```

### 2. if else

```if else
if (m === 0) {
  // ...
} else if (m === 1) {
  // ...
} else if (m === 2) {
  // ...
} else {
  // ...
}
```

```if else{ if else }
var m = 1;
var n = 2;
if (m !== 1)
if (n === 2) console.log('hello');
else console.log('world');
```

```if else{ if else }
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  } else {
    console.log('world');
  }
}
```

### 3. switch

```switch
var x=1;

switch (x) {
  case 1:
    console.log('x 等于1');
    break;
  case 2:
    console.log('x 等于2');
    break;
  default:
    console.log('x 等于其他值');
}
```

### 4. 三元运算符

```?:
var msg = '数字' + n + '是' + (n % 2 === 0 ? '偶数' : '奇数');
```

## 循环语句

### 1. while

```while
while (条件)
  语句;

// 或者
while (条件) 语句;

// 或者
while (条件) {
  语句;
}
```

```while
var i = 0;

while (i < 100) {
  console.log('i 当前为：' + i);
  i = i + 1;
}
```

### 2. for

```for
for (初始化表达式; 条件; 递增表达式)
  语句

// 或者

for (初始化表达式; 条件; 递增表达式) {
  语句
}
```

```for
var x = 3;
for (var i = 0; i < x; i++) {
  console.log(i);
}
// 0
// 1
// 2
```

### 3. do while

```do while
do
  语句
while (条件);

// 或者
do {
  语句
} while (条件);
```

```do while
var x = 3;
var i = 0;

do {
  console.log(i);
  i++;
} while(i < x);
```

### 4. break和continue

```break
var i = 0;

while(i < 100) {
  console.log('i 当前为：' + i);
  i++;
  if (i === 10) break;
}
```

```continue
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}
```

## 标签

```label
label:
  语句
```

```label
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```

```label
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

```label
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```