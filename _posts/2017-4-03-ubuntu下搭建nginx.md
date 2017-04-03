# ubuntu 下配置nginx与uwsgi #
#####  (做此工作前应该先保证自己的代码能够在django提供的服务器的环境下跑起来，在服务器上要装django，mysql,还有python-mysqldb(sudo apt-get install python-mysqldb)),          测试的话可以用django的服务器跑起来（python manage.py runserver 0.0.0.0:8000） 这样所有的公网里的电脑就可以通过8000端口访问你的网站   #####

#### 配置工作

1. 安装nginx
 


        sudo apt-get install nginx
        
        
        
2. 安装uwsgi：


        sudo apt-get install uwsgi uwsgi-plugin-python

或者使用

        pip: sudo pip install uwsgi
	
	
测试uwsgi
在你的机器上写一个test.py

    	# test.py
    	
    	def application(env, start_response):
	        start_response('200 OK', [('Content-Type','text/html')])
	        return "Hello World"
	    
	     
然后执行shell命令：
	
        # 测试用代码
        uwsgi --http :8001 --wsgi-file test.py

***
	
访问网页：

http://127.0.0.1:8001/

看在网页上是否有Hello World

***
#### 错误处理 ####
	2.1 如果报chdir() nosuch file or directory[core/uwsgi.c line 2581]之类的错：
	解决 ：只要安装apt-get install build-essential python-dev就可以了
	
	2.2 如果报错 uwsgi: option '--http' is ambiguous; possibilities: '--http-socket' '--http-socket-modifier2' '--http-socket-modifier1' 
		getopt_long() error ;
	解决：uwsgi --http-socket :8001 --plugin python --wsgi-file test.py
		
原因：
   使用 uwsgi 时都会碰到uwsgi: unrecognized option '--uwsgi-file'如 --module , --wsgi-file , --callable 等，需要在上面那些未识别选项前加上 --plugin python 来告诉 uWSGI 我在使用 python 插件，后面那些选项你 用python 插件去解析。




3、配置django：

编写django_wsgi.py文件，将其放在与文件manage.py同一个目录下。    
注意：
编写文件时需要注意语句os.environ.setdefault。
比如，如果你的项目为myNote，则你的语句应该是

     os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myNote.settings")

(注意缩进)
    
    #!/usr/bin/env python
	# coding: utf-8

	import os
	import sys

	# 将系统的编码设置为UTF8
	reload(sys)
	sys.setdefaultencoding('utf8')

	os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myNote.settings")

	from django.core.wsgi import get_wsgi_application
	application = get_wsgi_application()
	#from django.core.handlers.wsgi import get_wsgi__application #不同版本的不同
	#application = get_wsgi__application



4、连接django和uwsgi，实现简单的WEB服务器：

假设你的Django项目的地址是/home/wen/myNote，
 可以执行以下命令：
 
     uwsgi --http-socket :8000 --chdir /home/wen/myNote --plugin python --module django_wsgi
 
 （PS：这是通过uwsgi启动服务，后面是通过nginx启动服务，所以此时通过8000可以访问）

为了实现Nginx与uWSGI的连接，两者之间将采用soket来通讯方式。
在本节中，我们将使用uWSGI配置文件的方式来改进uWSGI的启动方式。
假定你的程序目录是 /home/wen/myNote
我们将要让Nginx采用8077端口与uWSGI通讯，请确保此端口没有被其它程序采用。
新建一个XML文件：
djangochina_socket.xml，将它放在 /home/wen/myNote目录下：
	
	<uwsgi>
	    <socket>:8077</socket>
	    <chdir>/home/wen/myNote</chdir>
	    <module>django_wsgi</module>
	    <processes>4</processes> <!-- 进程数 --> 
	    <daemonize>uwsgi.log</daemonize>
	</uwsgi>
	
在上面的配置中，我们使用 uwsgi.log 来记录日志，开启4个进程来处理请求。
这样，我们就配置好uWSGI了。
	



5、 配置Nginx:

我们假设你将会把Nginx程序日志放到你的目录下/var/log/error.log，(请确保该目录存在。)

我们假设你的Django的static目录是/home/work/src/sites/testdjango1/testdjango/collectedstatic/，(要检查setting里面是否有static的路径，然后要把static综合起来用

    python manage.py collectstatic

这一步在将网站部署到服务器上是很重要的一步，收集静态文件到指定的static文件下，这样在debug=false的时候才能寻找到正确的静态文件

[说明](http://www.ziqiangxuetang.com/django/django-static-files.html)

我们假设你的域名是 127.0.0.1 （在调试时你可以设置成你的机器IP）

我们假设你的域名端口是 80（在调试时你可以设置一些特殊端口如 8070）

基于上面的假设，我们为conf/nginx.conf添加以下配置(命令locate nginx.conf可以找到conf文件的路径,当你执行 nginx -t 得时候，nginx会去测试你得配置文件得语法，并告诉你配置文件是否写得正确，同时也告诉了你配置文件得路径：)
	(应该放在http{}，应该放在https的最后)
	
    server {
        listen   8001;
        server_name 127.0.0.1;
        access_log /var/log/access.log;
        error_log /var/log/error.log;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        location / {
         include        uwsgi_params;
         uwsgi_pass     127.0.0.1:8077;
        }
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        location /static/ {
        alias  /home/wen/myNote/webStatic/; (这里有个分号,一定要弄明白这个地方到底是什么比如/home/yuzhipeng/桌面/myblog/static)
        index  index.html index.htm;
        }
}


（nginx -t  命令可以检查是否配置正确，通过配置后才能监听8001；貌似是nginx就是监听8001的）





6、Nginx+uWSGI+Django的实现方式

在完成上面配置后，需要按以下步骤来做：

6.1..重启Nginx服务器，以使Nginx的配置生效。

    nginx -s  reload

6.2..重启后检查Nginx日志是否有异常。
启动uWSGI服务器

    cd /home/wen/myNote/
    uwsgi --plugin python -x djangochina_socket.xml

6.3..检查日志 uwsgi.log 是否有异常发现。


访问服务
基于上面的假设你的域名是127.0.0.1
因此，我们访问127.0.0.1，如果发现程序与单独使用Django启动的程序一模一样时，就说明成功啦！
6.3..如果打不开网页，可能是端口被占用，或者防火墙的问题


关闭服务的方法

6.4..将uWSGi进程杀死即可。
    sudo进入root 
    命令killall -9 nginx 


 # 经验： 
 
        
1.有时提示错误是权限的问题  要记者使用sudo或者su命令  如果使用此命令  则在整个过程中都要使用这个命令 
    
    
2.因为是成为生产环境 ，所以要把django的dubug关掉，并设置能够访问 host-什么里面改掉，【*】使他都可以访问
    
3.看看error里的错误  亲爱的 吗的