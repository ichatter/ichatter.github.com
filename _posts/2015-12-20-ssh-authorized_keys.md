---
layout: post
title: "ssh免密码登陆的权限问题"
description: ""
category: 工作
tags: []
---
##基本流程
客户端client通过SSH连接服务端server，要实现免密码登陆，则需要采用公私钥的形式来实现，即在本地client存放私钥，在服务端server存放公钥，这是基本思路，大致流程如下：

1. 生成公私密钥对
    密钥对在client或server都可以生成，类型有rsa和dsa等类型，以client生成rsa为例：
```
ssh-keygen -t rsa
```
根据提示完成后，可以看到：
```
ls ~/.ssh/
id_rsa  id_rsa.pub  known_hosts
```
生成了id_rsa及id_rsa.pub两个文件。
2. 上传公钥到server端
    将公钥文件id_rsa.pub上传到server端:
```
scp ~/.ssh/id_rsa.pub yzy@myvps.com:.ssh/
```
登陆server端后：
```
cd ~/.ssh
mv id_rsa.pub authorized_keys
```
3. 从client端连接server端：
    此时只需：
```
ssh myvps.com
```
即可登陆成功

##权限配置
完成上面的基本流程后，如果client连接server时还需要提示输入密码，则很可能是server端的ssh相关文件及文件夹的权限配置有问题。修改权限如下：
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod go-w ~
```
重新`ssh myvps.com`连接，即可直接登陆成功！

**补充**
    server端的权限限制，应该是为了保证安全而采取的措施，具体来说，在当前用户目录下，对于~、~/.ssh、~/.ssh/authorized_keys，需对group和others用户禁用write权限，即可实现client端免密码登陆。所以上面的权限适当放宽，改成如下也可以：
```
chmod 755 ~/.ssh
chmod 655 ~/.ssh/authorized_keys
```
