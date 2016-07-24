---
author: yangzy666
comments: true
date: 2014-01-26 04:53:05+00:00
layout: post
slug: hibernate%e6%b3%a8%e8%a7%a3formula%e4%bd%bf%e7%94%a8%e4%b8%ad%e8%a6%81%e6%b3%a8%e6%84%8f%e7%9a%84%e4%b8%80%e7%82%b9
title: hibernate注解@Formula使用中要注意的一点
wordpress_id: 778
categories:
- java
- 工作日志
---
{% include JB/setup %}

	在工作中遇到一个@Formula使用中的小问题，记录一下。

	以例子形式展现：

	  


```import javax.persistence.Entity;import javax.persistence.Table;import org.hibernate.annotations.Formula;@Entity@Table(name = "in_come")public class Income {	private int id;	private double salary;// 薪水	private double reward;// 奖金	private double subsidy;// 补助	@Formula("(select salary + reward + subsidy from in_come a where a.id=id)")	private double total;// 总收入	public int getId() {		return id;	}	public void setId(int id) {		this.id = id;	}        //省略其他getter和setter方法} 
```


	如上代码，我想通过@Formula来统计各类收入之和。但很不幸，每当新增或修改完一条记录后，自动跳转到记录列表页面时，total死活出不来，总是为0，需要将该列表页面重新查询一遍才行。

	**解决方法**：当新增或修改完记录后，且在自动跳转到列表页面之前，将会对记录重新查询，在查询之前加入一句**EntityManager.clear()**即可。这句的目的是将刚刚操作过的记录彻底从缓存中清除，迫使程序查询时从数据库中获取最新的数据，这样@Formula中定义的SQL就能够真正被执行。

	  


	  


	  


	  


	  

