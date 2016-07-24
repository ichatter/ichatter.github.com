---
author: yangzy666
comments: true
date: 2013-08-08 06:44:16+00:00
layout: post
slug: windows%e7%b3%bb%e7%bb%9f%e4%b8%ad%e9%80%9a%e8%bf%87%e8%a2%ab%e5%8d%a0%e7%94%a8%e7%9a%84%e7%ab%af%e5%8f%a3%e6%89%be%e5%88%b0%e8%bf%9b%e7%a8%8b
title: windows系统中通过被占用的端口找到进程
wordpress_id: 710
categories:
- 管理工具
- 网络
---
{% include JB/setup %}

	 通常在linux平台下会用一些基本的命令来找到对应的进程并杀死，但今天在本地windows电脑上使用某个软件时，有个端口被占用了，不想注销也不想重启，于是找到了这几个命令，轻松搞定。

	一、用netstat命令找到占用指定端口的进程ID

```C:\Documents and Settings\Administrator>netstat -ano|findstr 7077  Proto  Local Address          Foreign Address        State           PID  TCP    0.0.0.0:7077           0.0.0.0:0              LISTENING       2352
```
这里的端口是7077，通过此命令找到了进程ID2352。

	二、用tasklist命令找到进程ID2352对应的程序名称

```C:\Documents and Settings\Administrator>tasklist|findstr 2352图像名                       PID 会话名           会话#       内存使用========================= ====== ================ ======== ============plink.exe                   2352 Console                 0      5,820 K
```
原来是名叫plink.exe的程序占用了此端口，那就在任务管理器中找到并直接干掉它吧。

	三、不想用任务管理器，那就用命令taskkill吧

```C:\Documents and Settings\Administrator>taskkill /PID 2352成功: 已终止 PID 为 2352 的进程。
```
直接通过命令taskkill杀死指定进程2352。其实挺简单，在操作上整体跟linux平台思想相通，只是命令及其参数不同耳。  

