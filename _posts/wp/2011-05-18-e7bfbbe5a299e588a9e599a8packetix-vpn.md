---
author: yangzy666
comments: true
date: 2011-05-18 15:41:33+00:00
layout: post
slug: '%e7%bf%bb%e5%a2%99%e5%88%a9%e5%99%a8packetix-vpn'
title: 翻墙利器PacketiX VPN
wordpress_id: 41
categories:
- 生活
- 网络
---
{% include JB/setup %}

其实我很少翻墙出去呼吸高墙外的新鲜空气，自从认识到翻墙这个词以来，我知道了我们中国大陆有个最大的局域网，在这个局域网和Internet之间，有个很牛B的墙叫GFW（the great firewall of China），也知道了不少墙友如饥似渴想有个稳定的梯子能时不时爬到墙外去爽一把，只可惜天下没有永恒的梯子，正如那个很牛B的GFW之父方滨兴所言，我对那些像反ZF的众多言论不感兴趣！GFW和vpn之间的战争是场永久战。<!-- more -->

记得以前为了找一些资料，在网上苦寻在线代理网站，然后把这个网站发布的临时代理IP地址一个个尝试，当找到一个可用的代理IP时，心里感觉特别兴奋，终于找到了，哇。只可惜第二天这个代理可能就已经失效了，然后又得去寻找新的，如此反复，心想翻墙真不容易啊，当然也用过后来的一些vpn软件，但也是好命不长，不久就被封了，不仅如此，这些临时代理和免费的vpn软件往往速度也奇慢。后来用上了在google appengine上部署的一个比较好的免费代理软件GoogleAppproxy，这个速度很快，性能稳定，但最终还是印证了GFW之父所言，这是一场永久战，道高一尺，魔高一丈，GoogleAppproxy最终还是倒下了。可喜的是，这场战争没有永久的赢家，最近又认识了一款比较好的翻墙利器，它就是PacketiX VPN，目前版本是3.01。啰嗦了这么多，那就好好介绍一下这个日本人搞的VPN吧，手把手教大家怎么下载、安装、配置这个软件。

# 1.下载

**客户端下载地址：**[http://uploader.softether.co.jp/vpn3/v3.01-7177-rtm-2010.10.10/VPN/Simplified_Chinese/](http://uploader.softether.co.jp/vpn3/v3.01-7177-rtm-2010.10.10/VPN/Simplified_Chinese/)（这个软件确实不错，除windows版本外，还有其他几个主流操作系统下的版本，墙友可根据自身需要选择下载，目前本人用的是windows 7  x86_32位系统，所以在此以windows版本的PacketiX VPN介绍，我选择的下载：[http://uploader.softether.co.jp/vpn3/v3.01-7177-rtm-2010.10.10/VPN/Simplified_Chinese/Windows/PacketiX%20VPN%20Client%203.0/32bit%20-%20Intel%20x86/](http://uploader.softether.co.jp/vpn3/v3.01-7177-rtm-2010.10.10/VPN/Simplified_Chinese/Windows/PacketiX%20VPN%20Client%203.0/32bit%20-%20Intel%20x86/)[vpnclient-v3.01-7177-rtm-2010.10.10-zh_cn-win32_msi-x86-32bit.exe](/vpn3/v3.01-7177-rtm-2010.10.10/VPN/Simplified_Chinese/Windows/PacketiX%20VPN%20Client%203.0/32bit%20-%20Intel%20x86/vpnclient-v3.01-7177-rtm-2010.10.10-zh_cn-win32_msi-x86-32bit.exe)。至于linux或其他版本的安装可能稍微繁琐，下次再介绍。）或者在线安装[http://www.packetix.net/en/secure/install/](http://www.packetix.net/en/secure/install/)如果你运气不够好，这两种方式你都访问不了，别担心，再介绍一个国内下载站：[http://www.xdowns.com/soft/1/66/2010/Soft_57744.html](http://www.xdowns.com/soft/1/66/2010/Soft_57744.html)至少这些链接在我写此文时是可用的。**配置文件下载地址：**[http://www.packetix.net/en/secure/secure.vpn](http://www.packetix.net/en/secure/secure.vpn)（用来配置客户端连接服务端的相关信息，这个服务端是在日本，所以如果配置连接成功后，你的IP地址将会变成国外的）

# 2.配置

双击安装客户端之后，在菜单选择连接--->导入VPN连接设置，选中刚下载的配置文件secure.vpn即可。[![](http://www.ichatter.orguploads/2011/05/icon1.png)](http://www.ichatter.orguploads/2011/05/icon1.png)这时配置文件就被导入了，右键选中窗口上方的Secure配置文件，然后点击“连接”，此时会弹出一个确认框，有不少英文和日文，是一些协议确认的信息，不管它，点确认，稍等一会就根据配置信息给你虚拟出来了一个VPN Client网卡，并自动为你成功连接服务端，获得了一个分配的IP地址。[![](http://www.ichatter.orguploads/2011/05/icon2.png)](http://www.ichatter.orguploads/2011/05/icon2.png)（正在创建虚拟网卡）[![](http://www.ichatter.orguploads/2011/05/icon4.png)](http://www.ichatter.orguploads/2011/05/icon4.png)（正在分配IP地址）[![](http://www.ichatter.orguploads/2011/05/icon5.png)](http://www.ichatter.orguploads/2011/05/icon5.png)（进入CMD查看自己电脑的IP地址变化）至此，自己电脑的VPN已经成功连接服务端，现在就可以用这个IP地址去访问许多被墙的网站了，facebook、twitter、youtube的界面久违了吧！呵呵！当然，还有许多被误杀的国外技术网站，尽情爽吧，let's enjoy it!

# 3.补充

如果你下载不了配置文件secure.vpn，也可以自己新创建一个VPN连接，同样可以达到以上效果，配置信息如下<table style="width: 414px; height: 154px;" ><tbody ><tr >
<td width="340" >设置参数
</td>
<td width="200" >内容
</td></tr><tr >
<td >主机名(H)
</td>
<td >public.softether.com
</td></tr><tr >
<td >端口号(P)
</td>
<td >**443**
</td></tr><tr >
<td >虚拟Hub名(V)
</td>
<td >**PUBLIC**
</td></tr><tr >
<td >认证类型(T)
</td>
<td >**匿名身份验证**
</td></tr><tr >
<td >用户名(U)
</td>
<td >**public**
</td></tr></tbody></table>-----------------------------------------------------------------------------------------------------------------------------------最后是一些相关资料，希望对您有用：[http://auv.me/packetix-vpn-tips-1/](http://auv.me/packetix-vpn-tips-1/)[http://gnrsu.cn/archives/7473](http://gnrsu.cn/archives/7473)
