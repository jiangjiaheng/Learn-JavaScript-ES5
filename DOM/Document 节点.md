# Document 节点

<!-- TOC -->

- [Document 节点](#document-节点)
    - [概述](#概述)
    - [属性](#属性)
        - [1. 快捷方式属性](#1-快捷方式属性)
            - [1. document.defaultView](#1-documentdefaultview)
            - [2. document.doctype](#2-documentdoctype)
            - [3. document.documentElement](#3-documentdocumentelement)
            - [4. document.body，document.head](#4-documentbodydocumenthead)
            - [5. document.scrollingElement](#5-documentscrollingelement)
            - [6. document.activeElement](#6-documentactiveelement)
            - [7. document.fullscreenElement](#7-documentfullscreenelement)
        - [2. 节点集合属性](#2-节点集合属性)
            - [1. document.links](#1-documentlinks)
            - [2. document.forms](#2-documentforms)
            - [3. document.images](#3-documentimages)
            - [4. document.embeds，document.plugins](#4-documentembedsdocumentplugins)
            - [5. document.scripts](#5-documentscripts)
            - [6. document.styleSheets](#6-documentstylesheets)
            - [7. 小结](#7-小结)
        - [3. 文档静态信息属性](#3-文档静态信息属性)
            - [1. document.documentURI，document.URL](#1-documentdocumenturidocumenturl)
            - [2. document.domain](#2-documentdomain)
            - [3. document.location](#3-documentlocation)
            - [4. document.lastModified](#4-documentlastmodified)
            - [5. document.title](#5-documenttitle)
            - [6. document.characterSet](#6-documentcharacterset)
            - [7. document.referrer](#7-documentreferrer)
            - [8. document.dir](#8-documentdir)
            - [9. document.compatMode](#9-documentcompatmode)
        - [4. 文档状态属性](#4-文档状态属性)
            - [1. document.hidden](#1-documenthidden)
            - [2. document.visibilityState](#2-documentvisibilitystate)
            - [3. document.readyState](#3-documentreadystate)
        - [5. document.cookie](#5-documentcookie)
        - [6. document.designMode](#6-documentdesignmode)
        - [7. document.implementation](#7-documentimplementation)
    - [方法](#方法)
        - [1. 操作类方法](#1-操作类方法)
            - [1. document.open()，document.close()](#1-documentopendocumentclose)
            - [2. document.write()，document.writeln()](#2-documentwritedocumentwriteln)
        - [2. 获取类方法](#2-获取类方法)
            - [1.document.querySelector()，document.querySelectorAll()](#1documentqueryselectordocumentqueryselectorall)
            - [2. document.getElementsByTagName()](#2-documentgetelementsbytagname)
            - [3. document.getElementsByClassName()](#3-documentgetelementsbyclassname)
            - [4. document.getElementsByName()](#4-documentgetelementsbyname)
            - [5. document.getElementById()](#5-documentgetelementbyid)
            - [6. document.elementFromPoint()，document.elementsFromPoint()](#6-documentelementfrompointdocumentelementsfrompoint)
            - [7. document.caretPositionFromPoint()](#7-documentcaretpositionfrompoint)
        - [3. 创建类方法](#3-创建类方法)
            - [1. document.createElement()](#1-documentcreateelement)
            - [2. document.createTextNode()](#2-documentcreatetextnode)
            - [3. document.createAttribute()](#3-documentcreateattribute)
            - [4. document.createComment()](#4-documentcreatecomment)
            - [5. document.createDocumentFragment()](#5-documentcreatedocumentfragment)
            - [6. document.createEvent()](#6-documentcreateevent)
        - [4. document.addEventListener()，document.removeEventListener()，document.dispatchEvent()](#4-documentaddeventlistenerdocumentremoveeventlistenerdocumentdispatchevent)
        - [5. document.hasFocus()](#5-documenthasfocus)
        - [6. document.adoptNode()，document.importNode()](#6-documentadoptnodedocumentimportnode)
        - [7. document.createNodeIterator()](#7-documentcreatenodeiterator)
        - [8. document.createTreeWalker()](#8-documentcreatetreewalker)
        - [9. document.execCommand()，document.queryCommandSupported()，document.queryCommandEnabled()](#9-documentexeccommanddocumentquerycommandsupporteddocumentquerycommandenabled)

<!-- /TOC -->

## 概述

`document`节点对象代表整个文档，每张网页都有自己的`document`对象。`window.document`属性就指向这个对象。只要浏览器开始载入 `HTML` 文档，该对象就存在了，可以直接使用。

document对象有不同的办法可以获取。

- 正常的网页，直接使用document或window.document。
- iframe框架里面的网页，使用iframe节点的contentDocument属性。
- Ajax 操作返回的文档，使用XMLHttpRequest对象的responseXML属性。
- 内部节点的ownerDocument属性。

document对象继承了EventTarget接口、Node接口、ParentNode接口。这意味着，这些接口的方法都可以在document对象上调用。除此之外，document对象还有很多自己的属性和方法。

## 属性

### 1. 快捷方式属性

以下属性是指向文档内部的某个节点的快捷方式。

#### 1. document.defaultView

document.defaultView属性返回document对象所属的window对象。如果当前文档不属于window对象，该属性返回null。

```node
document.defaultView === window // true
```

#### 2. document.doctype

对于 HTML 文档来说，document对象一般有两个子节点。第一个子节点是document.doctype，指向<DOCTYPE>节点，即文档类型（Document Type Declaration，简写DTD）节点。HTML 的文档类型节点，一般写成`<!DOCTYPE html>`。如果网页没有声明 DTD，该属性返回null。

```node
var doctype = document.doctype;
doctype // "<!DOCTYPE html>"
doctype.name // "html"
```

`document.firstChild`通常就返回这个节点。

#### 3. document.documentElement

`document.documentElement`属性返回当前文档的根元素节点（`root`）。它通常是`document`节点的第二个子节点，紧跟在`document.doctype`节点后面。HTML网页的该属性，一般是`<html>`节点。

#### 4. document.body，document.head

document.body属性指向`<body>`节点，document.head属性指向`<head>`节点。

这两个属性总是存在的，如果网页源码里面省略了`<head>`或`<body>`，浏览器会自动创建。另外，这两个属性是可写的，如果改写它们的值，相当于移除所有子节点。

#### 5. document.scrollingElement

document.scrollingElement属性返回文档的滚动元素。也就是说，当文档整体滚动时，到底是哪个元素在滚动。

标准模式下，这个属性返回的文档的根元素document.documentElement（即`<html>`）。兼容（quirk）模式下，返回的是`<body>`元素，如果该元素不存在，返回null。

```node
// 页面滚动到浏览器顶部
document.scrollingElement.scrollTop = 0;
```

#### 6. document.activeElement

document.activeElement属性返回获得当前焦点（focus）的 DOM 元素。通常，这个属性返回的是`<input>`、`<textarea>`、`<select>`等表单元素，如果当前没有焦点元素，返回`<body>`元素或null。

#### 7. document.fullscreenElement

document.fullscreenElement属性返回当前以全屏状态展示的 DOM 元素。如果不是全屏状态，该属性返回null。

```node
if (document.fullscreenElement.nodeName == 'VIDEO') {
  console.log('全屏播放视频');
}
```

上面代码中，通过document.fullscreenElement可以知道`<video>`元素有没有处在全屏状态，从而判断用户行为。

### 2. 节点集合属性

以下属性返回一个HTMLCollection实例，表示文档内部特定元素的集合。这些集合都是动态的，原节点有任何变化，立刻会反映在集合中。

#### 1. document.links

document.links属性返回当前文档所有设定了href属性的`<a>`及`<area>`节点。

```node
// 打印文档所有的链接
var links = document.links;
for(var i = 0; i < links.length; i++) {
  console.log(links[i]);
}
```

#### 2. document.forms

document.forms属性返回所有`<form>`表单节点。

```node
var selectForm = document.forms[0];
```

上面代码获取文档第一个表单。

除了使用位置序号，id属性和name属性也可以用来引用表单。

```node
/* HTML 代码如下
  <form name="foo" id="bar"></form>
*/
document.forms[0] === document.forms.foo // true
document.forms.bar === document.forms.foo // true
```

#### 3. document.images

document.images属性返回页面所有`<img>`图片节点。

```node
var imglist = document.images;

for(var i = 0; i < imglist.length; i++) {
  if (imglist[i].src === 'banner.gif') {
    // ...
  }
}
```

上面代码在所有img标签中，寻找某张图片。

#### 4. document.embeds，document.plugins

document.embeds属性和document.plugins属性，都返回所有`<embed>`节点。

#### 5. document.scripts

document.scripts属性返回所有`<script>`节点。

```node
var scripts = document.scripts;
if (scripts.length !== 0 ) {
  console.log('当前网页有脚本');
}
```

#### 6. document.styleSheets

document.styleSheets属性返回文档内嵌或引入的样式表集合，详细介绍请看《CSS 对象模型》一章。

#### 7. 小结

除了document.styleSheets，以上的集合属性返回的都是HTMLCollection实例。

```node
document.links instanceof HTMLCollection // true
document.images instanceof HTMLCollection // true
document.forms instanceof HTMLCollection // true
document.embeds instanceof HTMLCollection // true
document.scripts instanceof HTMLCollection // true
```

HTMLCollection实例是类似数组的对象，所以这些属性都有length属性，都可以使用方括号运算符引用成员。如果成员有id或name属性，还可以用这两个属性的值，在HTMLCollection实例上引用到这个成员。

```node
// HTML 代码如下
// <form name="myForm">
document.myForm === document.forms.myForm // true
```

### 3. 文档静态信息属性

以下属性返回文档信息。

#### 1. document.documentURI，document.URL

document.documentURI属性和document.URL属性都返回一个字符串，表示当前文档的网址。不同之处是它们继承自不同的接口，documentURI继承自Document接口，可用于所有文档；URL继承自HTMLDocument接口，只能用于 HTML 文档。

```node
document.URL
// http://www.example.com/about

document.documentURI === document.URL
// true
```

如果文档的锚点（#anchor）变化，这两个属性都会跟着变化。

#### 2. document.domain

document.domain属性返回当前文档的域名，不包含协议和端口。比如，网页的网址是`http://www.example.com:80/hello.html`，那么document.domain属性就等于www.example.com。如果无法获取域名，该属性返回null。

document.domain基本上是一个只读属性，只有一种情况除外。次级域名的网页，可以把document.domain设为对应的上级域名。比如，当前域名是a.sub.example.com，则document.domain属性可以设置为sub.example.com，也可以设为example.com。修改后，document.domain相同的两个网页，可以读取对方的资源，比如设置的 Cookie。

另外，设置document.domain会导致端口被改成null。因此，如果通过设置document.domain来进行通信，双方网页都必须设置这个值，才能保证端口相同。

#### 3. document.location

Location对象是浏览器提供的原生对象，提供 URL 相关的信息和操作方法。通过window.location和document.location属性，可以拿到这个对象。

关于这个对象的详细介绍，请看《浏览器模型》部分的《Location 对象》章节。

#### 4. document.lastModified

document.lastModified属性返回一个字符串，表示当前文档最后修改的时间。不同浏览器的返回值，日期格式是不一样的。

```node
document.lastModified
// "03/07/2018 11:18:27"
```

注意，document.lastModified属性的值是字符串，所以不能直接用来比较。Date.parse方法将其转为Date实例，才能比较两个网页。

```node
var lastVisitedDate = Date.parse('01/01/2018');
if (Date.parse(document.lastModified) > lastVisitedDate) {
  console.log('网页已经变更');
}
```

如果页面上有 JavaScript 生成的内容，document.lastModified属性返回的总是当前时间。

#### 5. document.title

document.title属性返回当前文档的标题。默认情况下，返回`<title>`节点的值。但是该属性是可写的，一旦被修改，就返回修改后的值。

```node
document.title = '新标题';
document.title // "新标题"
```

#### 6. document.characterSet

document.characterSet属性返回当前文档的编码，比如UTF-8、ISO-8859-1等等。

#### 7. document.referrer

document.referrer属性返回一个字符串，表示当前文档的访问者来自哪里。

```node
document.referrer
// "https://example.com/path"
```

如果无法获取来源，或者用户直接键入网址而不是从其他网页点击进入，document.referrer返回一个空字符串。

document.referrer的值，总是与 HTTP 头信息的Referer字段保持一致。但是，document.referrer的拼写有两个r，而头信息的Referer字段只有一个r。

#### 8. document.dir

document.dir返回一个字符串，表示文字方向。它只有两个可能的值：rtl表示文字从右到左，阿拉伯文是这种方式；ltr表示文字从左到右，包括英语和汉语在内的大多数文字采用这种方式。

#### 9. document.compatMode

compatMode属性返回浏览器处理文档的模式，可能的值为BackCompat（向后兼容模式）和CSS1Compat（严格模式）。

一般来说，如果网页代码的第一行设置了明确的DOCTYPE（比如`<!doctype html>`），document.compatMode的值都为CSS1Compat。

### 4. 文档状态属性

#### 1. document.hidden

document.hidden属性返回一个布尔值，表示当前页面是否可见。如果窗口最小化、浏览器切换了 Tab，都会导致导致页面不可见，使得document.hidden返回true。

这个属性是 Page Visibility API 引入的，一般都是配合这个 API 使用。

#### 2. document.visibilityState

document.visibilityState返回文档的可见状态。

它的值有四种可能。

- visible：页面可见。注意，页面可能是部分可见，即不是焦点窗口，前面被其他窗口部分挡住了。
- hidden：页面不可见，有可能窗口最小化，或者浏览器切换到了另一个 Tab。
- prerender：页面处于正在渲染状态，对于用户来说，该页面不可见。
- unloaded：页面从内存里面卸载了。

这个属性可以用在页面加载时，防止加载某些资源；或者页面不可见时，停掉一些页面功能。

#### 3. document.readyState

document.readyState属性返回当前文档的状态，共有三种可能的值。

- loading：加载 HTML 代码阶段（尚未完成解析）
- interactive：加载外部资源阶段
- complete：加载完成

这个属性变化的过程如下。

- 浏览器开始解析 HTML 文档，document.readyState属性等于loading。
- 浏览器遇到 HTML 文档中的`<script>`元素，并且没有async或defer属性，就暂停解析，开始执行脚本，这时document.readyState属性还是等于loading。
- HTML 文档解析完成，document.readyState属性变成interactive。
- 浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，document.readyState属性变成complete。

下面的代码用来检查网页是否加载成功。

```node
// 基本检查
if (document.readyState === 'complete') {
  // ...
}

// 轮询检查
var interval = setInterval(function() {
  if (document.readyState === 'complete') {
    clearInterval(interval);
    // ...
  }
}, 100);
```

另外，每次状态变化都会触发一个readystatechange事件。

### 5. document.cookie

document.cookie属性用来操作浏览器 Cookie，详见《浏览器模型》部分的《Cookie》章节。

### 6. document.designMode

document.designMode属性控制当前文档是否可编辑。该属性只有两个值on和off，默认值为off。一旦设为on，用户就可以编辑整个文档的内容。

下面代码打开iframe元素内部文档的designMode属性，就能将其变为一个所见即所得的编辑器。

```node
// HTML 代码如下
// <iframe id="editor" src="about:blank"></iframe>
var editor = document.getElementById('editor');
editor.contentDocument.designMode = 'on';
```

### 7. document.implementation

document.implementation属性返回一个DOMImplementation对象。该对象有三个方法，主要用于创建独立于当前文档的新的 Document 对象。

- DOMImplementation.createDocument()：创建一个 XML 文档。
- DOMImplementation.createHTMLDocument()：创建一个 HTML 文档。
- DOMImplementation.createDocumentType()：创建一个 DocumentType 对象。

下面是创建 HTML 文档的例子。

```node
var doc = document.implementation.createHTMLDocument('Title');
var p = doc.createElement('p');
p.innerHTML = 'hello world';
doc.body.appendChild(p);

document.replaceChild(
  doc.documentElement,
  document.documentElement
);
```

上面代码中，第一步生成一个新的 HTML 文档doc，然后用它的根元素document.documentElement替换掉document.documentElement。这会使得当前文档的内容全部消失，变成hello world。

## 方法

### 1. 操作类方法

#### 1. document.open()，document.close()

document.open方法清除当前文档所有内容，使得文档处于可写状态，供document.write方法写入内容。

document.close方法用来关闭document.open()打开的文档。

```node
document.open();
document.write('hello world');
document.close();
```

#### 2. document.write()，document.writeln()

document.write方法用于向当前文档写入内容。

在网页的首次渲染阶段，只要页面没有关闭写入（即没有执行document.close()），document.write写入的内容就会追加在已有内容的后面。

```node
// 页面显示“helloworld”
document.open();
document.write('hello');
document.write('world');
document.close();
```

注意，document.write会当作 HTML 代码解析，不会转义。

```node
document.write('<p>hello world</p>');
```

上面代码中，document.write会将<p>当作 HTML 标签解释。

如果页面已经解析完成（DOMContentLoaded事件发生之后），再调用write方法，它会先调用open方法，擦除当前文档所有内容，然后再写入。

```node
document.addEventListener('DOMContentLoaded', function (event) {
  document.write('<p>Hello World!</p>');
});

// 等同于
document.addEventListener('DOMContentLoaded', function (event) {
  document.open();
  document.write('<p>Hello World!</p>');
  document.close();
});
```

如果在页面渲染过程中调用write方法，并不会自动调用open方法。（可以理解成，open方法已调用，但close方法还未调用。）

```node
<html>
<body>
hello
<script type="text/javascript">
  document.write("world")
</script>
</body>
</html>
```

在浏览器打开上面网页，将会显示hello world。

document.write是 JavaScript 语言标准化之前就存在的方法，现在完全有更符合标准的方法向文档写入内容（比如对innerHTML属性赋值）。所以，除了某些特殊情况，应该尽量避免使用document.write这个方法。

document.writeln方法与write方法完全一致，除了会在输出内容的尾部添加换行符。

```node
document.write(1);
document.write(2);
// 12

document.writeln(1);
document.writeln(2);
// 1
// 2
//
```

注意，writeln方法添加的是 ASCII 码的换行符，渲染成 HTML 网页时不起作用，即在网页上显示不出换行。网页上的换行，必须显式写入<br>。

### 2. 获取类方法

#### 1.document.querySelector()，document.querySelectorAll()

document.querySelector方法接受一个 CSS 选择器作为参数，返回匹配该选择器的元素节点。如果有多个节点满足匹配条件，则返回第一个匹配的节点。如果没有发现匹配的节点，则返回null。

```node
var el1 = document.querySelector('.myclass');
var el2 = document.querySelector('#myParent > [ng-click]');
```

document.querySelectorAll方法与querySelector用法类似，区别是返回一个NodeList对象，包含所有匹配给定选择器的节点。

```node
elementList = document.querySelectorAll('.myclass');
```

这两个方法的参数，可以是逗号分隔的多个 CSS 选择器，返回匹配其中一个选择器的元素节点，这与 CSS 选择器的规则是一致的。

```node
var matches = document.querySelectorAll('div.note, div.alert');
```

上面代码返回class属性是note或alert的div元素。

这两个方法都支持复杂的 CSS 选择器。

```node
// 选中 data-foo-bar 属性等于 someval 的元素
document.querySelectorAll('[data-foo-bar="someval"]');

// 选中 myForm 表单中所有不通过验证的元素
document.querySelectorAll('#myForm :invalid');

// 选中div元素，那些 class 含 ignore 的除外
document.querySelectorAll('DIV:not(.ignore)');

// 同时选中 div，a，script 三类元素
document.querySelectorAll('DIV, A, SCRIPT');
```

但是，它们不支持 CSS 伪元素的选择器（比如:first-line和:first-letter）和伪类的选择器（比如:link和:visited），即无法选中伪元素和伪类。

如果querySelectorAll方法的参数是字符串*，则会返回文档中的所有元素节点。另外，querySelectorAll的返回结果不是动态集合，不会实时反映元素节点的变化。

最后，这两个方法除了定义在document对象上，还定义在元素节点上，即在元素节点上也可以调用。

#### 2. document.getElementsByTagName()

document.getElementsByTagName方法搜索 HTML 标签名，返回符合条件的元素。它的返回值是一个类似数组对象（HTMLCollection实例），可以实时反映 HTML 文档的变化。如果没有任何匹配的元素，就返回一个空集。

```node
var paras = document.getElementsByTagName('p');
paras instanceof HTMLCollection // true
```

上面代码返回当前文档的所有p元素节点。

HTML 标签名是大小写不敏感的，因此getElementsByTagName方法也是大小写不敏感的。另外，返回结果中，各个成员的顺序就是它们在文档中出现的顺序。

如果传入*，就可以返回文档中所有 HTML 元素。

```node
var allElements = document.getElementsByTagName('*');
```

注意，元素节点本身也定义了getElementsByTagName方法，返回该元素的后代元素中符合条件的元素。也就是说，这个方法不仅可以在document对象上调用，也可以在任何元素节点上调用。

```node
var firstPara = document.getElementsByTagName('p')[0];
var spans = firstPara.getElementsByTagName('span');
```

上面代码选中第一个p元素内部的所有span元素。

#### 3. document.getElementsByClassName()

document.getElementsByClassName方法返回一个类似数组的对象（HTMLCollection实例），包括了所有class名字符合指定条件的元素，元素的变化实时反映在返回结果中。

```node
var elements = document.getElementsByClassName(names);
```

由于class是保留字，所以 JavaScript 一律使用className表示 CSS 的class。

参数可以是多个class，它们之间使用空格分隔。

```node
var elements = document.getElementsByClassName('foo bar');
```

上面代码返回同时具有foo和bar两个class的元素，foo和bar的顺序不重要。

注意，正常模式下，CSS 的class是大小写敏感的。（quirks mode下，大小写不敏感。）

与getElementsByTagName方法一样，getElementsByClassName方法不仅可以在document对象上调用，也可以在任何元素节点上调用。

```node
// 非document对象上调用
var elements = rootElement.getElementsByClassName(names);
```

#### 4. document.getElementsByName()

document.getElementsByName方法用于选择拥有name属性的 HTML 元素（比如`<form>`、`<radio>`、`<img>`、`<frame>`、`<embed>`和`<object>`等），返回一个类似数组的的对象（NodeList实例），因为name属性相同的元素可能不止一个。

```node
// 表单为 <form name="x"></form>
var forms = document.getElementsByName('x');
forms[0].tagName // "FORM"
```

#### 5. document.getElementById()

document.getElementById方法返回匹配指定id属性的元素节点。如果没有发现匹配的节点，则返回null。

```node
var elem = document.getElementById('para1');
```

注意，该方法的参数是大小写敏感的。比如，如果某个节点的id属性是main，那么document.getElementById('Main')将返回null。

document.getElementById方法与document.querySelector方法都能获取元素节点，不同之处是document.querySelector方法的参数使用 CSS 选择器语法，document.getElementById方法的参数是元素的id属性。

```node
document.getElementById('myElement')
document.querySelector('#myElement')
```

上面代码中，两个方法都能选中id为myElement的元素，但是document.getElementById()比document.querySelector()效率高得多。

另外，这个方法只能在document对象上使用，不能在其他元素节点上使用。

#### 6. document.elementFromPoint()，document.elementsFromPoint()

document.elementFromPoint方法返回位于页面指定位置最上层的元素节点。

```node
var element = document.elementFromPoint(50, 50);
```

上面代码选中在(50, 50)这个坐标位置的最上层的那个 HTML 元素。

elementFromPoint方法的两个参数，依次是相对于当前视口左上角的横坐标和纵坐标，单位是像素。如果位于该位置的 HTML 元素不可返回（比如文本框的滚动条），则返回它的父元素（比如文本框）。如果坐标值无意义（比如负值或超过视口大小），则返回null。

document.elementsFromPoint()返回一个数组，成员是位于指定坐标（相对于视口）的所有元素。

```node
var elements = document.elementsFromPoint(x, y);
```

#### 7. document.caretPositionFromPoint()

document.caretPositionFromPoint()返回一个 CaretPosition 对象，包含了指定坐标点在节点对象内部的位置信息。CaretPosition 对象就是光标插入点的概念，用于确定光标点在文本对象内部的具体位置。

```node
var range = document.caretPositionFromPoint(clientX, clientY);
```

上面代码中，range是指定坐标点的 CaretPosition 对象。该对象有两个属性。

- CaretPosition.offsetNode：该位置的节点对象
- CaretPosition.offset：该位置在offsetNode对象内部，与起始位置相距的字符数。

### 3. 创建类方法

#### 1. document.createElement()

document.createElement方法用来生成元素节点，并返回该节点。

```node
var newDiv = document.createElement('div');
```

createElement方法的参数为元素的标签名，即元素节点的tagName属性，对于 HTML 网页大小写不敏感，即参数为div或DIV返回的是同一种节点。如果参数里面包含尖括号（即<和>）会报错。

```node
document.createElement('<div>');
// DOMException: The tag name provided ('<div>') is not a valid name
```

注意，document.createElement的参数可以是自定义的标签名。

```node
document.createElement('foo');
```

#### 2. document.createTextNode()

document.createTextNode方法用来生成文本节点（Text实例），并返回该节点。它的参数是文本节点的内容。

```node
var newDiv = document.createElement('div');
var newContent = document.createTextNode('Hello');
newDiv.appendChild(newContent);
```

上面代码新建一个div节点和一个文本节点，然后将文本节点插入div节点。

这个方法可以确保返回的节点，被浏览器当作文本渲染，而不是当作 HTML 代码渲染。因此，可以用来展示用户的输入，避免 XSS 攻击。

```node
var div = document.createElement('div');
div.appendChild(document.createTextNode('<span>Foo & bar</span>'));
console.log(div.innerHTML)
// &lt;span&gt;Foo &amp; bar&lt;/span&gt;
```

上面代码中，createTextNode方法对大于号和小于号进行转义，从而保证即使用户输入的内容包含恶意代码，也能正确显示。

需要注意的是，该方法不对单引号和双引号转义，所以不能用来对 HTML 属性赋值。

```node
function escapeHtml(str) {
  var div = document.createElement('div');
  div.appendChild(document.createTextNode(str));
  return div.innerHTML;
};

var userWebsite = '" onmouseover="alert(\'derp\')" "';
var profileLink = '<a href="' + escapeHtml(userWebsite) + '">Bob</a>';
var div = document.getElementById('target');
div.innerHTML = profileLink;
// <a href="" onmouseover="alert('derp')" "">Bob</a>
```

上面代码中，由于createTextNode方法不转义双引号，导致onmouseover方法被注入了代码。

#### 3. document.createAttribute()

document.createAttribute方法生成一个新的属性节点（Attr实例），并返回它。

```node
var attribute = document.createAttribute(name);
```

document.createAttribute方法的参数name，是属性的名称。

```node
var node = document.getElementById('div1');

var a = document.createAttribute('my_attrib');
a.value = 'newVal';

node.setAttributeNode(a);
// 或者
node.setAttribute('my_attrib', 'newVal');
```

上面代码为div1节点，插入一个值为newVal的my_attrib属性。

#### 4. document.createComment()

document.createComment方法生成一个新的注释节点，并返回该节点。

```node
var CommentNode = document.createComment(data);
```

document.createComment方法的参数是一个字符串，会成为注释节点的内容。

#### 5. document.createDocumentFragment()

document.createDocumentFragment方法生成一个空的文档片段对象（DocumentFragment实例）。

```node
var docFragment = document.createDocumentFragment();
```

DocumentFragment是一个存在于内存的 DOM 片段，不属于当前文档，常常用来生成一段较复杂的 DOM 结构，然后再插入当前文档。这样做的好处在于，因为DocumentFragment不属于当前文档，对它的任何改动，都不会引发网页的重新渲染，比直接修改当前文档的 DOM 有更好的性能表现。

```node
var docfrag = document.createDocumentFragment();

[1, 2, 3, 4].forEach(function (e) {
  var li = document.createElement('li');
  li.textContent = e;
  docfrag.appendChild(li);
});

var element  = document.getElementById('ul');
element.appendChild(docfrag);
```

上面代码中，文档片断docfrag包含四个<li>节点，这些子节点被一次性插入了当前文档。

#### 6. document.createEvent()

document.createEvent方法生成一个事件对象（Event实例），该对象可以被element.dispatchEvent方法使用，触发指定事件。

```node
var event = document.createEvent(type);
```

document.createEvent方法的参数是事件类型，比如UIEvents、MouseEvents、MutationEvents、HTMLEvents。

```node
var event = document.createEvent('Event');
event.initEvent('build', true, true);
document.addEventListener('build', function (e) {
  console.log(e.type); // "build"
}, false);
document.dispatchEvent(event);
```

上面代码新建了一个名为build的事件实例，然后触发该事件。

### 4. document.addEventListener()，document.removeEventListener()，document.dispatchEvent()

这三个方法用于处理document节点的事件。它们都继承自EventTarget接口，详细介绍参见《EventTarget 接口》一章。

```node
// 添加事件监听函数
document.addEventListener('click', listener, false);

// 移除事件监听函数
document.removeEventListener('click', listener, false);

// 触发事件
var event = new Event('click');
document.dispatchEvent(event);
```

### 5. document.hasFocus()

document.hasFocus方法返回一个布尔值，表示当前文档之中是否有元素被激活或获得焦点。

```node
var focused = document.hasFocus();
```

注意，有焦点的文档必定被激活（active），反之不成立，激活的文档未必有焦点。比如，用户点击按钮，从当前窗口跳出一个新窗口，该新窗口就是激活的，但是不拥有焦点。

### 6. document.adoptNode()，document.importNode()

document.adoptNode方法将某个节点及其子节点，从原来所在的文档或DocumentFragment里面移除，归属当前document对象，返回插入后的新节点。插入的节点对象的ownerDocument属性，会变成当前的document对象，而parentNode属性是null。

```node
var node = document.adoptNode(externalNode);
document.appendChild(node);
```

注意，document.adoptNode方法只是改变了节点的归属，并没有将这个节点插入新的文档树。所以，还要再用appendChild方法或insertBefore方法，将新节点插入当前文档树。

document.importNode方法则是从原来所在的文档或DocumentFragment里面，拷贝某个节点及其子节点，让它们归属当前document对象。拷贝的节点对象的ownerDocument属性，会变成当前的document对象，而parentNode属性是null。

```node
var node = document.importNode(externalNode, deep);
```

document.importNode方法的第一个参数是外部节点，第二个参数是一个布尔值，表示对外部节点是深拷贝还是浅拷贝，默认是浅拷贝（false）。虽然第二个参数是可选的，但是建议总是保留这个参数，并设为true。

注意，document.importNode方法只是拷贝外部节点，这时该节点的父节点是null。下一步还必须将这个节点插入当前文档树。

```node
var iframe = document.getElementsByTagName('iframe')[0];
var oldNode = iframe.contentWindow.document.getElementById('myNode');
var newNode = document.importNode(oldNode, true);
document.getElementById("container").appendChild(newNode);
```

上面代码从iframe窗口，拷贝一个指定节点myNode，插入当前文档。

### 7. document.createNodeIterator()

document.createNodeIterator方法返回一个子节点遍历器。

```node
var nodeIterator = document.createNodeIterator(
  document.body,
  NodeFilter.SHOW_ELEMENT
);
```

上面代码返回`<body>`元素子节点的遍历器。

document.createNodeIterator方法第一个参数为所要遍历的根节点，第二个参数为所要遍历的节点类型，这里指定为元素节点（NodeFilter.SHOW_ELEMENT）。几种主要的节点类型写法如下。

- 所有节点：NodeFilter.SHOW_ALL
- 元素节点：NodeFilter.SHOW_ELEMENT
- 文本节点：NodeFilter.SHOW_TEXT
- 评论节点：NodeFilter.SHOW_COMMENT

document.createNodeIterator方法返回一个“遍历器”对象（NodeFilter实例）。该实例的nextNode()方法和previousNode()方法，可以用来遍历所有子节点。

```node
var nodeIterator = document.createNodeIterator(document.body);
var pars = [];
var currentNode;

while (currentNode = nodeIterator.nextNode()) {
  pars.push(currentNode);
}
```

上面代码中，使用遍历器的nextNode方法，将根节点的所有子节点，依次读入一个数组。nextNode方法先返回遍历器的内部指针所在的节点，然后会将指针移向下一个节点。所有成员遍历完成后，返回null。previousNode方法则是先将指针移向上一个节点，然后返回该节点。

```node
var nodeIterator = document.createNodeIterator(
  document.body,
  NodeFilter.SHOW_ELEMENT
);

var currentNode = nodeIterator.nextNode();
var previousNode = nodeIterator.previousNode();

currentNode === previousNode // true
```

上面代码中，currentNode和previousNode都指向同一个的节点。

注意，遍历器返回的第一个节点，总是根节点。

```node
pars[0] === document.body // true
```

### 8. document.createTreeWalker()

document.createTreeWalker方法返回一个 DOM 的子树遍历器。它与document.createNodeIterator方法基本是类似的，区别在于它返回的是TreeWalker实例，后者返回的是NodeIterator实例。另外，它的第一个节点不是根节点。

document.createTreeWalker方法的第一个参数是所要遍历的根节点，第二个参数指定所要遍历的节点类型（与document.createNodeIterator方法的第二个参数相同）。

```node
var treeWalker = document.createTreeWalker(
  document.body,
  NodeFilter.SHOW_ELEMENT
);

var nodeList = [];

while(treeWalker.nextNode()) {
  nodeList.push(treeWalker.currentNode);
}
```

上面代码遍历`<body>`节点下属的所有元素节点，将它们插入nodeList数组。

### 9. document.execCommand()，document.queryCommandSupported()，document.queryCommandEnabled()

如果document.designMode属性设为on，那么整个文档用户可编辑；如果元素的contenteditable属性设为true，那么该元素可编辑。这两种情况下，可以使用document.execCommand()方法，改变内容的样式，比如document.execCommand('bold')会使得字体加粗。

```node
document.execCommand(command, showDefaultUI, input)
```

该方法接受三个参数。

- command：字符串，表示所要实施的样式。
- showDefaultUI：布尔值，表示是否要使用默认的用户界面，建议总是设为false。
- input：字符串，表示该样式的辅助内容，比如生成超级链接时，这个参数就是所要链接的网址。如果第二个参数设为true，那么浏览器会弹出提示框，要求用户在提示框输入该参数。但是，不是所有浏览器都支持这样做，为了兼容性，还是需要自己部署获取这个参数的方式。

```node
var url = window.prompt('请输入网址');

if (url) {
  document.execCommand('createlink', false, url);
}
```

上面代码中，先提示用户输入所要链接的网址，然后手动生成超级链接。注意，第二个参数是false，表示此时不需要自动弹出提示框。

document.execCommand()的返回值是一个布尔值。如果为false，表示这个方法无法生效。

这个方法大部分情况下，只对选中的内容生效。如果有多个内容可编辑区域，那么只对当前焦点所在的元素生效。

document.execCommand()方法可以执行的样式改变有很多种，下面是其中的一些：bold、insertLineBreak、selectAll、createLink、insertOrderedList、subscript、delete、insertUnorderedList、superscript、formatBlock、insertParagraph、undo、forwardDelete、insertText、unlink、insertImage、italic、unselect、insertHTML、redo。这些值都可以用作第一个参数，它们的含义不难从字面上看出来。

document.queryCommandEnabled()方法返回一个布尔值，表示浏览器是否允许使用这个方法。

```node
if (document.queryCommandEnabled('SelectAll')) {
  // ...
}
```

document.queryCommandSupported()方法返回一个布尔值，表示当前是否可用某种样式改变。比如，加粗只有存在文本选中时才可用，如果没有选中文本，就不可用。

```node
if (document.queryCommandSupported('SelectAll')) {
  // ...
}
``

document.queryCommandEnabled()方法返回一个布尔值
