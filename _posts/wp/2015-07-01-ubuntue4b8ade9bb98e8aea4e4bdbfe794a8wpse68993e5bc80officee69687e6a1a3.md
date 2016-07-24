---
author: yangzy666
comments: true
date: 2015-07-01 10:57:10+00:00
layout: post
slug: ubuntu%e4%b8%ad%e9%bb%98%e8%ae%a4%e4%bd%bf%e7%94%a8wps%e6%89%93%e5%bc%80office%e6%96%87%e6%a1%a3
title: ubuntu中默认使用wps打开office文档
wordpress_id: 958
categories:
- linux
---
{% include JB/setup %}

	下载解压wps后即可直接打开wps，但用户在打开各类office文档（xls、ppt、doc等）时只能选用系统中的默认程序libreOffice，所以我们需要添加wps作为office文档可选的打开程序。

	在ubuntu中，有三个文件可配置默认程序：

```1./etc/gnome/defaults.list2./usr/share/applications/defaults.list3.~/.local/share/applications/mimeapps.list
```
其中第一和第二个属于系统级配置文件，里面包含了系统中配置好的文件类型及其对应的打开程序；第三个文件为当前登陆用户的局部配置环境，如果与前面系统级的配置有重复，则以局部配置优先，比如：我要修改xls文件的打开方式，我就会在第三个文件即mimeapps.list中加入一行：```application/vnd.ms-excel=wps-sheet.desktop
```


	**注意**：wps-sheet.desktop是一个自定义的桌面快捷键，存放在与文件mimeapps.list相同的目录，即~/.local/share/applications/。在此不讨论如何创建快捷键。

保存后即可在右击excel文档时发现多出了一个wps打开选项，如图：

	  


	![](uploads/2015/07/20150701103619_44901.png)

	  


	同理，我们可以继续修改，实现把word、ppt和其他任何文档以我们指定的程序打开，最后WPS配置后应该如下：

	  


```application/vnd.ms-excel=wps-sheet.desktopapplication/msword=wps.desktopapplication/vnd.openxmlformats-officedocument.presentationml.presentation=wps-ppt.desktop
```
最后，关于文件类型，可右击文件属性查看，然后按如上格式配置好对应的快捷方式（.desktop），比如，查看excel：

	  


	![](uploads/2015/07/20150701105654_35009.png)

	  

