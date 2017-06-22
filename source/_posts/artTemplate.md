---
title: JS模板引擎 ：ArtTemplate
date: 2017-06-12 09:55:04
tags: [javascript]
---

# JS模板引擎 ：ArtTemplate
## 为什么需要用到模板引擎
我们在做前端开发的时候，有时候经常需要根据后端返回的json数据，然后来生成html，再显示到页面中去。

例如这样子：
```
var data = [
    {text: "测试一"},
    {text: "测试二"},
    {text: "测试三"},
    {text: "测试四"}
];
function generateList(data) {
    var listHtml = "";
    listHtml += "<ul>";
    for (var i = 0, len = data.length; i < len; i++) {
        listHtml += "<li>";
        listHtml += data.text;
        listHtml += "</li>";
    }
    listHtml += "</ul>";
    return listHtml;
}
```
<!--more--> 
但是，这种通过字符串拼接的方式，比较简单的还好，如果结构比较复杂，拼接的时候还需要注意引号之间的嵌套，这样的代码维护起来比较困难。

一旦需求发生变化，这里修改起来也是很麻烦。所以我们需要模板引擎来改善这种情况。

例如上面的例子，如果使用模板引擎则可以是这样子：
```
var data = {
    list:[
        {text: "测试一"},
        {text: "测试二"},
        {text: "测试三"},
        {text: "测试四"}
    ]
};
<script id="test" type="text/html">
    <ul>
        <% for (var i = 0; i < list.length; i ++) { %>
        <li><%= list[i].text %></li>
        <% } %>
    </ul>
</script>
```
不知道你有没有感觉简单一点呢，反正我是感觉更清晰明了一点。

## artTemplate的介绍
artTemplate 是新一代 javascript 模板引擎，它采用预编译方式让性能有了质的飞跃，并且充分利用 javascript 引擎特性，使得其性能无论在前端还是后端都有极其出色的表现。在 chrome 下渲染效率测试中分别是知名引擎 Mustache 与 micro tmpl 的 25 、 32 倍。

除了性能优势外，调试功能也值得一提。模板调试器可以精确定位到引发渲染错误的模板语句，解决了编写模板过程中无法调试的痛苦，让开发变得高效，也避免了因为单个模板出错导致整个应用崩溃的情况发生。

artTemplate 这一切都在 1.7kb(gzip) 中实现！

这是artTemplate的官网，使用方法相信有一定js基础的，看了文档之后都能够使用。这里就不详细介绍了
[官网](https://github.com/aui/art-template)

## artTemplate 模板引擎的基本原理
模板引擎其实做的就是两件事。
1. 根据一定的规则，解析我们所定义的模板
这里，我们将模板定义在script标签中，然后，当我们使用到某个模板的时候，引擎会根据我们提供的ID，解析相应的模板，此时会返回一个渲染函数。（为了性能，还会将这个渲染函数缓存起来）
```
(function($data,$filename) {
    'use strict';
    var i=$data.i,list=$data.list,$out='';$out+='<ul>\n';
    for (var i = 0; i < list.length; i ++) {
        $out+='\n        <li>';
        $out+= list[i].text;
        $out+='</li>\n';
    }
    $out+='\n</ul>';
    return new String($out);
})
```
上述代码，我已经删除了一些不必要的信息，解析模板之后，会返回一个这样的渲染函数。也就是说，其实模板引擎就是将我们平时用的字符串拼接的事情给做了。
至于，引擎是如何解析的，在下一篇我会详细介绍
2. 根据数据以及模板生成html（其实背后也是用的字符串拼接）
这里，会根据用户所传的数据，然后调用上一步返回的渲染函数。得到我们想要的结果。
