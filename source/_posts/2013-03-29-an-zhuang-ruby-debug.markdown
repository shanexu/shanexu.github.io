---
layout: post
title: "安装ruby-debug"
date: 2013-03-29 11:56
comments: true
categories: ruby
---

<div class="begin-indent2em"></div>
最近想在emacs里面调试ruby，搜到一篇博文说是安装ruby-debug和ruby-debug-extra，于是首先尝试安装ruby-debug

```
rvm use 1.9.3
gem search ruby-debug -r
gem install ruby-debug
```

然后找到[ruby-debug](http://rubyforge.org/docman/view.php/8883/10451/ruby-debug.html)的官方文档，然后自己胡乱写了个脚本开始对着文档调试，然后就是rdebug抛错。之后发现竟然还有一个ruby-debug19的包。然后又安装ruby-debug19。尝试运行rdebug，还是出错。没辙拿着错误信息直接上google，终于在[stackoverflow](http://stackoverflow.com/questions/8378277/cannot-use-ruby-debug19-with-1-9-3-p0)找到答案。貌似答案的出处在这里[http://blog.wyeworks.com/2011/11/1/ruby-1-9-3-and-ruby-debug/](http://blog.wyeworks.com/2011/11/1/ruby-1-9-3-and-ruby-debug/)
首先需要从[http://rubyforge.org/frs/?group_id=8883](http://rubyforge.org/frs/?group_id=8883)下载

1. linecache19-0.5.13.gem 
2. ruby_core_source-0.1.5.gem 
3. ruby-debug19-0.11.6.gem 
4. ruby-debug-base19-0.11.26.gem

然后用下面的命令安装：

```
which ruby
export RVM_SRC=/Users/shane/.rvm/src/ruby-1.9.3-p392
gem install archive-tar-minitar
gem install ruby_core_source-0.1.5.gem -- --with-ruby-include=$RVM_SRC
gem install linecache19-0.5.13.gem -- --with-ruby-include=$RVM_SRC
gem install ruby-debug-base19-0.11.26.gem -- --with-ruby-include=$RVM_SRC
gem install ruby-debug19-0.11.6.gem -- --with-ruby-include=$RVM_SRC
```

结果后来在安装rdebug for emacs的时候看到了一个gem， [debugger](https://github.com/cldwalker/debugger)，感觉这个包比较好。

>A fork of ruby-debug(19) that works on 1.9.2 and 1.9.3 and installs easily for rvm/rbenv rubies.

```
gem install debugger
```

然后在emacs里面用el-get安装rdebug就好了。

