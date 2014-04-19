---
layout: post
title: "萨特存在主义的自由和责任"
date: 2014-04-19 09:30
comments: true
categories: [ruby, 让·保罗·萨特, 萨特]
---

<div class="begin-indent2em"></div>
早上起来的时候，因为一些很难忍受的现实而写了一个脚本，希望能对自己今后的生活学习工作有所帮助。脚本很简单，寥寥几行希望能慰藉我脆弱的心灵。希望有朝一日能够不再需要依靠这个脚本。


``` ruby
# -*- coding: utf-8 -*-

f1 = File.open("china_route.sh", "w")
f2 = File.open("china_route_delete.sh", "w")
gate = "192.168.8.1"
`curl http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest | grep "CN|ipv4"`.each_line do |l|
  _, _, _, ip, length,_ = l.split("|")
  ipl = "#{ip}/#{(Math.log(length.to_i)/Math.log(2)).to_i}"
  f1.puts "route add #{ipl} #{gate}"
  f2.puts "route delete #{ipl} #{gate}"
end

```
大概就这样了，我看DATE·A·LIVE去了。
