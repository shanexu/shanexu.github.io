---
layout: post
title: "Tiling Window Manager (1)"
date: 2013-04-07 01:19
comments: true
categories: [xmonad, "tiling window manager", linux]
---

<div class='begin-indent2em' filter='p:not(:has(a.fancybox :first-child))'></div>

>I would die for XMonad working with native OSX windows. Interested in any answers beyond the crappy things I've seen already.

写这篇文章是因为实在对os x的窗口管理实在已经吐槽无力了。

首先要解释一下什么是tiling window manager。Tiline Window Manager，也就是平铺式窗口管理器。平铺式窗口管理器区别于普通的窗口管理器，主要体现在平铺两个字。平铺是窗口管理器其主要目的是将屏幕空间分割成互不重合的几个区块。这样的特性能够让人同时关注多个窗口，同时还能尽可能地利用屏幕空间。同时省去了用鼠标调节窗口大小的麻烦，提高工作效率。其实大部分人还是不需要平铺式窗口管理器的。

第一次接触平铺式窗口管理器是在。。。好吧忘了什么时候的事情了。应该是在linuxsir上看到的一篇文章吧。那时候其实很无聊，每天几乎在摆弄linux的桌面环境，从gnome换到kde，再从kde换到xfce，然后用openbox，接着捣鼓fluxbox，然后用了一会儿e17（e17真的很漂亮，但是不好用），然后在fvwm上坚持过一段时间。然后就在好像没什么可以玩的时候，终于读到了那篇介绍平铺是窗口管理器的文章。

好吧，让我回忆一下，最早接触的平铺是窗口管理器是哪一个吧。
[awesome](http://awesome.naquadah.org/)，应该就是他了。其实从名字来看就知道他有多diao了。awesome其实很省心，默认配置自带一个panel，然后还有各种小插件（小插件什么的我几乎一次都没用过），然后官网上还有几个漂亮的主题，让装13的门槛迅速降低数个等级，同时配置文件又是用lua写的，在使用窗口管理器的同时又能学习一门新语言真是双赢啊。

{% img http://fileserver.cfapps.io/images/tiling-1.png '#1' 'ubuntu 13.04 with awesome' %}

出于某些原因，我没有长期使用下去，可能当时没有习惯auto tiling window manager吧，打开一个窗口，自动平铺到屏幕上，感觉自己没法控制。接着是[Musca](http://aerosuidae.net/musca.html)，和awesome不一样Musca是一个手动平铺式窗口管理器。Musca让用户手动将screen切分成不同的frames，然后把应用程序手动放到一个个frame里面。Musca没有提供任何panel、tab之类的控件，如果非要这些东西不可的话，可以拉一个xfce的panel，或者tint2之类的panel。我那时候的配置应该是Archlinux+ xfce4-panel + musca + dmenu，用了好久好久，那时候做操作系统实验的时候也是这个组合吧。

linux的rm -rf *之类的命令其实是非常危险的，尤其是在rm之前加sudo，然后想都没想直接输入密码。好吧那时候我/etc目录下执行了这个命令，反应过来的时候已经为时已晚。然后马上爬到校内论坛上问大家怎么补救，结果得到的答案便是“重装吧”。之后重装的时候也没有保存home里的配置文件。之后也懒得重写配置文件。

这之后玩的fvwm，其实比较喜欢fvwm那只小猫。用了[box-look](http://box-look.org)上的[Lethe](http://box-look.org/content/show.php/Lethe?content=91022)这个配置。是个非常清新的配置。

其实我至今都很喜欢kde3.5。kde总是给我很笨重的感觉，尤其是kde4，但是archlinux上有一个非常好的Kdemod，这是经过kde fans的优化的kde3.5桌面环境。这真的是一个值得拥有的桌面环境。可是好景不长，kde4的不断冲击，最后3.5的kdemod终于不再更新了。后来archlinux也衍生出了一个默认（其实只支持）KDE4桌面环境的发行版[Chakra](http://www.chakra-project.org)。其实这是KDE狂人搞出来的发行版，他把所有的gtk依赖去除的干干净净。对于像Emacs、Firefox这样的无法替代的软件则使用Bundle系统mount到系统上。如果非常喜欢KDE可以尝试一下这个发行版。

跑题了。好吧我忘了到底在什么情况下我遇到了我的本命，[XMonad](http://xmonad.org)，努力回忆下的话还是有点印象的。那时候有货吐槽各种窗口管理器，其中也包括了XMonad，为了装XMonad这个窗口管理器需要安装好几百M的ghc编译器，然后反吐槽的人就说，编译完了就可以把ghc删掉了。然后抱着试试看的心态安装了XMonad，XMonad还有个特点就是可以和现成的桌面环境结合，比如他能够替代xfce、gnome、kde的窗口管理器。同时还提供了在这三种桌面环境上默认配置，非常容易上手。同时XMonad的配置文件使用了Haskell语言进行编写，haskell是一门函数式编程语言，阅读起来也不是那么难。看久了也渐渐喜欢上这么语言，有时间应该去学学。XMonad用了好久好久。在KDE下用，在gnome下用，最后固定在了xfce4上了。

[subtile](http://subtile.subforge.org)是一个小插曲，那段时间在学习ruby，subtile是用ruby写的手动平铺式窗口管理器。基本上已经没有什么印象了。

时代在进步，终于有一天gnome 3带着他的gnome-shell来了。一开始gnome-shell，只是个花瓶，然后我发现我竟然可以用javascript写插件了，于是开始拥抱gnome-shell。想起那时候为了让right super键能够触发overlay view，挖到Mutter(gnome 3在gnome-shell的状态下的窗口管理器)的源代码里面。那段时间真是幸福。写到这里的时候突然想起漏了点东西，提到XMonad和gnmoe的时候还有一个不得不提的项目，就是[Bluetile](http://www.bluetile.org/)，bluetile是一个将gnome和XMonad结合在一起的项目。自从gnome 3出来以后，这个项目也停滞不前了。其实gnome 3在fallback模式下可以用XMonad替代默认窗口管理器。但是实在是，gnome 3的fallback模式真的很难受，gnome 3的可配置度也真的一直在向KDE4看齐。难道在没有安装gnome-tweak-tools的时候，只能用gconftool-2进行配置吗？好在gnome-shell用户做了一个平铺窗口管理插件，[shellshape](http://gfxmonk.net/shellshape/)，shellshape is inspired by bluetile。gnome-shell去除了在panel上的task manager，使用overlay view或者alt+tab切换应用程序。其实gnome-shell还有一个更惊人的改动，gnome-shell去除了窗口的最大化和最小化按钮，窗口上就只剩下一个孤零零的关闭按钮。其实他在学一些移动设备吧，每次都只有一个最大化的窗口，还好双击窗口的行为没有变。shellshape的出现，让我坚持使用gnome-shell了一段很长时间。

提到KDE4，其实KDE组是个很用心做软件的组。所有的kde软件都是union在一起的，比如dolphin中打开konsole，好吧，好久不用了，有些忘记了。而且kwin中加入平铺模式。只是kde有些让人无法满意的地方，比如，firefox无法替代，emacs无法替代，而这类gtk的软件真的在kde下很难看。无法忍受。我是一个对这种事情非常吹毛求疵的人。

