---
layout: post
title: "Tiling Window Manager (3)"
date: 2013-04-08 13:20
comments: true
categories: [xmonad, osxmonad, "tiling window manager"]
---

<div class='begin-indent2em' filter='p:not(:has(a.fancybox :first-child))'></div>

在我对在os x下使用平铺式窗口管理器差不多快绝望的时候，我在github上竟然发现[osxmonad](https://github.com/xmonad/osxmonad.git)这个项目。来我们重温那句话。

>I would die for XMonad working with native OSX windows. Interested in any answers beyond the crappy things I've seen already.

后来看了下，这个comment是在2011年的时候发布的，而osxmonad则开始于9个月前。好吧不知道这个老兄现在知不知道这个东西的存在。好吧现在来说说osxmonad的安装方法吧。

```
git clone git://github.com/xmonad/osxmonad.git
darcs get http://code.haskell.org/xmonad
cd xmonad
darcs apply ../osxmonad/xmonad.patch
cabal install "X11 >=1.5 && <1.7"
cabal configure
cabal install
cd ../osxmonad
cabal configure
cabal install
```

以上是github上提供的安装方法。作者是对xmonad-1.0.1版本进行了补丁，而到目前为止，xmonad已经发布了1.1版本，所以作者的path无法在1.1版本的xmonad上上使用。然后看了看path文件，大概明白了补丁的意思。于是在XMonad/Core.hs的源代码的465行加上"-framework","Cocoa"编译选项，同时修改osxmonad/osxmonad.cabal的24行将xmonad的版本号改成0.11，就能按照上面的步骤编译安装了，当然ghc要从haskell的官网上下载haskel platform。

然后编写xmonad的配置文件~/.xmonad/xmonad.hs

``` haskell
import XMonad
import OSXMonad.Core

main = osxmonad defaultConfig {
         modMask = mod1Mask .|. mod4Mask,
         keys = osxKeys
       }
```

最后运行xmonad。


```
~/Library/Haskell/bin/xmonad
```

于是我终于在os x下用上了好久没有（其实我有时候会偷跑到linux下去爽几下）使用的XMonad。当然osxmonad还远远不能说完美，比如反应滞后，窗口焦点只能在自己的机制下维护。但是，够用了。

{% img http://106.187.36.166/shanexu.org/images/tiling-3.png 'Mountain Lion with osxmonad' 'Mountain Lion with osxmonad' %}

