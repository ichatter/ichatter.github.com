---
author: yangzy666
comments: true
date: 2012-11-15 05:44:33+00:00
layout: post
slug: bitbucket%e4%bd%93%e9%aa%8c
title: bitbucket体验
wordpress_id: 309
categories:
- 管理工具
---
{% include JB/setup %}

跟github.com一样，bitbucket.org也是一个代码托管的著名网站，关于两者的比较，可以baidu或google，一大把。在此主要简谈下我对bitbucket的使用感受。

	
  1. 在bitbucket.org注册一个帐号（不能是中文），有了帐号就可以马上新建仓库，没有数量限制，而且可以是私有库，如果是team开发，最多可以有5位成员，这一切都是免费的哦。另外，值得一提的是，注册的帐号登陆名是可以随时改的，只要这个名字没有被其他人使用过。比如我注册的帐户名：aaaaaa，然后我在帐户设置中又将其改为了yzy。当然，改了名字之后，git仓库的远程分支的访问也会随着改变，以前本地访问的路径也要随之修改了。
	
  2. 建好自己的仓库后，比如我的仓库名为abc，那么就会自动生成一个远程仓库路径：https://yzy@bitbucket.org/yzy/abc.git这是配置本地访问远程时需要的信息。
	
  3. 进入仓库abc可以看到，由于没有任何内容，会有个向导教你如何在本地创建一个仓库，并将本地的内容提交到bitbucket的远程仓库abc。其主要命令如下：```#新建一个目录作为本地仓库$ mkdir /path/to/your/project #进入仓库目录$ cd /path/to/your/project #初始化当前目录为仓库，在此目录下会多出.git文件夹$ git init #添加远程仓库，在本地的名称为origin$ git remote add origin https://yzy@bitbucket.org/yzy/abc.git
```
```$ echo "# This is my README" >> README.md#将文件README.md加入到索引（准备提交）$ git add README.md #提交至本地仓库（注意：这里的commit只表示提交到了本地仓库的指定分支）$ git commit -m "First Commit. Adding a README."#这里才表示提交到远程仓库的分支master中（进行网上上传）$ git push -u origin master
```
通过以上命令，就可以以最精简的方式完成本地仓库的创建或初始化工作，并建立与远程仓库的连接，并提交了一个readme文件。
