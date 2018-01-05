---
title: docker命令记录
date: 2017-12-20 10:35:29
tags:
---
# docker基本命令
* brew cask install docker:Homebrew 的 Cask安装docker
* docker --version:查看版本
* docker info:查看docker是否存在，返回所有容器+镜像
* docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]:获取镜像
> docker pull ubuntu:16.04

* docker run:创建容器
> docker run -i -t ubuntu /bin/bash

基于ubuntu镜像创建容器，在容器中执行/bin/bash命令启动Bash shell。

* /etc/hosts:linux 的/etc/hosts是配置ip地址和其对应主机名的文件
> cat /etc/hosts:查看/etc/hosts

* ps -aux:查看容器中运行进程
* apt-get update && apt-get install vim:软件安装（vim）
* exit:返回到宿主机命令行（！只有在指定的/bin/bash命令处于运行状态时，容器才会相应的处于运行状态）
* docker ps:正在运行的容器;docker ps -a:所有容器列表;docker ps -l:最后一次运行的容器
* docker run --name xxx -i -t ubuntu /bin/bash:给创建的容器指定名称
* docker rm xxx:删除容器；docker rm `docker ps -a -q`:删除所有容器，<b>运行中的容器是无法删除的。</b>
* docker start xxx:重新启动已经停止的容器
* docker attach xxx: 附着到容器上，运行一个交互式回话shell（需要按一下enter）
* whereis xxx:查找xxx
* sudo docker run --name aofo -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done":创建守护式容器
守护式容器（daemonized container）:长期运行的容器,没有交互式会话，适合运行应用程序和服务,大多数时候是需要以守护式来运行容器。 -d 参数会让容器在后台运行
* docker logs xxx:查看容器日志; docker logs -f xxx:添加-f参数来监听日志，ctrl+c来退出监控日志；docker logs --tail 10 xxx:获取日志的最后10行；docker logs --tail 0 -f xxx:跟踪某个容器的最新日志而不必读取整个日志文件；docker logs -ft xxx:-t参数为每条日志加上时间戳。
* docker top xxx:查看容器内的所有进程
* docker exec:在容器内部额外启动新进程
> docker exec -d daemon aofo touch /etc/new_config_file:-d标志标明要运行一个后台进程，后面指定要在内部执行这个命令的容器名,在容器内创建了一个空文件夹new_config_file。
> docker exec -t -i aofo /bin/bash:在容器中创建一个新的bash会话

* docker stop xxx:停止容器
* docker kill xxx:快速停止容器
* docker run --restart=always --name xxx -d ubuntu /bin/bash -c "while true;do echo hello world;sleep 1;done":除了always，不管退出代码是什么都会重启该容器，--restart=on-failure:5接受一个可选的重启次数参数，即最多重启5次。
* docker inspect:获取更多的容器消息
* docker history xxx:查看镜像是如何构建的
* docker run --name xxx -i -t -w /var/log ubuntu pwd:-w标志在运行时覆盖工作目录，即容器内的工作目录为/var/log

# docker镜像
<b>本地镜像都保存在Docker宿主机的/var/lib/docker目录下。每个镜像都保存在Docker所采用的存储驱动目录下。也可以在/var/lib/docker/containers目录下面看到所有的容器</b>
* docker images:列出镜像
* docker pull xxx:拉取镜像
* docker search xxx:查找所有Docker Hub上公共的可用镜像
* 使用:+tag(标签名)来准确的指定容器的镜像
* docker commit/docker build命令和Dockerfile文件,推荐后者，更灵活更强大。
* docker login:登录到Docker Hub
* 用 commit 命令创建镜像：
    1. docker run -i -t ubuntu /bin/bash：创建一个新容器；
    2. apt-get -yqq update;    apt-get -y install apache2  :安装Apache
    3. 先用exit命令退出容器， 再docker commit 614122c0aabb aoct/apache2：指定了要提交的修改过的容器的ID、目标镜像仓库、镜像名，把这个容器作为一个Web服务器来运行,需要把它当前状态保存下来，就不必每次都创建一个新容器并再次安装Apache；
    4. docker commit -m='A new image' --author='Aomine' 614122c0aabb aoct/apache2:像git一样，在提交镜像时指定更多信息来描述所做的修改
    5. docker run -t -i aoct/apache2 /bin/bash:docker run命令从刚创建的新镜像运行一个容器

* 用Dockerfile构建镜像
```dockerfile
# Version:0.0.1
#指定一个已经存在的镜像
FROM ubuntu 16.04
#该镜像的作者及作者的电子邮件地址
MAINTAINER sansan "15000876069@163.com"
RUN apt-get update&&apt-get install -y nginx
RUN echo 'HI,container' \
    >/usr/share/nginx/html/index.html
EXPOSE 80
```
1、每条指令必须为大写；
2、指令会按顺序从上到下执行；
> docker build -t="sansan/static_web:v1" .

-t选项为新镜像设置了仓库(sansan)和名称(static_web):后为标签，如果没有指定任何标签，会自动为镜像设置一个latest标签，"."告诉docker在本地目录去找Dockerfile,也可以指定一个Git仓库的源地址。
> docker build --no-cache -t="sansan/static_web:v1" .

--no-cache确保构建过程不会使用缓存

* 基于构建缓存的Dockerfile模板
```dockerfile
FROM ubuntu:16.04
MAINTAINER James Turnbull "liaojiafa@qq.com"
ENV REFRESHED_AT 2016-12-05
RUN apt-get -qq update
```
有了这个模版，如果想刷新一个构建，只需要修改ENV指令中的日期。这使DOcker在命令中ENV指令时开始重置这个缓存，并运行后续指令而无需依赖该缓存。也就是说，RUN apt-get update这条指令就会被再次执行，包缓存也会被刷新为最新内容
* 从新镜像启动容器
docker run -d -p 80 --name static_web sansan/static_web nginx -g "daemon off;"
-d选项告诉docker以分离(detached)的方式在后台运行，-p用来控制docker在运行时应该公开哪些端口给外部(宿主机)
docker port 容器ID 容器端口号：返回宿主机映射端口
docker run -d -p 8080:80 --name static_web sansan/static_web nginx -g "daemon off;"
将容器80端口绑定到宿主机8080端口
docker run -d -p 127.0.0.1:8080:80 --name static_web sansan/static_web nginx -g "daemon off;"
将容器80端口绑定到特定的网络接口
docker run -d -p 127.0.0.1::80 --name static_web sansan/static_web nginx -g "daemon off;"
也可以不指定
* Dockerfile指令(docker推荐用数组语法来设置要执行的命令["xx","xx","xx"])
1、CMD(指定容器启动时执行的命令)
2、RUN(容器构建时执行的命令):RUN命令可以覆盖CMD指令
3、ENTRYPOINT(配置容器启动后执行的命令，并且不可被docker run 提供的参数覆盖。每个Dockerfile中只能有一个ENTRYPOINT，当指定多个ENTRYPOINT时，只有最后一个生效。和CMD相似，却有不同。)
4、WORKDIR(镜像创建一个新容器时定义工作目录，如果容器中没有此目录，会自动创建)
5、ENV(在镜像构建过程中设置环境变量):ENV RVM_PATH /home/rvm
6、VOLUME(向基于镜像创建的容器添加卷)
7、ADD(将构建环境下的文件和目录复制到镜像中)
8、COPY(和ADD类似，但不会去做文件提取和解压工作)
9、ONBUILD(为镜像添加触发器trigger)：ONBUILD命令可以在镜像上运行docker inspect查看
10、为apache2镜像构建一个Dockerfile
```dockerfile
FROM ubuntu:16.04 
MAINTAINER sansan "15000876069@163.com"
RUN apt-get update&&apt-get install -y apache2
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ONBUILD ADD . /var/www/
EXPOSE 80
ENTRYPOINT ["/usr/sbin/apache2"]
CMD ["-D","FOREGROUND"]
```
我们可以将这个Dockerfile作为一个通用的web应用程序的模板，基于这个模板来构建web应用程序
<b>ONBUILD ADD . /var/www/:该指令会使用ADD指令将构建环境所在目录下内容全部添加到镜像中的/var/www/目录下</b>
使用模板来构建webapp镜像：
```dockerfile
FROM sansan/apache2
MAINTAINER sansan "15000876069@163.com"
ENV APPLICATION_NAME webapp
ENV ENVIRONMENT development
```
11、将镜像推送到Docker Hub
> docker push xxx

注意：tag的名字斜线前面部分如果跟Docker Hub姓名不一致会导致push失败需要使用docker tag重命名：
> docker push oldname newname

12、自动构建
只需将GitHub或BitBucket中含有Dockerfile文件的仓库连接到Docker Hub即可，只能通过更新GitHub仓库来更新自动构建
13、删除镜像
> docker rmi xxx:该操作只会将本地镜像删除，GitHub依然存在。删除多个：docker rmi xxx yyy

14、运行自己的Docker Registry
 * 从容器运行Registry:docker run -d -p 5000:5000 --restart=always --name registry registry
 * 使用这个Registr为本地镜像打上标签:docker tag 9043319a1906 127.0.0.1:5000/sansan/webapp
 * 将镜像推送到新的Registry:docker push 127.0.0.1:5000/sansan/webapp
 * 将其用于使用docker run 构建新容器:docker run -i -t 127.0.0.1:5000/sansan/webapp /bin/bash

# 在测试中使用docker
1、静态网站：[Docker使用---静态网站测试](http://blog.csdn.net/alive2012/article/details/51871972)

2、web应用程序：[Docker构建并测试Web应用程序](http://blog.csdn.net/alive2012/article/details/51881360)
下载Sinatra Web应用程序源码：
> wget --cut-dirs=3 -nH -r --no-parent http://dockerbook.com/code/5/sinatra/webapp/

保证webapp/bin/webapp文件可以执行
> chmod +x $PWD/webapp/bin/webapp

3、使用docker构建第一个Java应用服务