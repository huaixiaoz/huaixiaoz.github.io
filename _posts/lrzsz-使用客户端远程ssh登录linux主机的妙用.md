title: lrzsz 使用客户端远程ssh登录linux主机的妙用
date: 2014-08-31 10:51:39
tags: lrzsz
---

###项目地址####
[lrzsz下载](https://ohse.de/uwe/software/lrzsz.html)

> 官方作者的介绍： `lrzsz is a unix communication package providing the XMODEM, YMODEM ZMODEM file transfer protocols. lrzsz is a heavily rehacked version of the last public domain release of Omen Technologies rzsz package, and is now free software and released under the GNU General Public Licence.`

###使用###

使用`xshell`等远程登录`linux`主机，可以直接把文件拖入xshell窗口，传输文件，方便易用。

###安装###

- 常用发行版相图方便就直接从源安装吧，升级维护方便。 
```apt-get install lrzsz```
```yum install -y lrzsz```
- 源码编译也可以； `下载`[源码](https://ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz)=> `./configure` => `make` => `make install`

