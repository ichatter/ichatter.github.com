---
author: yangzy666
comments: true
date: 2014-03-09 09:55:22+00:00
layout: post
slug: kindeditorsyntaxhighlighter%e4%b8%aa%e6%80%a7%e5%8c%96%e4%bf%ae%e6%94%b9
title: KindEditor+SyntaxHighlighter个性化修改
wordpress_id: 837
categories:
- javascript
- php
---
{% include JB/setup %}

	 我的个人博客用的富文本编辑器是KindEditor，这是国产开源软件中的一款精品，但作为程序员，它的默认语法高亮用的是google prettify，整体色调比较素雅，为了满足实际需求，也引入了SyntaxHighlighter作为第二个语法高亮工具，国内许多著名网站都用了这个工具，如CSDN和开源中国。关于二者的结合方法，网上很容易找到，本文也会简单介绍。

	 在我的wordpress博客中使用的对应插件是Kindeditor For Wordpress+SyntaxHighlighter Evolved。我发现，大多情况下，使用SyntaxHighlighter进行语法着色即可，但如果代码行数较少，使用Kindeditor默认的prettify则更好看。于是我决定对Kindeditor做些微的改动，使我在输入代码时可自由选择使用何种着色效果：SyntaxHighlighter or prettify。先看效果：

	

```public static void main(String[] args){    System.out.println("hello,this is colored by SyntaxHighlighter!");}
```
```public static void main(String[] args){    System.out.println("hello,this is colored by prettify!");}
```
两者唯一的区别就是在输入代码框时，我加入了一个自定义的复选框，如果勾选，则表示使用prettify，否则默认SyntaxHighlighter ：

	![](uploads/2014/03/20140309112304_35896.jpg)

	这样我就实现了两种语法高亮工具同时使用，鱼和熊掌兼得。下面再看我对源码的一点修改。

	1.修改Kindeditor For Wordpress/plugins/code/code.js

```KindEditor.plugin('code', function(K) {	var self = this, name = 'code';	self.clickToolbar(name, function() {		var lang = self.lang(name + '.'),			html = ['<div style="padding:10px 20px;">',				'<div class="ke-dialog-row">',				'<select class="ke-code-type">',				'<option value="js">JavaScript</option>',				'<option value="html">HTML</option>',				'<option value="css">CSS</option>',				'<option value="php">PHP</option>',				'<option value="pl">Perl</option>',				'<option value="py">Python</option>',				'<option value="rb">Ruby</option>',				'<option value="java">Java</option>',				'<option value="vb">ASP/VB</option>',				'<option value="cpp">C/C++</option>',				'<option value="cs">C#</option>',				'<option value="xml">XML</option>',				'<option value="bsh">Shell</option>',				'<option value="">Other</option>',				'</select>',				'<label>&nbsp;&nbsp;<input type="checkbox" id="isPretty">使用prettify</label>',//1.加入自定义的选择按钮				'</div>',				'<textarea class="ke-textarea" style="width:408px;height:260px;"></textarea>',				'</div>'].join(''),			dialog = self.createDialog({				name : name,				width : 450,				title : self.lang(name),				body : html,				yesBtn : {					name : self.lang('yes'),					click : function(e) {						var type = K('.ke-code-type', dialog.div).val(),							code = textarea.val(), //2.根据是否选择prettify生成相应的样式							cls = type === '' ? '' : (isPretty.checked ? (' lang-' + type) : type),							html = '<pre class="' + (isPretty.checked ? 'prettyprint':'brush:') + cls + '">\n' + K.escape(code) + '</pre> ';						if (K.trim(code) === '') {							alert(lang.pleaseInput);							textarea[0].focus();							return;						}						self.insertHtml(html).hideDialog().focus();					}				}			}),			textarea = K('textarea', dialog.div);		textarea[0].focus();	});});
```


	  


	其实很简单，其他代码不用太关注，就只修改了两处（有注释）。 

	2.修改Kindeditor For Wordpress/plugins/code/prettify.css

```.ke-content pre { border: 1px solid #ddd; border-left: 5px solid #6CE26C; background: #f6f6f6; margin-left: 2em; padding: 0.5em; font-size: 110%; display: block; font-family: "Consolas", "Monaco", "Bitstream Vera Sans Mono", "Courier New", Courier, monospace; margin: 1em 0px; white-space: pre;}pre.prettyprint { border: 0; border-left: 3px solid rgb(204, 204, 204); margin-left: 2em; padding: 0.5em; font-size: 110%; display: block; font-family: "Consolas", "Monaco", "Bitstream Vera Sans Mono", "Courier New", Courier, monospace; margin: 1em 0px; white-space: pre;} 
```


	  


	更简单，在原来的样式pre.prettyprint前面加入一个样式.ke-content pre（其实就是对pre.prettyprint复制过来再修改了一点颜色）。这个样式文件的修改并不是必须的，他只是在文本编辑期间，让正在编辑的代码可以被简单预览，而在文章发布出去以后，这个样式基本就不会被使用了。

	这样，我的两个着色工具就可以自由切换使用了。

	  


	  

