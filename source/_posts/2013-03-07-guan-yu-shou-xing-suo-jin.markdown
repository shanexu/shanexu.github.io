---
layout: post
title: "关于首行缩进"
date: 2013-03-07 10:24
comments: true
categories: [javascript, css]
---

<div class="begin-indent2em"></div>
最近在把以前写的小说搬到了这个地方。有件事情让我有些苦恼，原来没有首行缩进的中文文本是非常难看的东西。我想最直接方式就是给每个p标签加上一段style。
``` html
<p style="text-indent: 2em;">
    接下来的日子与琳琅失去联络。于是只好用回忆来消磨时光。
    可回忆却像是嚼久了的口香糖，越嚼越硬，最后竟硌伤了我自己。
    琳琅终究还是从未喜欢过我。
</p>
```
然后把这段style放在css中。
``` css
.indent2em {
    text-indent: 2em;
}
```
然而octopress的post文档是从markdown文件生成的。markdown文本生成html时候并不能给生成的tag加上class等属性，要么使用html混排的方式把class写进tag中，但是这样一来就完全失去了使用markdown编写post的优势了，还不如整个去写一个html文档算了。所以要么把格式直接写在p标签里。比如这样，修改[slash](https://github.com/tommy351/Octopress-Theme-Slash)主题的_post.scss文件。
``` scss
.post{
    h2.title, .entry-content{
        p{
            text-indent: 2em;
        }
    }
}
```
但是这样势必造成所有的p标签都首行缩进，而且也只有对p标签起到作用。其实归根到底，我想要的效果就是只在需要缩进的地方缩进。既然markdown没有提供加入class的办法，那就只能靠javascript。
``` javascript
$(function(){
    $('a[href="the-path-of-blog"]:eq(0)')
        .parent().next().find('>p')
        .addClass('indent2em').length
        || $('.entry-content > p').addClass('indent2em');
});
```
把这段放在*&lt;!&minus;&minus; more &minus;&minus;&gt;*之前，这样折叠之前的内容和折叠之后的内容都会有首行缩进的样式。这样还是有问题，不仅每次都要把这么一段代码放到文档中，同时段落的缩进还是很难控制，当然可以通过修改javascript代码来做到。还有一个麻烦的地方是每次都要把当前的博文的地址放到*“the-path-of-blog”*中去。好吧既然每张需要首行缩进的页面都要加上一段javascript代码那么干脆把代码放到单独的js文件去吧。
首先我们加三个css类。
``` scss
.indent2em{
    text-indent: 2em;
}
.begin-indent2em{
    display: none;
}
.end-indent2em{
    display: none;
}
```
第一个自不必说了，*begin-indent2em*和*end-indent2em*则用来分别标出开始和结束首行缩进的位置。这样就能在行文中自如的控制缩进格式了。

接着加入一段javascript。
``` javascript
(function($){
    var addIndent2em = function(){
        $('.begin-indent2em').each(function(index, ele){
            var qele = $(ele);
            qele.nextUntil('.end-indent2em', qele.attr('filter')||'p').addClass('indent2em');
        });
    };
    addIndent2em();
})(jQuery);
```

来看一下怎么用吧。下面这段文字来自我非常喜欢的一本书[芒果街上的小屋](http://book.douban.com/subject/1794620/)，这是*头发（hair）*这一章的中英文对照。中文需要首行缩进，而英文不要。于是在markdown代码中加入了两个不占用空间的*div*风别标识需要开始和结束缩进的位置。而*begin div*的*filter*属性中可以加入过滤选择器，默认的时候只选择*p*标签。这样就能较为自由的控制缩进格式了。而实际效果也还可以，除了某些时候。

``` html
<div class='begin-indent2em' filter='p, block'></div>
我们家里每个人的头发都不一样。爸爸的头发像扫把，根根直立往上插。而我，我的投放挺懒惰。它从来不听发夹和发带的话。卡洛斯的头发又直又厚。他不用梳头。蕾妮的头发滑滑的——会从你手里溜走。还有奇奇，他最小，茸茸的头发像毛皮。
 
只有妈妈的头发，妈妈的头发，好像一朵朵小小的玫瑰花结，一枚枚小小的糖果圈儿，圈都那么拳曲，那么漂亮，因为她成天给它们上发卷。把鼻子伸进去闻一闻吧，当她搂着你时。当她搂着你时，你觉得那么安全，闻到的气味那么香甜。是那种待烤的面包的暖暖的香味，是那种她给你让出一角被窝时，和着体温散发的芬芳。你睡在她的身旁，外面下着雨，爸爸打着鼾。哦，鼾声、雨声，还有妈妈吗那闻起来像面包的头发。
 
<div class='end-indent2em'></div>
Everybody in our family has different hair. My Papa’s hair is like a broom, all up in the air. And me, my hair is lazy. It never obeys barrettes or bands. Carlos’s hair is thick and straight. He doesn’t need to comb it. Nenny’s hair is slippery -- slides out of  your hand. And Kiki, who is the youngest, has hair like fur.
 
But my mother’s hair, my mother’s hair, like little rosettes, like little candy circles all curly and pretty because she pinned it in pincurls all day, sweet to put your nose into when she is holding you, holding you and you feel safe, is the warm smell of bread before you bake it, is the smell when she makes room for you on her side of the bed still warm with her skin, and you sleep near her, the rain outside falling and Papa snoring. The snoring, the rain outside falling and Papa snoring. The snoring, the rain, and Mama’s hair that smells like bread.
```

