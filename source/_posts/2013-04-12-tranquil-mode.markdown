---
layout: post
title: "tranquil-mode"
date: 2013-04-12 16:22
comments: true
categories: [emacs, tranquil]
---

<div class='begin-indent2em' filter='p:not(:has(a.fancybox :first-child))'></div>
前面几篇写tiling window manager的文章里提到了，xnomad这个在os x下极端好用的窗口管理器，这个窗口管理器是用tranquil这个语言写的。目前[tranquil](https://github.com/fjolnir/tranquil)这门语言还只是在github上的一个项目。

>Tranquil is a programming language built on top of LLVM & the Objective-C Runtime.
>It aims to provide a more expressive & easy to use way to write Mac and iOS Apps.

Tranquil是一门基于LLVM和Objective-C runtime的编程语言，其目的在与提供一种更具表现力的和方便的方法来编写Mac应用和IOS Apps。

作者在他的repository里只提供了vim的语法高亮插件，于是只能自己去写一个emacs的mode，于是通过看elisp手册，emacswiki以及阅读js.el和ruby-mode.el，加上不断的google，还有[\C-h f]终于写出了我自己的勉强还能用的[tranquil-mode](https://github.com/shanexu/tranquil-mode)。

```
(add-to-list 'load-path "~/path/to/tranquil-mode/")
(require 'tranquil-mode)
```

{% img http://106.187.36.166/shanexu.org/images/tranquil-mode.png 'tranquil-mode' 'tranquil-mode' %}
