---
title: 我使用的前端插件
date: 2017/2/28 11:16:25
tags:
  - 前端
  - 插件推荐
---
最近的项目中使用了很多第三方插件，在此简单记录一下，本篇文章会长期更新。

有必要将一些优秀的插件推荐给大家，先挖个坑，以后有时间会找其中比较优秀的插件写一写深入解析的文章。

大家比较熟知的框架和插件，比如jQuery、bootstrap在此略过。

##### saveSvgAsPng
github:[saveSvgAsPng](https://github.com/exupero/saveSvgAsPng)

saveSvgAsPng可以很方便的将网页中的**&lt;svg>**标签另存为**.png**格式的图片。saveSvgAsPng的原理是将svg的内容绘制到**canvas**中，让后将**canvas**的dataUrl绑定到一个**&lt;a>**标签的href属性中，触发**&lt;a>**标签的的click事件下载图片。

``` js
// 使用方法：
saveSvgAsPng(document.getElementById("diagram"), "diagram.png");
```

***

##### TableExport
::TODO

##### select2

##### toastr

##### bootstrap-tasinput

##### fontawesome

##### introjs
