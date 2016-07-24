---
author: yangzy666
comments: true
date: 2013-07-14 14:52:23+00:00
layout: post
slug: '%e9%80%9a%e8%bf%87vnc%e8%bf%9c%e7%a8%8b%e8%bf%9e%e6%8e%a5linux%e6%a1%8c%e9%9d%a2'
title: 通过VNC远程连接linux桌面
wordpress_id: 691
categories:
- linux
---
{% include JB/setup %}

	 对于远程连接windows系统的桌面，我们并不陌生，而对于linux桌面的远程连接，可能会相对较少，我们可以通过VNC或xmanager来实现。此文主要简单地讲下vnc连接redhat Linux桌面。

	一、vnc基本原理

	 vnc由服务端vnc server及客户端vnc viewer组成。在linux系统中安装vnc server并启动（如果是第一次启动，会要求输入密码），然后在客户端安装vnc viewer或者在浏览器中输入服务器IP及端口即可访问linux桌面，如10.13.1.35:5801。

	二、检查linux系统的vnc服务器是否安装并启动

```[root@myserver ~]# whereis vncvnc: /usr/share/vnc
```


	  


这说明vnc在本机已安装vnc server，否则需要自行下载安装：```yum install vnc-server
```


	三、启动vnc server

	 第一次启动时会要求创建密码，此密码用于客户端连接时使用。

	  


```[root@myserver ~]# vncserverYou will require a password to access your desktops.Password:Verify:New 'myserver:1 (root)' desktop is myserver:1Creating default startup script /root/.vnc/xstartupStarting applications specified in /root/.vnc/xstartupLog file is /root/.vnc/myserver:1.log
```
同时还会在当前用户目录下生成vnc相应配置文件，如果要修改桌面，比如使用kde桌面，就将**gnome-session &**注释掉，并加一行**startkde**，如下

	  


```[root@myserver ~]# vim ./.vnc/xstartup#!/bin/sh# Uncomment the following two lines for normal desktop:# unset SESSION_MANAGER# exec /etc/X11/xinit/xinitrc[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresourcesxsetroot -solid greyvncconfig -iconic &xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &#twm &#gnome-session &startkde
```


	  


	四、客户端访问

	最简单的方式就是通过浏览器访问:

	![](uploads/2013/07/20130715000109_37456.png)

	此种方式需要客户端安装有java运行环境，或者你也可以直接下载一个独立客户端，下载地址：[http://www.realvnc.com/download/viewer/](http://www.realvnc.com/download/viewer/)

	连接成功后，即可看到远程linux桌面：

	![](uploads/2013/07/20130715001232_24068.png)

	  

