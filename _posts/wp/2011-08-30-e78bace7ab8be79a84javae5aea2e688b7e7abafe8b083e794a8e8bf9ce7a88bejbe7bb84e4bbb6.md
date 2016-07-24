---
author: yangzy666
comments: true
date: 2011-08-30 08:56:36+00:00
layout: post
slug: '%e7%8b%ac%e7%ab%8b%e7%9a%84java%e5%ae%a2%e6%88%b7%e7%ab%af%e8%b0%83%e7%94%a8%e8%bf%9c%e7%a8%8bejb%e7%bb%84%e4%bb%b6'
title: 独立的java客户端调用远程EJB组件
wordpress_id: 200
categories:
- java
---
{% include JB/setup %}

调用远程EJB组件的场景有很多，比如，从tomcat或resin等纯web容器调用EJB；从纯独立的java客户端（比如swing）等访问EJB组件；从web service程序调用EJB组件等。另外由于使用的JAVA EE应用服务器不同，调用的方式也略有差异。本文以独立java客户端调用EJB组件的为例，简单总结一下。开发环境如下：<!-- more -->操作系统：windows 7服务器：glassfish v3IDE：netbeans 7.01.通过netbeans创建企业应用程序，工程名为Hello，对应的EJB模块则自动命名为Hello-ejb。2.在Hello-ejb中添加无状态会话bean。业务接口Hello.java```package examples.session.stateless;/*** This is the Hello business interface.*/public interface Hello {/*** @return a greeting to the client.*/public String hello();}
```
接口实现类HelloBean.java```package examples.session.stateless; import javax.ejb.Remote; import javax.ejb.Stateless; /** * Demonstration stateless session bean. */ @Stateless @Remote(Hello.class) public class HelloBean implements Hello { public String hello() { System.out.println("hello()"); return "Hello, World!"; } }
```
3.新建一个java客户端工程Hello-client，添加一个客户端启动类```HelloClient.java。 package examples.session.stateless; import java.rmi.Naming; import java.rmi.registry.LocateRegistry; import java.rmi.registry.Registry; import java.util.Hashtable; import javax.naming.Context; import javax.naming.InitialContext; // The following import should be removed. /** * This class is an example of client code which invokes * methods on a simple, remote stateless session bean. */ public class HelloClient { public static void main(String[] args) throws Exception { /* * Obtain the JNDI initial context. * * The initial context is a starting point for * connecting to a JNDI tree. We choose our JNDI * driver, the network location of the server, etc * by passing in the environment properties. */ System.out.println("about to create initialcontext!"); //Hashtable env = new Hashtable(); //env.put("java.naming.factory.initial","com.sun.enterprise.naming.SerialInitContextFactory"); //env.put("java.naming.factory.url.pkgs","com.sun.enterprise.naming"); //env.put("java.naming.provider.url","rmi://localhost:3700"); //Context ctx = new InitialContext(env); Context ctx = new InitialContext(); System.out.println("Got initial context ... yeah "); Hello hello = (Hello) ctx.lookup("examples.session.stateless.Hello"); /* * Call the hello() method on the bean. * We then print the result to the screen. */ System.out.println(hello.hello()); } }
```
4.客户端调用分两种情况，一种是在服务器所在电脑上调用EJB组件，即客户端和EJB容器在同一JVM中；另一种是客户端处于局域网或者互联网中的某台电脑上，即两者分别处于不同JVM中。第一种情况，针对当前版本的glassfish v3服务器，只需在Hello-client工程中添加glassfish服务器安装目录下的lib/gf-client.jar包引用即可，gf-client.jar会自动引用其需要的包。如果是glassfish v2，则是引用lib下的appserv-rt.jar等相关包。第二种情况，其他电脑由于是拥有自己独立的JVM，所以通过远程调用本机的EJB组件时，对初始化上下文环境需要带参数即new InitialContext(env);把上面客户端被注释掉的代码取消注释，并注释掉下面那行不带参的初始化上下文环境代码即Context ctx = new InitialContext();另外把第一种情况中需要引用的相关包与客户端工程一起打包发布，远程客户端就可以使用了，比如：D:\>java -jar Hello-client.jar客户端控制台输出：Got initial context ... yeahHello, World!";
