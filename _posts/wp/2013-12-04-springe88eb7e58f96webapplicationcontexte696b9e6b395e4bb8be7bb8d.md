---
author: yangzy666
comments: true
date: 2013-12-04 10:28:04+00:00
layout: post
slug: spring%e8%8e%b7%e5%8f%96webapplicationcontext%e6%96%b9%e6%b3%95%e4%bb%8b%e7%bb%8d
title: Spring获取WebApplicationContext方法介绍
wordpress_id: 754
categories:
- java
---
{% include JB/setup %}

	 但凡用Spring框架，就免不了获取SpringBean实例。众所周知，spring主要是依靠xml配置及较新的注解方式供spring实例化并装载到容器中，web程序启动并初始化所有Bean之后，框架会以反射的方式提供对应的bean给有需要的其他实例，或者程序员直接调用类似getBean("myBean")的方法向spring容器请求需要的bean以完成业务操作。

	 本文在此仅简单介绍后者即程序员手工获取bean的几种常用方法，而要获取bean，实现对getBean方法的操作，就需要获取WebApplicationContext实例：

	**一、史上最简单的获取WebApplicationContext方式**

```WebApplicationContext ctx = ContextLoader.getCurrentWebApplicationContext();MyBean bean=ctx.getBean("myBean",MyBean.class);
```


	  


	没错，就这么简单，尤其值得一提的是，从spring3.0开始，ContextLoaderListener继承了ContextLoader，也就是说，如果在系统的web.xml中配置了该listener，我们也可以这样获取WebApplicationContext。

```	<!-- Creates the Spring Container shared by all Servlets and Filters -->	<listener>		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>	</listener>
```


	  


	

```WebApplicationContext wac = ContextLoaderListener.getCurrentWebApplicationContext();
```
**二、通过指定的xml配置文件来获取WebApplicationContext** ```ApplicationContext ac = new FileSystemXmlApplicationContext("applicationContext.xml");ApplicationContext ac2 = new ClassPathXmlApplicationContext("applicationContext.xml");
```


	  


	此方法适合于独立应用程序，或者根据业务需求独立加载指定bean资源的情况。

	**三、在已知ServletContext情况下获取WebApplicationContext**

	这种方法应该是目前web应用程序中使用最普遍的，具体而言，它又有多种使用方式。 

```WebApplicationContext wac=(WebApplicationContext)servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE);
```


	  


	spring在web程序加载时，默认会将WebApplicationContext对象存放在ServletContext的属性中，并以WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE为属性名。

	当然，通过常用的WebApplicationContextUtils类也能获取WebApplicationContext，只不过多包裹了一层，最张还是从ServletContext中取出来的： 

```WebApplicationContext ac1 = WebApplicationContextUtils.getRequiredWebApplicationContext(ServletContext sc);WebApplicationContext ac2 = WebApplicationContextUtils.getWebApplicationContext(ServletContext sc);
```


	  


	另外，当我们在自己的业务类中需要调用WebApplicationContext时，除了以上的直接调用方式，还可以通过spring框架的自动依赖注入来获取该实例。此时需要实现相应的业务接口ApplicationContextAware或ServletContextAware。 

```public class MyService implements ApplicationContextAware {	private ApplicationContext applicationContext;	@Override	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {		this.applicationContext = applicationContext;	} //my business methods...}
```


	OR

```public class MyService implements ServletContextAware, ApplicationContextAware {	private ServletContext servletContext;	@Override	public void setServletContext(ServletContext servletContext) {		this.servletContext = servletContext;	} //my business methods... }
```


	**四、最后**

	在我们开发一个新的系统时，为了保证代码的统一和系统的规范，可以定义一个统一的全局接口供程序员调用以获取WebApplicationContext实例。

	另外，如果我们需要在web应用启动时，对WebApplicationContext对象进行某些操作的话（比如马上获取一个bean处理某项业务），也可以自定义一个实现了ServletContextListener 接口的类，与ContextLoaderListener一同配置到web.xml中。

	  


	

	  


	  


	  


	  

