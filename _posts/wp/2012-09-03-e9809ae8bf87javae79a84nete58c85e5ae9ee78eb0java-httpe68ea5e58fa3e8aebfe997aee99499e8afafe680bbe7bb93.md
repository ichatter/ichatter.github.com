---
author: yangzy666
comments: true
date: 2012-09-03 14:36:28+00:00
layout: post
slug: '%e9%80%9a%e8%bf%87java%e7%9a%84net%e5%8c%85%e5%ae%9e%e7%8e%b0java-http%e6%8e%a5%e5%8f%a3%e8%ae%bf%e9%97%ae%e9%94%99%e8%af%af%e6%80%bb%e7%bb%93'
title: 通过JAVA的net包实现JAVA http接口访问错误总结
wordpress_id: 263
categories:
- java
---
{% include JB/setup %}

今天在测试http短信接口时，犯了个错误，导致一天工作下来都不爽，晚上回来调试了很久，总算找到问题所在，竟然很简单，甚感羞愧，特此记录，以绝再犯。错误代码： ```try {	URL myUrl = new URL(			"http://sms.hcsdsms.com:8080/SmsService/SmsService.asmx/SendEx?"					+ "UserId=XXXXXX&Password=XXXXXX&MsgContent=短信测试"					+ "&DestNumber=15817611677&SendTime=&SubNumber="					+ "&BizType=22&WapURL=&BatchSendID=");	HttpURLConnection conn = (HttpURLConnection) myUrl.openConnection();	conn.setDoOutput(true);	conn.setDoInput(true);	OutputStreamWriter osw = new OutputStreamWriter(			conn.getOutputStream());	osw.write("");	osw.flush();	osw.close();	BufferedReader br = new BufferedReader(new InputStreamReader(			conn.getInputStream()));	String s = null;	while ((s = br.readLine()) != null) {		sb.append(s);	}	conn.disconnect();} catch (MalformedURLException e) {	e.printStackTrace();} catch (IOException e) {	e.printStackTrace();}
```
当执行程序时，控制台报500错误：```java.io.IOException: Server returned HTTP response code: 500 for URL: http://sms.hcsdsms.com:8080/SmsService/SmsService.asmx/SendEx?UserId=XXXXXX&amp;Password=XXXXXX&amp;MsgContent=短信测试&amp;DestNumber=15817611677&amp;SendTime=&amp;SubNumber=&amp;BizType=22&amp;WapURL=&amp;BatchSendID= at sun.net.www.protocol.http.HttpURLConnection.getInputStream(Unknown Source) at sms.SmsHttpTest.doHttp(SmsHttpTest.java:43) at sms.SmsHttpTest.test(SmsHttpTest.java:23)
```
正确代码：```try {	URL myUrl = new URL(			"http://sms.hcsdsms.com:8080/SmsService/SmsService.asmx/SendEx?");	HttpURLConnection conn = (HttpURLConnection) myUrl.openConnection();	conn.setDoOutput(true);	conn.setDoInput(true);	OutputStreamWriter osw = new OutputStreamWriter(			conn.getOutputStream());	osw.write("UserId=XXXXXX&Password=XXXXXX&MsgContent=短信测试"			+ "&DestNumber=15817611677&SendTime=&SubNumber="			+ "&BizType=22&WapURL=&BatchSendID=");	osw.flush();	osw.close();	BufferedReader br = new BufferedReader(new InputStreamReader(			conn.getInputStream()));	String s = null;	while ((s = br.readLine()) != null) {		sb.append(s);	}	conn.disconnect();} catch (MalformedURLException e) {	e.printStackTrace();} catch (IOException e) {	e.printStackTrace();}
```


	 执行程序，得到返回的XML结构数据：

```<?xml version="1.0" encoding="utf-8"?><SendExResp xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"	xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://tempuri.org/">	<PayCount>1</PayCount>	<BlackWords />	<ErrorMobiles />	<BlackMobiles />	<BatchSendID>00000000-0000-0000-0000-000000000000</BatchSendID>	<Result>0</Result>	<ErrorDesc>成功</ErrorDesc></SendExResp> 
```
详解：通过url字符串构造java.net.URL对象，并打开连接（myUrl.openConnection()）时，实际上只针对URL中的访问路径http://sms.hcsdsms.com:8080/SmsService/SmsService.asmx/SendEx?建立了连接，而后面的一系列参数串并不是建立连接的有效部分，所以被舍去了。当访问API时，由于服务端强制要求相应的参数必不可少，在建立连接后也没有把相应的参数通过输出流write到服务端，所以程序报错。在正确的代码中，通过将服务端强制要求的必需参数write到输出流中，实现了正常的访问，得到了正确的返回结果。

	  

