---
author: yangzy666
comments: true
date: 2012-10-16 09:59:40+00:00
layout: post
slug: spring2-5%e7%8e%af%e5%a2%83%e4%b8%ad%e4%b8%8d%e8%83%bd%e8%87%aa%e5%8a%a8%e6%b3%a8%e5%86%8chibernate%e5%ae%9e%e4%bd%93%e7%b1%bb%e7%9a%84%e9%97%ae%e9%a2%98
title: Spring2.5环境中不能自动注册实体类的问题
wordpress_id: 286
categories:
- spring
---
{% include JB/setup %}

	**问题一：**

	今天有一同事遇到DAO中总是报错：

```org.hibernate.QueryException: unexpected token: as [from SmsConfig as sc where 1=1] at org.hibernate.hql.classic.FromParser.token(FromParser.java:98) at org.hibernate.hql.classic.ClauseParser.token(ClauseParser.java:86) at org.hibernate.hql.classic.PreprocessingParser.token(PreprocessingParser.java:108) at org.hibernate.hql.classic.ParserHelper.parse(ParserHelper.java:28) at org.hibernate.hql.classic.QueryTranslatorImpl.compile(QueryTranslatorImpl.java:192) at org.hibernate.hql.classic.QueryTranslatorImpl.compile(QueryTranslatorImpl.java:168) at org.hibernate.engine.query.HQLQueryPlan.<init>(HQLQueryPlan.java:77) at org.hibernate.engine.query.HQLQueryPlan.<init>(HQLQueryPlan.java:56)
```
我记得前不久自己也遇到过这个问题，但是一时想不起来，当时是怎么解决的。对于犯过的错，应该记下来。经过网上搜索及测试，总结如下：由于这是一个维护中的旧产品，里面用的spring版本为2.5，在这个版本及之前，对于用@Entity标注的实体对象，spring不能自动加载，必须将相应的实体对象的路径加入XML配置文件，如：```<bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean"><property name="dataSource"><ref bean="dataSource" /></property><property name="annotatedClasses"><list><value> com.genlot.loms.persistence.model.SmsConfig </value></list></property></bean>
```
这种配置方式的最大缺点就是，当实体类新增或修改了包路径时，都必须在<property name="annotatedClasses">标签中进行注册或修改，极大地增加了代码量，同时也导致不安全问题的产生。所以我同事的问题，就是因为他新增的实体类没有在这个标签中注册，导致用hql语句查询时出现了异常org.hibernate.QueryException: unexpected token: as解决这个异常，要么就是在hql语句中写出实体类的全路径名，要么在这个标签中注册实体类的全路径名称。当然，在基于Spring 2.5及之前版本的情况下，只能选择后者才比较实际。而从Spring 2.5.6开始，增加了属性```<property name="packagesToScan"  value="com.**.entity"/>
```
通过这个属性的配置可以搜索到所有value值指定的表达式表示的实体类所在包，这样，我们就只需将实体类全部放在entity包中就可以被系统自动 加载，再也不用在<property name="annotatedClasses">标签中维护实体类的名称了。所以，我们在使用不同的开发工具以及相同开发工具的不同版本时，都应该注意其特性的差异，以有的放矢，命中问题，提高开发效率。

	**问题二：  
**通过Spring的HibernateTemplate执行hql语句时，如果使用了别名，在where条件语句中就必须使用实体对象的字段名，如：

```from SysUser as s where s.userName='admin';
```
否则，如下便会报错：```from SysUser where userName='admin';//错误形式
```
所以，如果不使用别名，就应该用sql形式的where语句，如下：```from SysUser where user_name='admin';
```
这是基于维护旧版本的Spring和Hiberante的时候遇到的问题，一开始也是莫名其妙，找不到路子，甚至在网上也找不到解决方案，最终自己大胆尝试后得出了答案。但是目前最新版本的Spring和Hibernate可能已经不适应此解决思路了，在此仅作一个维护笔记。

	  

