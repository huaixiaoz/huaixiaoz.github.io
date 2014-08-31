title: Archlinux 下LNMP环境配置以及CGI脚本支持
date: 2014-08-16 07:35:53
categays: linux
tags: acrhlinux
tags: nginx
tags: php
---

###Archlinux简介####

* 良好的软件包管理。 ``pacman``: [about pacmaan](https://wiki.archlinux.org/index.php/Pacman)
* KISS原则 ===> ``Keep it simple and stupid!!``
* 友好的[wiki](https://wiki.archlinux.org/index.php)指导 .
* 软件及时的更新打包，开发社区的氛围等都十分的好。
* 默认的最小化安装，突出的良好的可定制环境。绝对是`linux user & developer`的最佳选择。
* And more ...

Archlinux绝对是linux发行版中比较好的一个。

---------
###LNMP环境安装&配置###

- 安装必要软件包：
```
    #: pacman -Syu // update system and package
    #: pacman -S nginx  mariadb php php-cgi php-fpm fcgiwrap
  
```


<!--more-->


- 配置环境：
```
    #: systemctl enable nginx mysqld php-fpm fcgiwrap
```

####配置php####
``    #: vim /etc/php/php.ini ``

- 设置要开启的必要的php模块， 如果没有开启，弃掉注释即可启用模块。

例如： `mysql`以及`odbc`模块
```
     874 extension=mysqli.so
     875 extension=mysql.so
     876 extension=odbc.so
```
###设置fcgiwrap服务####

``#: vim /usr/lib/systemd/system/fcgiwrap.service ``

修改如下：

```
     1  [Unit]
     2  Description=Simple CGI Server
     3  After=nss-user-lookup.target
     4  
     5  [Service]
     6  ExecStart=/usr/bin/spawn-fcgi -u http -g http -s /run/fcgiwrap.sock -n  -f  3 -- /usr/sbin/fcgiwrap
     7  ExecStartPost=/usr/bin/chmod 660 /run/fcgiwrap.sock
     8  PrivateTmp=true
     9  Restart=on-failure
    10  
    11  [Install]
    12  WantedBy=multi-user.target
```
*`Note: no line number included`*

> <del>#: systemctl enbale fcgiwrap.service</del>

####Nginx Setup####

修改Nginx配置如下： 

- 添加 `php`的 `php-fpm`支持；
```
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
            root           /usr/share/nginx/html;
           # fastcgi_pass   127.0.0.1:9000;
            index index.php;
            fastcgi_pass   unix:/run/php-fpm/php-fpm.sock;
            #fastcgi_pass   unix:/run/fcgiwrap.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
           # include     fastcgi.conf;
            }
```
- 添加 `php`的 `fcgiwrap`支持；

```
location ~ \.(cgi|pl)$ {
            root           /usr/share/nginx/html;
           # fastcgi_pass   127.0.0.1:9000;
            fastcgi_pass   unix:/run/fcgiwrap.sock;
            index index.cgi;
            fastcgi_index  index.cgi;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

```

####重启服务测试####

`` #: systemctl restart php-fpm ``
`` #: systemctl restart fcgiwrap ``
`` #: systemctl restart nginx``

编写一个简单的php脚本：

```
<?php
    phpinfo();
?>
```

保存为--test.php--. 放入 *`/usr/share/nginx/html`* (**如果你修改过默认的网站路径，放入你修改的路径中**)

浏览器远程访问： ``http://*.*.*.*/test.php``

- CGI脚本同上，自测。

有问题请留言,或者发邮件：[hz@huaixiaoz.com](mailto:hz@huaixaoz.com)。 可以参考Archlinux的[*WIKI*](https://wiki.archlinux.org)
