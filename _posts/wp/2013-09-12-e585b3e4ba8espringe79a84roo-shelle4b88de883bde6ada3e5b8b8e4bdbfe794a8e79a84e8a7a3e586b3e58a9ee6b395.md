---
author: yangzy666
comments: true
date: 2013-09-12 03:45:50+00:00
layout: post
slug: '%e5%85%b3%e4%ba%8espring%e7%9a%84roo-shell%e4%b8%8d%e8%83%bd%e6%ad%a3%e5%b8%b8%e4%bd%bf%e7%94%a8%e7%9a%84%e8%a7%a3%e5%86%b3%e5%8a%9e%e6%b3%95'
title: 关于spring的roo shell不能正常使用的解决办法
wordpress_id: 722
categories:
- java
---
{% include JB/setup %}

	 现在spring的使用非常流行，而且可能爱屋及乌地用上了STS(spring source toolsuite)，而该套开发工具自带了maven及spring roo工具，所以很多小伙伴们会使用这些插件。但要命的是，许多小伙伴在实际应用这些插件的时候，会遇到各种莫名其妙的问题，比如roo shell不能使用了，真令人抓狂啊。

	 基于我的使用经验及同事的一些解决办法，总结常见的问题及解决办法。

	 **问题1：**roo shell报错，无法启动。

	 **问题2：**roo shell启动后相关命令变少了，比如我要建个实体对象，连个entity命令都出不来了。

	 解决办法：对于第一个问题，一般最好将spring roo工具包解压到非中文目录下即可（roo shell启动时貌似对中文路径解析有问题，有时会出现乱码）；第二个问题最常见，也是最伤脑筋的，往往是roo shell可以正常使用，但某一天突然出现了，即便重装spring roo甚至STS也无济于事（如果你重装了操作系统那我没话说了）。

	 当找到解决办法时，才发现问题原来如此简单：删除掉spring roo安装目录下使用后产生的缓存文件，如下图。

	![](http://www.ichatter.cnuploads/2013/09/20130912113546_17742.jpg)

	（删除以sts-cache或.sts-cache开头的文件）

	如果这样依然无法解决问题，那就删除当前系统用户目录下的一个spring roo相关文件，比如我的是**C:\Documents and Settings\Administrator\****.spring_roo_pgp.bpg**

	这样就可以解决了，爽快地用你的roo shell吧。

	小伙伴们，你们的问题解决了吗。
