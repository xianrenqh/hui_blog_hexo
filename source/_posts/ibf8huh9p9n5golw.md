---
title: 4种方法替换JavaScript里所有出现的字符串
urlname: ibf8huh9p9n5golw
date: '2023-08-07 01:43:46 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_author: 小灰灰
copyright_url:
cover:
---

在 JavaScript 代码中出现这个字符串：

```
"Test abc test test abc test test test abc test test abc"
```

比如直接使用 replace 替换的方法，如下：

```
str = str.replace('abc', '');
```

似乎只删除了 abc 上面字符串中第一次出现的。我怎样才能替换它的所有出现？下面介绍 4 种替换所有出现字符串的方法。

# 方式一、使用 replace 加正则

必须启用正则表达式上的全局标志，才能使 replace()方法替换模式出现的所有内容，我们可以这样做：

1. 在正则表达式文字中，将 g 附加到标志部分：/abc/g。

2. 对于正则表达式构造函数，使用 flags 参数:new RegExp(' abc '， 'g')

代码如下：

```
str = str.replace(/abc/g, '');
```

或者：

```
var find = 'abc';var re = new RegExp(find, 'g');str = str.replace(re, '');
```

可以通过封装方法，进一步简化它。

```
function replaceAll(str, find, replace) {  return str.replace(new RegExp(find, 'g'), replace);}
```

注意：正则表达式包含特殊（元）字符，因此在 find 上面的函数中盲目传递参数而不对其进行预处理以转义这些字符是危险的。
因此，为了使上述 replaceAll()功能更安全，如果您还包含以下内容，则可以将其修改为以下内容 escapeRegExp：

```
function escapeRegExp(string) {  return string.replace(/[.*+\-?^${}()|[\]\\]/g, '\\$&'); // $& means the whole matched string}
```

```
function replaceAll(str, find, replace) {  return str.replace(new RegExp(escapeRegExp(find), 'g'), replace);}
```

# 方式二、replaceAll() 方法

新的提案 String.prototype.replaceAll()(在第 3 阶段)将 replaceAll()方法引入到 JavaScript 的字符串中。
replaceAll(search, replaceWith)字符串方法用 replaceWith 替换所有的 search 字符串，没有任何变通方法。

```
let result = "1 abc 2 abc 3".replaceAll("abc", "xyz");// `result` is "1 xyz 2 xyz 3"
```

但是首先检查我可以使用或其他兼容性表，以确保您的目标浏览器首先添加了对它的支持。
replaceAll 的兼容实现：

```
String.prototype.replaceAll = String.prototype.replaceAll || function(string, replaced) {  return this.replace(new RegExp(string, 'g'), replaced);};
```

# 方式三、使用 split 和 join 的方法

不使用任何正则表达式的最简单方法是 split 和 join，这种方法主要包含二个阶段：

1. 使用 split 方法，根据指定的字符将字符串分成多个部分。
2. 然后使用 join 方法将分割的多个部分连接在一直，并在它们之间插入指定的字符。

作为简单文字字符串的正则表达式的替代方案，您可以使用：

```
str = "Test abc test test abc test...".split("abc").join("");
```

一般模式是

```
str.split(search).join(replacement)
```

# 方法四、利用循环

基于 while 循环的解决方案很慢，通常对小字符串慢约 4 倍，对长字符串慢约 3000 倍。如下：

```
while(str.includes("abc")){    str = str.replace("abc", "replaced text");}
```

#
