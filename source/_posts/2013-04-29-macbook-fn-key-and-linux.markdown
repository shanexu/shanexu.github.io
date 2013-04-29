---
layout: post
title: "macbook fn key and Linux"
date: 2013-04-29 15:01
comments: true
categories: [MacBook, Linux]
---

<div class="begin-indent2em"></div>
其实我呢，很不喜欢os x的function key的默认设置，也就是说当我按下F1的时候实际上降低亮度，而且我会经常用到F12，呼出firebug的快捷键，所以在os x下我会改掉function key的默认行为。

然后是新问题，虽然这个问题在刚买了MacBook的那个星期里就碰到了，并且在读了archlinux wiki之后很轻松地解决了，但是直到现在我才想把这个过程写下来。

我的在MacBook上装好LInux之后遇到了些许问题。比如，如何引导Linux启动，然后就是如何设置那个莫名奇妙的function key。

其实要配置到我需要的模式，只需要执行一句命令就可以了

```
echo 2 | sudo tee > /sys/module/hid_apple/parameters/fnmode
```

之后就是开机自动执行这个命令，在早些版本的archlinux可以将这些命令放在*/etc/rc.local*中，然而现在的archlinux使用systemd取代了原先的sysvinit。

{% blockquote %}
systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic. It can work as a drop-in replacement for sysvinit.
{% endblockquote %}

带来便利的同时必然也会带来不便。以前也许只要rc.conf或者rc.local中添加几行代码的事情，现在可能就要写一大段代码然后再加上执行几个命令，有时候，不知道这样的做法是在进步还是一种退步。于是在arch wiki上看了systemd的文档，然后试着去写了一个设置function key的系统服务。

在目录/usr/lib/systemd/system/下创建一个名为fnkey.service的文件，填入以下内容。

```
[Unit]
Description=Set MacBook Fn key
After=getty.target

[Service]
Type=oneshot
ExecStart=/usr/bin/bash -c "echo 2 > /sys/module/hid_apple/parameters/fnmode"

[Install]
WantedBy=multi-user.target
```

然后enable/start fnkey.service。

```
sudo systemctl enable fnkey
#sudo systemctl start fnkey
```


