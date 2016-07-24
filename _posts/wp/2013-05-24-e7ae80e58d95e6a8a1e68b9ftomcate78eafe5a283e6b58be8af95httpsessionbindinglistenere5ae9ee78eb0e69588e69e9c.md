---
author: yangzy666
comments: true
date: 2013-05-24 06:56:53+00:00
layout: post
slug: '%e7%ae%80%e5%8d%95%e6%a8%a1%e6%8b%9ftomcat%e7%8e%af%e5%a2%83%e6%b5%8b%e8%af%95httpsessionbindinglistener%e5%ae%9e%e7%8e%b0%e6%95%88%e6%9e%9c'
title: 简单模拟tomcat环境测试HttpSessionBindingListener实现效果
wordpress_id: 587
categories:
- java
---
{% include JB/setup %}

HttpSessionBindingListener接口在很多情况下用于在线用户人数的统计与管理。不言而喻，session是必须的，但又不想专门启动tomcat运行一个web程序来测试它的效果。那么有没有办法直接调用tomcat自己的jar包来构造一个session来实现我的目的。比如：  


	1.调用tomcat的某个jar，这个包就是在tomcat运行时接收远程客户端的http请求时，用来构造HttpServletRequest、HttpServletResponse、HttpSession等我们熟知的servlet API中的实现类，现在我只需要HttpSession接口的实现。

2.获得了HttpSession实例之后，我就可以用session.setAttribute()方法将HttpSessionBindingListener的实例放入session，然后又调用session.removeAttribute()删除这个属性，看看HttpSessionBindingListener的实例中相应方法是否被触发，从而实现测试目的。  
  
通过多番测试，证明上面的步骤是完全能实现的。  


	一、首先将tomcat安装目录下面的所有jar包引入工程（其实并非全部需要，只是为了操作简单），包括lib及bin目录下的。

二、自定义HttpSessionBindingListener的实现类  
```import javax.servlet.http.HttpSessionBindingEvent;import javax.servlet.http.HttpSessionBindingListener;public class MyBindingListener implements HttpSessionBindingListener {	// 绑定与解绑定时需要操作的业务数据	private Object obj;	public MyBindingListener(Object obj) {		this.user = obj;	}	@Override	public void valueBound(HttpSessionBindingEvent event) {		System.out.println("hi...valueBound");		// process obj	}	@Override	public void valueUnbound(HttpSessionBindingEvent event) {		System.out.println("hi...valueUnbound");		// process obj	}}
```
三、寻找Tomcat中的HttpSession实现类  
其实也比较简单，首先查看一下tomcat源码对应的API文档[http://tomcat.apache.org/tomcat-7.0-doc/api/index.html](http://tomcat.apache.org/tomcat-7.0-doc/api/index.html)，找到后缀为session的包名：org.apache.catalina.session，经查看，HttpSession的实现类为StandardSession和StandardSessionFacade，事后测试，这两个都可以用来检查HttpSessionBindingListener的监听效果。不过为满足好奇，我还是启动了tomcat，在随便一个demo的页面中写上<%=request.getSession()%>，看看tomcat默认是使用的哪个实现，结果输出 org.apache.catalina.session.StandardSessionFacade@1f6df4c 那就用它吧。  
然后，就是写测试类，如下：```import org.apache.catalina.core.StandardContext;import org.apache.catalina.session.StandardManager;import org.apache.catalina.session.StandardSession;import org.apache.catalina.session.StandardSessionFacade;import org.junit.Test;public class HttpSessionBindingListenerTest {	@Test	public void test() {		StandardManager sm=new StandardManager();		sm.setContainer(new StandardContext());//添加容器		StandardSession ss=new StandardSession(sm);//构造一个session		System.out.println(ss.isValid());		ss.setValid(true);		System.out.println(ss.isValid());//session变为了有效，我们才能用它        //利用上面的session构造出我们需要的那个session，    //其实也可直接用上面的session测试         StandardSessionFacade session=new StandardSessionFacade(ss);//		StandardSession session=new StandardSession(sm);//		session.setValid(true);		System.out.println(session);		MyBindingListener my=new MyBindingListener(new Object());		session.setAttribute("my", my);		try {			Thread.currentThread().sleep(3000);//暂停3秒，看控制台的变化		} catch (InterruptedException e) {			// TODO Auto-generated catch block			e.printStackTrace();		}		session.removeAttribute("my");	}}
```


	 运行结果：

```falsetrueorg.apache.catalina.session.StandardSessionFacade@1f6df4chi...valueBoundhi...valueUnbound
```


	 说明，为求简便，MyBindingListener类中的属性只是简单使用了Object类型，具体可依个人实现业务需要进行相应修改，然后在对应的触发方法中对其进行相应处理。
