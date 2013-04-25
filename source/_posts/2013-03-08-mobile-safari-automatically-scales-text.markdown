---
layout: post
title: "Mobile Safari Automatically Scales Text"
date: 2013-03-08 22:13
comments: true
categories: css
---

<div class='begin-indent2em' filter='p:not(:has(a.fancybox :first-child))'></div>

今天用iPhone浏览昨天写的[*关于首行缩进*](/blog/2013/03/07/guan-yu-shou-xing-suo-jin/)的日志的时候发现代码引用部分字体出现莫名其妙的放大现象，导致效果很不好。以为是最近老是换iPhone的字体所致，于是就打开了xcode的iPhone模拟器，一来可以看看模拟器上是不是出现这样的现象，二来也可以顺便调戏一下模拟器上的Mobile Safari。

结果模拟器上的Mobile Safari也出现了这样的现象。于是打开Safari调戏Mobile Safari。发现从code标签开始到最深处的span之前的那一层的font-size的计算值都是好的，直到最后一层的时候就莫名其妙的变大了。即便我在最后一层的span上设置了font-size的大小也无济于事，总之，折腾了半天，也没有个结果。无奈之下只能求助于google。

{% img http://fileserver.cloudfoundry.com/images/mobile_safari-1.png '#1' 'Mobile Safari automatically scales text' %}

<!-- more -->
Mobile Safari（也包括Android下的Chrome，FireFox，以及IE Mobile）在默认状态下会自动调整字体的大小以满足可读性的需求。如果把*\-webkit\-text\-size\-adjust（-moz-text-size-adjust 或者 -ms-text-size-adjust）*设置成*100%*或者*none*，Mobile Safari就不会自动缩放文本。于是我在*custom/_layout.scss*里加段补丁。

``` scss
body{
    -webkit-text-size-adjust: 100%;
    -moz-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
}

@media only screen and (min-width:768px) {
    body{
        -webkit-text-size-adjust: auto;
        -moz-text-size-adjust: auto;
        -ms-text-size-adjust: auto;
    }
}
```
重新用iPhone载入页面，原来参差不齐的代码字体就变得整齐了。

{% img http://fileserver.cloudfoundry.com/images/mobile_safari-2.png '#2' 'set -webkit-text-size-adjus to 100%' %}


