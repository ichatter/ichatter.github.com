---
author: yangzy666
comments: true
date: 2013-04-26 01:18:21+00:00
layout: post
slug: '%e4%bf%ae%e6%94%b9windows%e6%93%8d%e4%bd%9c%e7%b3%bb%e7%bb%9f%e7%9a%84%e9%bb%98%e8%ae%a4%e6%b5%8f%e8%a7%88%e5%99%a8'
title: 修改windows操作系统的默认浏览器
wordpress_id: 490
categories:
- 网络
---
{% include JB/setup %}

	 电脑用久了，会发现自己的默认浏览器被篡改，而且在Chrome浏览器的设置项中已无法将其修改为默认浏览器，今天我也遇到了，于是在网上找了些修复方法，自己试了两种，经验证有效。

	  


**系统环境：**windows XP SP3  


	**已安装浏览器：**IE8、Chrome、Firefox、百度浏览器

	  


	方法一：通过控制面板修改

	控制面板→添加或删除程序→设定程序访问默认值→自定义，然后选择自己喜欢的Chrome

	![QQ截图20130426092515](http://www.ichatter.cnuploads/2013/04/QQ截图20130426092515.jpg)

	  


	方法二：通过修改注册表实现

	1.HKEY_CLASSES_ROOT\http\shell\open\command，双击"默认"，输入默认浏览器的可执行文件的完整路径。例如：输入**"C:\Program Files\Google\Chrome\Application\chrome.exe"  "%1"**。注意后面有**空格+"%1"**

	2.HKEY_CLASSES_ROOT\http\shell\open\ddeexec\Application，双击"默认"，设置浏览器名，如果是Firefox则输入Firefox，如果是IE则输入IExplore。（此步骤为可选）
