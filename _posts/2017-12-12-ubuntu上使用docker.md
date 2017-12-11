今天要总结一下近期来使用学习docker的经验

****
#### Docker的安装
>sudo apt-get update

安装  这是用比较新的方法 也可以用
>  sudo apt-get install docker.io

但是这个下载的版本会比较低  不建议这样下载

> sudo apt-get install curl

> curl -sSL  https://get.docker.com/  | sh

下载完毕启动Docker的守护进程

>sudo service docker start

检查Docker是否安装成功
> sduo docker run hello-world 

如果提示Hello from Docker! 

当然是里面有提示，并不是一开始的提示就是这个，说明安装成功了提示（Ubnable to find image 'hello-world lateset' locally）

如果不想总是输入sudo 可以输入以下命令解决
>sudo usermod  -aG docker yuzhipeng

****

#### 体验Docker
使用docker指令创建 启动几个Docker应用，比如WordPress,gitlab服务


* 通过搭建WordPress来试试Docker

这几个应用下载的话会比较慢 要等几分钟

> sudo docker run --name db --env MYSQL_ROOT_PASSWORD=example -d mariadb

> sudo  docker run --name MyWordPress  --link db:mysql -p 8080:80 -d wordpress 

通过ifconfig查看本机的IP地址，在本机地址后加上端口号8080,会有惊喜。简直不能更爽

这个我在京东云上部署了一下，感觉用起来很快，部署效果非常好，比之前部署项目快的过，真的是几条命令就可以跑起来服务，厉害了  [访问](http://116.196.76.223:8079/)


* 搭建Gitlab服务
首先启动postgresql

```sudo docker run --name gitlab-postgresql -d --env 'DB_NAME=gitlabhq_production' --env 'DB_USER=gitlab' --env 'DB_PASS=password' sameersbn/postgresql:9.4-12
```
然后启动redis
```
sudo docker run  --name gitlab-redis -d sameersbn/redis:latest 
```
最后启动gitlab

```sudo docker run --name gitlab -d --link gitlab-postgresql:postgresql --link gitlab-redis:redisio --publish 10022:22 --publish 10080:80 --env  'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' sameersbn/gitlab:8.4.4
```
后来就可以访问端口10080即可进入gitlab，确实是太好用了，这时候用户名为root，密码是5iveL!fe   连接（如果可用） [点击进入](http://116.196.76.223:10080)

* 项目管理系统 Redmine
要使用saneersbn/redmine的镜像。两条指令
```sudo docker run --name=postgresql-redmine -d --env='DB_NAME=redmine_production' --env='DB_USER=redmine' --env='DB_PASS=password' sameersbn/postgresql:9.4-12

 docker run --name=redmine -d --link=postgresql-redmine:postgresql --publish=10083:80 --env='REDMINE_PORT=10083' sameersbn/redmine:3.2.0-4
```

**** 
10分钟小任务认识Docker

版本号
>docker version

查找镜像 (镜像的全称是  ```<username>/<repository>```)

>docker search tutorial

下载镜像
>docker pull learn/tutorial 

>docker ps -l

用docker run来创建和运行docker容器(docker可以创建容器并在容器中运行指定的命令)
>docker run learn/tutorial  echo "hello world"

修改容器(安装ping)
>docker run learn/tutorial apt-get install -y ping

通过docker ps -l找出安装过ping包的容器的ID号，
>docker ps -l

然后将容器提交为新的镜像，这时会返回一个新的ID便是新生成的镜像的ID
>docker commit 09c2e9353f01 learn/ping

在基于新镜像的容器中执行 ping www.google.com这条指令(新镜像要使用全名 learn/ping)
> docker run learn/ping ping www.google.com

查询容器信息

>docker ps    查询所有运行的容器

>docker inspect a102  查看单个容器的信息(根据docker ps列出的容器名，选取前三四个字符即可)

新镜像上传仓库

>docker images  查询本机的镜像列表

查询结果中有
```
REPOSITORY                                                                      TAG                 IMAGE ID            CREATED             SIZE
learn/ping                                                                      latest              714c84471b9f        11 minutes ago      139MB
```
所以把镜像推送到Docker官仓

> docker push learn/ping

****

#### Docker常用词

查看所有的镜像
>docker images 

查看相关进程
>sudo docker ps

查看版本
>docker version

ps只是看一些大体容器内容 inspect是看容器的详细内容
>docker ps 

>docker inspect gitlab

>docker push michelesr/ping 

****
#### docker基础概念与常用命令
1.仓库、镜像、容器

2.基本指令

docker指令操作对象主要针对四个方面：

 1 针对守护进程的系统资源设置和全局信息的获取： docker info, docker deamon
 2 针对Docker仓库的查询，下载 ： docker search, docker pull
 3 针对docker镜像的查询，创建与删除: docker images, docker build
 4 针对docker容器的查询，创建，开启，停止: docker ps, docker run
 5 支持赋值，变量解析，嵌套

>docker+命令关键字(command)+一系列参数([arg  ])

例如
>docker run --name MyWordPress --link db:mysql -p 8080:80 -d wordpress

基于wordpress镜像创建容器MyWordPress,通过docker ps可以查到名字MyWordPress,使用的镜像是woedpress的容器


获取帮助
>docker command --help


*****
#### 


