---
author: yangzy666
comments: true
date: 2012-09-20 16:20:33+00:00
layout: post
slug: spring-web-mvc%e4%b8%ad%e5%85%b3%e4%ba%8erestful%e8%ae%bf%e9%97%ae%e8%b7%af%e5%be%84%e7%9a%84%e9%97%ae%e9%a2%98
title: Spring web mvc中关于restful访问路径的问题
wordpress_id: 275
categories:
- spring
---
{% include JB/setup %}

前几天在写新系统中的一个controller时，使用了http://localhost:8080/myproject/getinfo/getinfo这样的路径去访问服务器，结果服务器报错（返回400还是500错误，记不清了），也就是说spring web mvc不支持两级相同的访问路径。只需将其改成http://localhost:8080/myproject/getinfo/getinfo2即可。
