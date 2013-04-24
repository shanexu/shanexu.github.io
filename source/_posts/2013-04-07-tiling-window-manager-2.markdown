---
layout: post
title: "Tiling Window Manager (2)"
date: 2013-04-07 22:46
comments: true
categories: [linux, "tiling window manager", xmonad, ruby, macruby]
---

<div class="begin-indent2em"></div>

OS X下有这么几款窗口管理器，好吧，严格意义上OS X没有什么窗口管理器这种东西，其实有的只是辅助性的工具，比如SizeUp能够用快捷键把窗口放到左半屏，上半屏，四分之一屏，移动窗口等等，然后收费，其实也是可以免费用的，只是过段时间会弹出对话框。然后，[Tyler](http://www.tylerwm.com/)，这货其实是很像XMonad的，尤其是看完了他的演示视频。但是
>Not currently compatible with Mountain Lion. We are working on an update.

好吧我没法用。

在搜索tiling window manager for os x的时候我在github上看到了[slate](https://github.com/jigish/slate)。slate的功能其实和SizeUp很相近。然而最近这个项目有了很大的突破，比如layout、javascript config file的引入，但是还是没有XMonad带给我体验，我仍旧需要记住有一大堆快捷键，然后用快捷键手动设置每一个窗口的大小。

于是我冒出了一个想法，要不我自己来写一个窗口管理器吧。于是我clone了slate代码下来仔细研读。而事实是，我不太会用Objective-C，我对OC的认识，也差不多仅限于差不多能读懂代码罢了。还好这个世界上有个macruby的东西

>MacRuby is an implementation of Ruby 1.9 directly on top of Mac OS X core technologies such as the Objective-C runtime and garbage collector, the LLVM compiler infrastructure and the Foundation and ICU frameworks. It is the goal of MacRuby to enable the creation of full-fledged Mac OS X applications which do not sacrifice performance in order to enjoy the benefits of using Ruby.

于是我写了这么一个没啥大用的脚本。

``` ruby
# -*- coding: utf-8 -*-
framework "AppKit"
framework "CoreGraphics"

require 'logger'

module RMonad
  self.instance_eval do
    @logger = Logger.new(STDOUT)
    def logger
      @logger
    end
  end

  class RMonadWorkspace
    attr_accessor :windows
    attr_accessor :layout
    def initialize
    end
  end

  class HLayout
    attr_accessor :frames
    def initialize(w,h)
      @w, @h=w,h
      @frames=[]
    end
    
    def swap_next(win)
      return if @frames.count <= 1
      index = @frames.index{|f| f.win.id == win.id}
      if index
        frame = @frames[index]
        frame_next = @frames[(index+1)%@frames.count]
        frame.win, frame_next.win = frame_next.win, frame.win
      end
    end

    def swap_prev(win)
      return if @frames.count <= 1
      index = @frames.index{|f| f.win.id == win.id}
      if index
        frame = @frames[index]
        frame_prev = @frames[(index-1)%@frames.count]
        frame.win, frame_prev.win = frame_prev.win, frame.win
      end
    end

    def swap_main(win)
      return if @frames.count <= 1
      index = @frames.index{|f| f.win.id == win.id}
      if index
        frame = @frames[index]
        frame_main = @frames[0]
        frame.win, frame_main.win = frame_main.win, frame.win
      end
    end

    def main
      @frames[0].win if @frames.count >= 1
    end
    
    def prev(win)
      index = @frames.index{|f| f.win.id == win.id}
      @frames[(index+1)%@frames.count].win if index
    end
    
    def next(win)
      index = @frames.index{|f| f.win.id == win.id}
      @frames[(index-1)%@frames.count].win if index
    end

    def <<(win)
      if @frames.empty?
        @frames << Frame.new(0,0,@w,@h).add(win)
      else
        frame1 = @frames[1]
        if frame1.nil?
          w = @frames[0].w == @w ? @w/2.0 : @w - @frames[0].w
          h = @h
          frame0 = @frames[0]
          frame0.w = @w-w
          frame0.adjust
        else
          w = frame1.w
          h = @h / (count).to_f
        end
        @frames << Frame.new(0,0, w, h).add_not_resize(win)
        @frames[1..-1].each_with_index do |f, i|
          f.x, f.y, f.h = w, h*i, h
          f.adjust
        end
      end
      @frames
    end

    def >>(win)
      index = @frames.index{|f| f.win.id == win.id}
      if index
        removed_frame = @frames.delete @frames[index]
        
        return if @frames.count == 0
        if @frames.count == 1
          @frames[0].resize(0,0,@w,@h)
        else
          @frames[0].resize(0,0,removed_frame.w, removed_frame.h) if index == 0
          w=@w-@frames[0].w
          h=@h/(@frames.count -1).to_f
          @frames[1..-1].each_with_index{|f, i| f.resize(w, h*i, w, h)}
        end
      end
    end

    def init_layout
      return if @frames.count == 0
      if @frames.count == 1
        @frames[0].resize(0,0,@w,@h)
      else
        @frames[0].resize(0,0,@w/2.0, @h)
        w,h = @w-@frames[0].w, @h/(@frames.count-1).to_f
        @frames[1..-1].each_with_index{|f, i| f.resize(w, h*i, w, h)}
      end
    end

    def count
      @frames.count
    end

    def contains?(win)
    end
  end

  class Frame
    attr_accessor :x,:y,:w,:h
    def initialize(x=0,y=0,w=0,h=0)
      @x,@y,@w,@h = x,y,w,h
    end
    def win
      @win
    end
    def win=(win)
      add(win)
    end
    def add_not_resize(win)
      @win = win
      self
    end
    def add(win)
      win.position={x:@x, y:@y}
      win.size = {w:w, h:h}
      @win = win
      self
    end
    def adjust
      win.position={x:@x, y:@y}
      win.size = {w:w, h:h}
      self
    end
    def resize(x,y,w,h)
      @x,@y,@w,@h = x,y,w,h
      win.position={x:@x, y:@y}
      win.size = {w:@w, h:@h}
      self
    end
  end

  class RMonadWindow

    def self.current_windows
      running_apps.inject([]) do |wins, app|
        puts app.localizedName
        ws = windows_of_app(app.processIdentifier)
        wins.concat ws.inject([]){|rws, w| rw = RMonadWindow.new(w); rws << rw if rw.title && !rw.title.empty?; rws} if ws
        wins
      end
    end

    def self.windows_of_app(pid)
      app=AXUIElementCreateApplication(pid)
      windows = Pointer.new_with_type("^{__CFArray}")
      return windows[0] if AXUIElementCopyAttributeValues(app, KAXWindowsAttribute, 0, 100, windows) == KAXErrorSuccess
    end

    def self.running_apps
      app_arr = NSWorkspace.sharedWorkspace.runningApplications
      app_arr.each_with_index{|a, i| puts "#{i}. #{a.localizedName}"}
    end
    
    def self.current_app
      current_app = NSRunningApplication.currentApplication
    end
    
    def initialize(axuelementref_window)
      @axuelementref_window = axuelementref_window
    end

    def title
      ptr = Pointer.new(:id)
      ptr[0] if AXUIElementCopyAttributeValue(@axuelementref_window, NSAccessibilityTitleAttribute, ptr)== KAXErrorSuccess
    end

    def is_main
      
    end

    def move(xy={})
      x=xy[:x]||0
      y=xy[:y]||0
      y+=22
      cgpoint = Pointer.new(CGPoint.type)
      cgpoint[0] = NSPoint.new(x,y)
      position = AXValueCreate(KAXValueCGPointType, cgpoint)
      if AXUIElementSetAttributeValue(@axuelementref_window, NSAccessibilityPositionAttribute, position) == KAXErrorSuccess
        true
      else
        Rmond.logger.error("#{axuelementref_window} could not move to position #{xy}")
      end
    end

    def position
      ptr = Pointer.new(:id)
      pos_ptr = Pointer.new(NSPoint.type)
      if AXUIElementCopyAttributeValue(@axuelementref_window, NSAccessibilityPositionAttribute, ptr) == KAXErrorSuccess and AXValueGetValue(ptr[0], KAXValueCGPointType, pos_ptr)
        {w:pos_ptr[0].x, h:pos_ptr[0].y}
      else
        Rmond.logger.error "#{axuelementref_window} could not get position"
      end
    end

    def position=xy
      move(xy)
    end

    def size
      ptr = Pointer.new(:id)
      size_ptr = Pointer.new(NSSize.type)
      if AXUIElementCopyAttributeValue(@axuelementref_window, NSAccessibilitySizeAttribute, ptr) == KAXErrorSuccess and AXValueGetValue(ptr[0], KAXValueCGSizeType, size_ptr)
        {w:size_ptr[0].width, h:size_ptr[0].height}
      else
        {w:0,h:0}
      end
    end

    def resize(wh)
      w=wh[:w]||0
      h=wh[:h]||0
      size_ptr = Pointer.new(NSSize.type)
      size_ptr[0] = NSSize.new(w, h)
      ptr_value = AXValueCreate(KAXValueCGSizeType, size_ptr)
      unless AXUIElementSetAttributeValue(@axuelementref_window, NSAccessibilitySizeAttribute, ptr_value) == KAXErrorSuccess
        Rmond.logger.error "#{@axuelementref_window} could not resize"
      end  
    end
    def size=wh
      resize({w:0,h:0})
      resize(wh)
    end
  end
end
```

然后找个终端运行下面的代码

``` ruby
RMonand::RMonadWindow::current_windows.inject(RMonad::HLayout.new(1280, 800-22)){|l, m| l << m; l}
```

就能把当前桌面上所有的窗口平铺开来。然后无奈的事情开始了。第一件事情，我如何能够得知当前的桌面是几号桌面。
``` ruby
CGWindowListCopyWindowInfo(KCGWindowListOptionOnScreenOnly | KCGWindowListExcludeDesktopElements, KCGNullWindowID).select{|w| !w['kCGWindowOwnerName'].empty? && w["kCGWindowLayer"]==0}
```
这条语句能够得到当前桌面上的所有可见窗口的列表，运行结果形如下面所示，其实也就是一个hash的数组
``` ruby
[{"kCGWindowLayer"=>0, "kCGWindowName"=>"macirb — macruby — 181×48", "kCGWindowMemoryUsage"=>5544180, "kCGWindowIsOnscreen"=>true, "kCGWindowSharingState"=>1, "kCGWindowOwnerPID"=>52112, "kCGWindowNumber"=>15366, "kCGWindowOwnerName"=>"终端", "kCGWindowStoreType"=>2, "kCGWindowBounds"=>{"Height"=>772.0, "X"=>0.0, "Width"=>1277.0, "Y"=>26.0}, "kCGWindowAlpha"=>1.0}, {"kCGWindowLayer"=>0, "kCGWindowName"=>"", "kCGWindowMemoryUsage"=>3933468, "kCGWindowIsOnscreen"=>true, "kCGWindowSharingState"=>1, "kCGWindowOwnerPID"=>90225, "kCGWindowNumber"=>7682, "kCGWindowOwnerName"=>"Emacs", "kCGWindowStoreType"=>2, "kCGWindowBounds"=>{"Height"=>764.0, "X"=>0.0, "Width"=>1274.0, "Y"=>26.0}, "kCGWindowAlpha"=>1.0}, {"kCGWindowLayer"=>0, "kCGWindowName"=>"Emacs@shane-macbook.local", "kCGWindowMemoryUsage"=>4018228, "kCGWindowIsOnscreen"=>true, "kCGWindowSharingState"=>1, "kCGWindowOwnerPID"=>90225, "kCGWindowNumber"=>7683, "kCGWindowOwnerName"=>"Emacs", "kCGWindowStoreType"=>2, "kCGWindowBounds"=>{"Height"=>764.0, "X"=>0.0, "Width"=>1274.0, "Y"=>26.0}, "kCGWindowAlpha"=>1.0}, {"kCGWindowLayer"=>0, "kCGWindowName"=>"Shane's Home - Vimperator", "kCGWindowMemoryUsage"=>5328948, "kCGWindowIsOnscreen"=>true, "kCGWindowSharingState"=>1, "kCGWindowOwnerPID"=>52228, "kCGWindowNumber"=>112, "kCGWindowOwnerName"=>"Firefox", "kCGWindowStoreType"=>2, "kCGWindowBounds"=>{"Height"=>770.0, "X"=>0.0, "Width"=>1280.0, "Y"=>26.0}, "kCGWindowAlpha"=>1.0}]
```
在10.8之前这个hash里会有一个名为“kCGWindowWorkspace”的键，但是现在已经取消了这个值，我想破脑袋google了一圈又一圈，也真没找到什么行之有效的方式获取当前的桌面号。

第二，怎么把window number和ACXUIElementRef关联起来。上面说过的CGWindowListCopyWindowInfo方法能把当前桌面上的窗口全都找出来，同时还能看到其中有个key为“kCGWindowNumber”，这便是窗口的id。然而我们现在需要对window进行move、resize这些操作，我现在知道的方法就是获取窗口AXUIElementRef，然而使用AXUIElementCopyAttributeValue通过application的reference获得窗口列表里却丝毫找不到关于WindowNumber的影子。好吧，难道我要比较两个对象的窗口位置和大小来匹配CGWindowListCopyWindowInfo和AXUIElementCopyAttributeValue产生的两组结果，真的好荒谬啊。

于是我放弃了，留下了前面那个莫名奇妙的例子。如果非要用的话也未尝不可。



