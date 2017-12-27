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





