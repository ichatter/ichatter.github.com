---
author: yangzy666
comments: true
date: 2013-08-05 07:27:56+00:00
layout: post
slug: linux%e6%b7%bb%e5%8a%a0%e5%bc%80%e6%9c%ba%e5%90%af%e5%8a%a8%e6%9c%8d%e5%8a%a1
title: Linux添加开机启动服务
wordpress_id: 702
categories:
- linux
---
{% include JB/setup %}

	 上周生产环境不能访问，客户那边将服务器重启后，站点管理系统（基于tomcat）不能自动开机重启，但是apache却成功启动了，不知何故。

	 我在生产环境中对这两个服务启动项的配置是基于/etc/rc.d/rc.local文件：

```[root@IBMSrv2 ~]# cat /etc/rc.d/rc.local#!/bin/sh## This script will be executed *after* all the other init scripts.# You can put your own initialization stuff in here if you don't# want to do the full Sys V style init stuff.touch /var/lock/subsys/local/opt/tomcat6/bin/startup.sh/usr/local/apache2/bin/apachectl start
```


	最后两行就是分别配置的tomcat及apache启动脚本，对于tomcat不能启动的原因，经多方尝试仍不得解决。

	http://bbs.chinaunix.net/thread-2223362-1-1.html

	http://www.cnblogs.com/diyunpeng/archive/2009/11/11/1600886.html

	http://blog.csdn.net/mezheng/article/details/7933228

	http://www.blogjava.net/shiliqiang/articles/317455.html

	  

