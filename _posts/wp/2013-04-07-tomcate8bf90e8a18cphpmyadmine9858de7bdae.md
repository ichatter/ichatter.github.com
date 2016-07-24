---
author: yangzy666
comments: true
date: 2013-04-07 05:55:18+00:00
layout: post
slug: tomcat%e8%bf%90%e8%a1%8cphpmyadmin%e9%85%8d%e7%bd%ae
title: tomcat运行phpMyAdmin配置
wordpress_id: 472
categories:
- java
- php
- 网络
---
{% include JB/setup %}

前提条件：已拥有了tomcat及java环境  
**一、下载**  
1.最新版本php环境  
官网：http://www.php.net/downloads.php  
本测试是在windows xp环境进行，所以我下载了windows版本的二进制包  
[php-5.4.13-nts-Win32-VC9-x86.zip](http://windows.php.net/downloads/releases/php-5.4.13-nts-Win32-VC9-x86.zip)  


	2.最新版本的phpMyAdmin  
官网：http://www.phpmyadmin.net/home_page/downloads.php  
我下载的是[phpMyAdmin-3.5.7-all-languages.zip](http://sourceforge.net/projects/phpmyadmin/files/phpMyAdmin/3.5.7/phpMyAdmin-3.5.7-all-languages.zip/download#!md5!fe0f19275bc860b8a5679b74ed1f3333)

**二、配置**  
1.tomcat配置  
我用的是apache-tomcat-7.0.21。tomcat默认只支持jsp，要想使其运行php，当然得适当配置。  
首先配置${TOMCAT_HOME}/conf/web.xml，将关于cgi的内容取消注释，如下：```<servlet>    <servlet-name>cgi</servlet-name>    <servlet-class>org.apache.catalina.servlets.CGIServlet</servlet-class>    <init-param>       <param-name>debug</param-name>       <param-value>0</param-value>    </init-param> <init-param>       <param-name>passShellEnvironment</param-name>           <param-value>true</param-value>        </init-param> <init-param>       <param-name>executable</param-name>          <param-value>php-cgi</param-value>        </init-param>    <init-param>       <param-name>cgiPathPrefix</param-name>       <param-value>WEB-INF/cgi</param-value>    </init-param>    <load-on-startup>5</load-on-startup></servlet>
```
当然还有它的映射servlet-mapping，如下：```<servlet-mapping> <servlet-name>cgi</servlet-name> <url-pattern>/cgi-bin/*</url-pattern></servlet-mapping>
```
关于上面CGIServlet的参数<param-name>executable</param-name>，有两种配置方式，如果在安装php环境时已将php安装目录配置到了系统变量path中，则只需写相应的命令名，如上。另一种是直接指定php解释程序的绝对路径，如<param-value>D:\php\php-cgi.exe</param-value>。  
**注意：** ```a.php安装目录下有三个exe可执行文件，除上面的php-cgi.exe之外，还有php.exe及php-win.exe，经本人简单测试，要想让php文件能被tomcat正常解析，应该使用php-cgi.exe。
```
```b.参数passShellEnvironment必须显示设为true，否则在phpMyAdmin首页输入帐密无法登入。
```
然后配置${TOMCAT_HOME}/conf/context.xml。很简单，在<Context>元素中加入属性，如<Context privileged="true">，配置这个属性的目的就是让tomcat能够启用CGIServlet，否则php文件无法执行。  
  
2.php配置  
我将php压缩文件解压到D盘，并将目录名简化为php，即D:\php。  
首先将\php目录下的php.ini-development复制并重命名为php.ini，然后修改此文件，如下：```cgi.force_redirect 去掉前面的;分号，并改为0（默认为1，页面上会有安全提示信息，必须设为0）extension_dir = "ext"  去掉前面的;分号。extension=php_mbstring.dll  去掉前面的;分号。extension=php_mysqli.dll  去掉前面的;分号。
```
这里配置的目的是为了使用php正常工作及连接mysql所需的扩展组件。  


	  


3.phpMyAdmin配置  
将phpMyAdmin-3.5.7程序解压，并重命名为phpMyAdmin，然后拷贝到${TOMCAT_HOME}\webapps\ROOT\WEB-INF\cgi目录下，自建cgi目录。  
  
**三、结束**  
启动tomcat，通过浏览器访问：http://localhost:8080/cgi-bin/phpMyAdmin/index.php  
登陆页面成功显示。  

