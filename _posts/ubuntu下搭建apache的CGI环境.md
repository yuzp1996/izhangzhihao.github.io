---
layout: post
title: CGI for python
description: how to bulid apache CGI
keywords: CGI
---

# CGI for python

## 最近在学python 用到了CGI技术，这是一个比较底层的技术，还是很好玩的，就是配置上有些绕，最终在好的教程下算是成功搭建好了

##  因为自己用的是nginx感觉很是难配， 所以索性改了apache,以下就是教程

参考了两个大神的博客，地址如下
https://zmobi.github.io/2016/08/31/python-cgi-setting-with-apache24.html
http://blog.sina.com.cn/s/blog_4aa65a3f01013az9.html

###  重点来了

咸来说如何安装apache吧
1. Ubuntu提供了强大的apt-get install命令，在终端输入：sudo apt-get install apache2    
      Apache的启动和停止文件是：/etc/init.d/apache2
       启动命令：sudo apache2ctl -k start
       停止命令：sudo apache2ctl -k stop
       重新启动命令：sudo apache2ctl -k restart(sudo /etc/init.d/apache2 restart)


     顺便提下查看Apache的版本，使用的命令是：apache2ctl -v 结果会显示：
      Server version: Apache/2.2.17 (Ubuntu)
      Server built:   Nov  3 2011 02:13:18



#  HERE WE GO
Ubuntu系统

在ubuntu16.04环境下，使用sudo apt install apache安装，在内网中调试使用时，需要做的更改有：

1、添加ServerName

> echo ServerName 127.0.0.1 >> /etc/apache/apache.conf

2、修改支持cgi的配置

# 在配置 sites-available/000-default.conf 中添加
> ScriptAlias /cgi-bin/ /var/www/html/cgi-bin/

# 在加载模块 mods-available/mime.load  中添加
> LoadModule cgi_module /usr/lib/apache2/modules/mod_cgi.so

3、Apache常用命令
```
apache2ctl [ start | stop | restart | reload ]
a2enconf 启用配置  
a2enmod  启用模块   
a2ensite 启用网站
```
同理，禁用的话，则把en改为dis

还有比较重要的一条， 那就是要给cgi程序相应的权限
##  修改cgi程序的权限。CGI程序属性一定要设为可运行（755），而与CGI有关的HTML文件的目录如果要被CGI程序写入，其权限一定要设为可写（666）。其命令为：sudo chmod 755 ~/simple.cgi

当然修改后还是要重启服务的，这样我们的配置文件才会生效
##  重启Apache服务：sudo /etc/init.d/apache2 restart

至此，通过浏览器输入相应的url应该能顺利的访问到cgi程序了，如
```   
http://localhost/cgi-bin/simple.cgi
```
如果还不能的话，估计就是cgi程序本身的问题了，可以通过查找Log，发现其中的错误。Apache的log放置在/var/log/apache2下。

















