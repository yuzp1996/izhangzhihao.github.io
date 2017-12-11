今天要总结一下近期来使用学习docker的经验

****
#### Docker的安装
>sudo apt-get update

安装  这是用比较新的方法 也可以用
>  sudo apt-get install docker.io

但是这个下载的版本会比较低  不建议这样下载

> sudo apt-get install curl

```
 curl -sSL  https://get.docker.com/  | sh
```
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

--name参数创建了两个Docker容器，db和MyWordPress 通过docker ps可以查到名字


通过ifconfig查看本机的IP地址，在本机地址后加上端口号8080,会有惊喜。简直不能更爽

这个我在京东云上部署了一下，感觉用起来很快，部署效果非常好，比之前部署项目快的过，真的是几条命令就可以跑起来服务，厉害了  [访问](http://116.196.76.223:8079/)


* 搭建Gitlab服务
首先启动postgresql

```
sudo docker run --name gitlab-postgresql -d --env 'DB_NAME=gitlabhq_production' --env 'DB_USER=gitlab' --env 'DB_PASS=password' sameersbn/postgresql:9.4-12

```

然后启动redis

```
sudo docker run  --name gitlab-redis -d sameersbn/redis:latest 
```
最后启动gitlab

```
sudo docker run --name gitlab -d --link gitlab-postgresql:postgresql --link gitlab-redis:redisio --publish 10022:22 --publish 10080:80 --env  'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' sameersbn/gitlab:8.4.4

```
后来就可以访问端口10080即可进入gitlab，确实是太好用了，这时候用户名为root，密码是5iveL!fe   连接（如果可用） [点击进入](http://116.196.76.223:10080)

* 项目管理系统 Redmine
要使用saneersbn/redmine的镜像。两条指令
```

sudo docker run --name=postgresql-redmine -d --env='DB_NAME=redmine_production' --env='DB_USER=redmine' --env='DB_PASS=password' sameersbn/postgresql:9.4-12

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
#### Docker容器管理

##### 容器标识符

每一个容器创建后都有一个CONTAINER ID作为容器的唯一的标识符，后续对容器的操作都是通过CONTAINER ID来完成，一般docker ps展示前16位，如果查询所有可以使用docker ps --no-trunc

查询容器状态
```
 docker ps -a | grep d0131ae38d9d
```

停止容器
> docker stop d0131ae38d9d

运行容器
> docker start d0131ae38d9d

CONTAINER ID比较难记忆，所以创建容器时会有--name来给容器起一个别名 所以用别名也可以


查询容器信息(使用-f时可以用golang的模板提取出制定信息)
>docker inspect -f {{.NetworkSettings.IPAddress}} MyWordPress

查询日志
>docker logs MyWordPress

查询容器占用的系统资源
> docker stats MyWordPress


##### 容器内部命令

* 单容器

需求：登入Docker容器执行命令

方案： Docker提供原生的方式支持登入docker exec
>docker exec + 容器名 + 容器内执行的命令

查看MyWordPress容器内的进程
> docker exec MyWordPress ps aux

在容器内连续执行命令 加上 -it 参数即可，相当于以root身份登入，完成后通过 exit 退出
> docker exec -it MyWordPress /bin/bash


* 多容器

Docker理念是‘一个容器一个进程’，如果一个服务由多个进程组成，就创建多个容器组成一个系统

在同一个主机下，docker run命令提供 ‘--link’建立容器之间的互联，而且他们是有顺序的，比如如果创建B容器的时候需要使用 ‘--link containerA’，那么创建B的时候，那么
在创建B容器的时候A容器必须已经创建且已经启动

对于WordPress,数据库容器(db)要先于Apache容器(MyWordPress)启动，所以启动方式应该是
>docker start db
>docker start MyWordPress

停止WordPress服务，先停止Apache容器（MyWordPress）,再停止数据库容器（db），或者同时停止这两个容器
>docker stop db MyWordPress


* Docker compose
但是如果比较多的容器启动的话就会比较麻烦，本着偷懒的原则，Docker提供了一个容器编排工具--Docker Compose,允许用户用一个模板定义一组相关联的应用容器，会根据配置模板的‘--link’参数对启动的优先级进行排序，只用‘docker-compose up’一条语句即可将一个服务中多个容器依次创建与启动

安装 Docker compose:(在<https://github.com/docker/compose/releases/download>这里可以找到随时更新的命令)
```
curl -L https://github.com/docker/compose/releases/download/1.18.0-rc2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

使用1：

1. 首先关掉WordPress的两个容器
> docker stop db MyWordPress

2. 创建一个文件夹 ~/wordpress，文件夹下创建docker-compose.yml的文件：
   wordpress:
     image: wordpress
     links:
       - db:mysql
     ports:
       - 8080:80
     db:
       image: mariadb
       environment:
         MYSQL_ROOT_PASSWORD: example
  作用:创建了两个容器wordpress跟db,使用image指定了使用的镜像

3. 创建启动WordPress服务

> cd ~/wordpress &&docker-compose up

4. 检测
 开另一个命令终端， 输入docker ps查看容器
  
这是启动停止就变的简单了，

启动：
>docker-compose start

停止：
> docker-compose stop

5. 删除原来的容器

查看所有容器（删除与未删除的）
> docker ps -a
 
删除(根据名字(NAMES)来删除)
> docker rm MyWordPress db

硬删的话就用 docker rm -f


使用2：gitlab改造
原来的用法
```
首先启动postgresql

sudo docker run --name gitlab-postgresql -d --env 'DB_NAME=gitlabhq_production' --env 'DB_USER=gitlab' --env 'DB_PASS=password' sameersbn/postgresql:9.4-12


然后启动redis

sudo docker run  --name gitlab-redis -d sameersbn/redis:latest 

最后启动gitlab

sudo docker run --name gitlab -d --link gitlab-postgresql:postgresql --link gitlab-redis:redisio --publish 10022:22 --publish 10080:80 --env  'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' sameersbn/gitlab:8.4.4

```

改造结果
```
postgresql:
  image: sameersbn/postgresql:9.4.4-12
  enviromnment:
    - DB_USER=gitlab
    - DB_PASS=password
    - DB_NAME=gitlabhq_production

redis:
  image: sameersbn/redis:lastest

gitlab:
  image: sameersbn/gitlab:8.4.4
  links:
    -redis:redisio
    -postgresql:postgresql
  ports:
    - "10080:80"
    - "10022:22"
  environment:
    - GITLAB_PORT=10080
    - GITLAB_SSH_PORT=10022
    - GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alphaumeric-srting
```

创建一个项目~/gitlab，然后将上面放在docker-compose.yaml并放在该项目下

删除旧容器： 
> docker rm -f gitlab gitlab-redis gitlab-postgresql

启动新容器组
> cd ~/gitlab/ && docker-compose up -d



#### Docker镜像管理

docker镜像查看
>docker images -a

查看镜像分了多少层
> docker history sameersbn/redis

方式：通过对容器的可写层修改来生成新镜像

问题：如果底层镜像出了问题，或者达到两个文件系统允许的最多层数(aufs最多支持128层)

解决方式： dockerfile