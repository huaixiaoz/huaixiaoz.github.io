title: wordpress多用户技巧以及国内访问优化
date: 2014-08-31 10:44:58
tags: wordpress
---

###wordpress如何在一个空间使用一个数据库来存储多份数据？###

原理很简单： 根据表头的区别，绑定多个域名到一个ip，根据域名不同而使用不同表头的表。

修改`wp-config.php`
``` 
if ($_SERVER["HTTP_HOST"]=="xxx1")
 {
     $table_prefix  = 'wp_';
 }
else if($_SERVER["HTTP_HOST"]=="xxx2")
{
    $table_prefix  = 'wptt_';
}
else
{
    $table_prefix  = 'wp_';
}
```
> `xxx1`和`xxx2`是域名， 如 `i.huaixiaoz.com`本站的域名， 或者 `test.huaixiaoz.com`

###Google fonts###
google越来越在中国不受欢迎了、、
[访问google方法](http://i.huaixiaoz.com/hack/google_in-1.html)
- 修改wordpress: `wp-includes/script.php`
>大概在533行的位置；
    替换`$open_sans_font_url =
    "//fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,600italic,300,400,600&subset=$subsets";`
    为`$open_sans_font_url =
    "//fonts.useso.com/css?family=Open+Sans:300italic,400italic,600italic,300,400,600&subset=$subsets";`
    其实也就是吧连接中的`googleapis`替换为`useso`: 国内360的镜像源。
    - 对于主题中的`googleapis`同理，替换即可。 一般在主题的`function.php`文件中。

    对于能访问主机也就是能ssh登录的用户来说，直接登录替换即可。
    命令如下：
    `grep googleapis ./* -R |awk -F: {'print $1'} | xargs sed -i  -e 's/googleapis/useso/g'`

                          当然，你也可以使用插件来禁用googleapis。

###wordpress修改为中文办法###

- 修改`config.php`中 
```
define('WPLANG', 'zh_CN');
```
更新即可。
-     不行的话，就手动复制中文语言包到`wp-content/languages`下。
