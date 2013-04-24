---
layout: post
title: "emacs-mac-port"
date: 2013-03-06 10:47
comments: true
categories: emacs
---

<div class='begin-indent2em'></div>
把工作环境迁移到OS X下面已经是几个月前的事情了，说实话一开始真的很难适应奇怪的OS X系统。比如奇怪的窗口管理器行为，没有双击标题栏最大化，即便是点击了窗口的最大化按钮（我现在还是不知道这个按钮的确是叫这个名字的，反正就是那个绿色的按钮）的行为也是非常的暧昧，还有Dock上的图标竟然不具备最小化窗口的功能（Yes, I'm a long-term PC user.）。

况且在此之前我一直用着xmonad和shellshape这样的平铺窗口管理器。如今OS X下还要手动调节窗口大小，这诚然是梦魇啊。后来我安装了SizeUp之后我所有的窗口基本上就只有两个模式了——普通大小和最大化。然后用Command-Tab切换窗口。也就将就着用了。其实在我妥协之前我还在我的mac上装了Archlinux但是最后还是放弃了。我发现我已经懒于配置一个系统了。

切入正题吧。工作环境切换到OS X之后还有一些比较不习惯的事情就是emacs，尝试的装了homebrew和macports下的emacs，但是总有一些不太对的地方。最后终于在某一天查看emacs的mailing list的时候发现了[emacs mac port](https://github.com/railwaycat/emacs-mac-port) by Yamamoto Mitsuharu。以下是我的安装过程。

```
git clone https://github.com/railwaycat/emacs-mac-port.git
cd emacs-mac-port
cp mac/Emacs.app/Contents/Resources/Emacs.icns.official mac/Emacs.app/Contents/Resources/Emacs.icns #我比较喜欢原版的图标
./configure --with-mac --enable-mac-app --prefix=/opt/local
make
sudo make install
```
