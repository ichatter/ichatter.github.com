---
author: yangzy666
comments: true
date: 2013-05-25 18:53:20+00:00
layout: post
slug: linux%e4%b8%8b%e5%ae%89%e8%a3%85apache%e3%80%81php%e3%80%81phpmyadmin%e5%8f%8aapache%e5%8f%8d%e5%90%91%e4%bb%a3%e7%90%86tomcat%e9%85%8d%e7%bd%ae%e5%ae%9e%e6%88%98
title: Linux下安装Apache、PHP、phpMyAdmin及Apache反向代理Tomcat配置实战
wordpress_id: 602
categories:
- linux
- php
---
{% include JB/setup %}

	**前言**：本文配置过程中使用的apache及php皆为最新版本，所以与网络上的其他旧教程有所出入，理论而言更具实用性。

	**实战环境**：Linux Red Hat 4.1.2-44、Apache/2.4.4、PHP 5.4.15、Tomcat/6.0.20、phpMyAdmin 3.5.7

	**背景**：本人在工作中需要经常VPN至客户的局域网中对mysql数据库进行适当维护，使用的工具最初是ssh和mysql command line，然后是navicat，时间长了，对于每次需要频繁VPN比较烦，而且VPN后不能访问互联网，遇到问题需要上网搜索时深感不便。本人利用vpn及root进行ssh访问客户服务器的权限，“私自”在客户系统中安装了相关软件以方便日后数据库维护。

	**前提条件**：已拥有linux、gcc及tomcat环境  
(如果你的意图与本文稍有差异，比如你不想安装apache，也不想考虑代理的情况，只想简单地在tomcat下直接运行phpMyAdmin，你也可参考我的另**一**篇博文：**[tomcat运行phpMyAdmin配置](http://www.ichatter.cn/?p=472)**)  


	**一、下载**

	apache:

```[root@web-server ~]# wget http://mirrors.tuna.tsinghua.edu.cn/apache//httpd/httpd-2.4.4.tar.gz
```


	php:

```[root@web-server ~]# wget http://cn2.php.net/distributions/php-5.4.15.tar.bz2
```


	phpMyAdmin:

```[root@web-server ~]# wget http://ncu.dl.sourceforge.net/project/phpmyadmin/phpMyAdmin/4.0.1/phpMyAdmin-4.0.1-all-languages.zip
```


	**二、安装**

	由于apache安装时是没有自带php模块的，也就是说它默认不支持php程序，需要在安装php时进行添加，所以我们先安装apache，然后再安装php。

	**1.安装apache**

解压```[root@web-server ~]# tar –xvf httpd-2.4.4.tar.gz
```
安装```[root@web-server ~]# cd httpd-2.4.4[root@web-server httpd-2.4.4]# ./configure  --prefix=/usr/local/apache2 --enable-so[root@web-server httpd-2.4.4]# make[root@web-server httpd-2.4.4]# make install
```


	  


	参数说明：--prefix=/usr/local/apache2 #指定安装路径，如果不指定，默认路径也为/usr/local/apache2  
--enable-so #让apache编译动态加载模块(DSO)，此模块使得Apache的各功能模块可以与核心分开编译、运行时动态加载。有了DSO支持，升级和增加模块时只需编译相关的模块即可，不必重新编译整个系统。apache代理功能就需要用到mod_proxy.so及mod_proxy_http.so模块，本文后续会演示此模块的独立编译及配置

	小记：安装过程中可能会提示有相关依赖包缺失或版本过低等情况出现，当然具体安装环境不同，报错也会有所差异，比如我在本次安装过程中就出现了apr包和pcre包的问题，通过下载相关包的源码并编译即可解决。

	 **添加对php扩展文件的支持，找到apache的配置文件/usr/local/apache2/conf/httpd.conf，有两种方法**

	网上较老的一种：

```AddType application/x-httpd-php .php AddType application/x-httpd-php-source .phps
```


	现在推荐的一种：

```<FilesMatch "\.ph(p[2-6]?|tml)$">    SetHandler application/x-httpd-php</FilesMatch><FilesMatch "\.phps$">    SetHandler application/x-httpd-php-source</FilesMatch>
```


	  


	**2.安装php**

解压```[root@web-server ~]# tar –xvf php-5.4.15.tar.bz2
```
安装```[root@web-server ~]# cd php-5.4.15[root@web-server php-5.4.15]# ./configure  --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql --with-mysqli=mysqlnd[root@web-server php-5.4.15]# make[root@web-server php-5.4.15]# make install
```


	 参数说明：--prefix #在此没有指定安装路径，所以默认路径为/usr/local/php  
--with-apxs2=/usr/local/apache2/bin/apxs #指定php编译时需要用到的apxs程序的路径，这个程序使得php能够识别已安装的apache程序路径，以方便后续php相关模块装入apache，从而使得apache能够支持php环境。  
--with-mysql #指定系统中默认的mysql安装路径  
--with-mysqli=mysqlnd #最新版本的数据库和php应该配置mysqlnd，使php程序与mysql之间能正常连接

	 安装成功后会在apache的配置文件/usr/local/apache2/conf/httpd.conf中多出一行（对应的modules目录也会多出相应的so模块文件）:

```LoadModule php5_module        modules/libphp5.so
```


	  


	**3.安装phpMyAdmin**

	 这个安装就不多说了，直接解压后丢到apache的安装目录下/usr/local/apache2/htdocs就可以了

	**4.启动apache**

``` [root@web-server ~]# /usr/local/apache2/bin/apachectl start
```
访问[http://localhost:9080/phpMyAdmin](http://localhost:9080/phpMyAdmin) 页面成功

	  


	**三、apache反向代理tomcat**  
1.安装代理模块

	由于对外暴露的端口只有一个：9080，为了保证tomcat下的客户系统及phpMyAdmin都能通过外网访问到，考虑使用apache的代理功能，因此需要独立安装此模块。

	 简明安装方式如下：

```[root@web-server ~]# cd httpd-2.4.4/modules/proxy[root@web-server proxy]# /usr/local/apache2/bin/apxs -c -i -a mod_proxy.c proxy_util.c[root@web-server proxy]# /usr/local/apache2/bin/apxs -c -i -a mod_proxy_http.c proxy_util.c
```


	 参数说明：

	 -c 执行编译操作  
-i 安装操作，安装一个或多个动态共享对象到服务器的modules目录  
-a 自动增加一个LoadModule行到httpd.conf文件，以激活此模块，若此行存在则启用之  
-A 与-a类似，但是它增加的LoadModule行前有井号前缀(#)  
-e 需要执行编辑操作，可与-a和-A选项配合使用，与-i操作类似，修改httpd.conf文件，但并不安装此模块

	通过上面的安装命令，会自动完成代理模块的安装，查看/usr/local/apache2/conf/extra/httpd-vhosts.conf文件，可以看到相关模块已取消注释（对应的modules目录也会多出相应的so模块文件）：

```LoadModule proxy_module       modules/mod_proxy.soLoadModule proxy_http_module  modules/mod_proxy_http.so
```


	  


	2.配置文件修改

	 首先打开/usr/local/apache2/conf/httpd.conf  
修改监听端口 Listen 9080

	 然后打开/usr/local/apache2/conf/extra/httpd-vhosts.conf

```     <VirtualHost *:9080>        ServerAdmin webmaster@dummy-host.example.com        ProxyPass /phpMyAdmin !         ProxyPass / http://127.0.0.1:8081/        ProxyPassReverse / http://127.0.0.1:8081/        ServerName dummy-host.example.com        ServerAlias www.dummy-host.example.com        ErrorLog "logs/dummy-host.example.com-error_log"        CustomLog "logs/dummy-host.example.com-access_log" common    </VirtualHost>
```


	说明：主要的配置项为**ProxyPass**及**ProxyPassReverse**，其中**ProxyPass /phpMyAdmin ! **表示对phpMyAdmin程序的访问不进行转发，其他表示对服务器的端口9080的访问全部转发到本机的8081端口即tomcat管理的应用程序。切记[**http://127.0.0.1:8081/**](http://127.0.0.1:9080/) 后面的斜杠一定要有，要不然会出现js、css和图片等资源无法加载的问题。ProxyPassReverse从字面上就可以看出，它是相对于ProxyPass而言，是用来转发内部tomcat对外请求的响应.

	**四、总结**

	 众所周知，在linux环境下安装软件是一件比较棘手的事情，由于使用的linux版本不同，在安装软件的过程中所需的依赖包及其版本也可能不同，从而报错的提示信息也不尽相同，所以针对此情况，每个人在参考他人的配置方式时，更多的是需要举一反三，依自己的具体环境和错误提示来解决问题，否则完全地照搬照抄只会令自己陷入尴尬。用一句话概括linux安装特点：linux环境安装软件出错时主要就是报“相关依赖出错”，然后根据错误提示，到底是依赖不存在还是依赖包版本过高或过低，抑或其他，作出相应的解决方案。

	

	参考文章：  
apache代理模块

	[http://www.phpchina.com/resource/manual/apache/mod/mod_proxy.html](http://www.phpchina.com/resource/manual/apache/mod/mod_proxy.html)

	  


	apache安装手册

	[http://cn2.php.net/manual/en/install.unix.apache2.php](http://cn2.php.net/manual/en/install.unix.apache2.php)

	  


	

	(来自作者博客：[http://www.ichatter.cn/?p=602](http://www.ichatter.cn/?p=602))
