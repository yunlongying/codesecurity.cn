---
title: Docker常用命令
date: 2020-06-21 19:18:47
tags:
    - 随笔
---

# 安装docker服务
>yum search docker  #查找docker
yum install -y docker   #安装
systemctl start docker
systemctl status docker
docker version
docker -v         #查看版本
docker info     #查看docker信息

<!--more-->

# 镜像管理
查看镜像列表:
>docker images
docker image ls

导出镜像:
>docker image save centos > docker-centos6.9.tar.gz
导入镜像:
docker image load -i docker-centos6.9.tar.gz

删除镜像:
>docker image rm centos:latest
docker image rm 578c3

搜索镜像
>docker search + 镜像名字
docker search [REPOSITORY[:TAG]]； #搜索镜像，
docker tag nginx:latest 10.0.0.11:80/nginx:latest

推送指定镜像到docker镜像源服务器
>docker push 10.0.0.11:80/nginx:latest

获取镜像 (下载)   
>docker pull image_name
docker pull centos:6.8（没有指定版本，默认会下载最新版） 私有仓库pull    docker pull daocloud.io/huangzhichong/alpine-cn:latest
docker history image_name   显示一个镜像的历史
docker build -t <image-name> .   *(点一定不能去掉)#使用当前目录下的Dockerfile构建镜像
docker的容器管理
docker pull                  #拉取镜像，
docker build -t vuenginxcontainer .  #运行命令（注意不要少了最后的 “.” ）： -t 是给镜像命名，. 是基于当前目录的 Dockerfile 来构建镜像。
给源中镜像打标签:

# 容器管理
>docker run（创建并运行一个容器） 
-d 放在后台 
-p 端口映射 :docker的容器端口
-P 随机分配端口
-v 源地址(宿主机):目标地址(容器)
-it 分配交互式的终端 
--name 指定容器的名字 
/bin/sh覆盖容器的初始命令
举例：
docker run --name 容器名 -d -p 3306:3306 mysql  docker 启动容器
docker run image_name  #启动容器***
docker run -d -p 80:80 nginx:latest
docker run -it --name centos6 centos:6.9 /bin/bash 

>docker stop container_id  停止容器
docker kill container_name   杀死容器
docker ps (-a -l -q)    查看容器列表
docker container rm  'docker ps -a -q'   删除所有容器
docker rm -f  'docker ps -a -q`      #删除所有容器
docker ps #查看所有正在运行容器 
docker ps -a  #查看容器列表
docker ps -a -q #查看所有容器ID 
docker stop $(docker ps -a -q) #stop停止所有容器 
docker rm $(docker ps -a -q)  # remove删除所有容器
docker start $(docker ps -a -q) #开启所有容器
docker rm 容器ID或者容器名       #删除容器实例
docker rmi -f ID                       #删除镜像
docker exec -it 77cd6bef4dc9 /bin/bash   #进容器
docker exec -it <CONTAINER-NAME> OR <CONTAINER-ID> env  #查看容器环境变量
docker exec -it xxx bash    #进入容器命令
docker exec -it xxx sh    #进入容器命令
docker exec -it xxx /bin/sh    #进入容器命令
docker stop containerId #停止容器 
docker start containerId #启动容器
docker start/stop container-id||container-name 开启/停止 指定容器id或者容器名称的容器
docker run -d -p 80:80 -v /opt/xiaoniao:/usr/share/nginx/html nginx:latest
docker run -p 3000:80 -d --name vueApp  vuenginxcontainer  #运行容器
docker logs container-name/container-id     #查看容器日志
docker logs -f 容器名或ID | grep fail | more  #查看容器日志
docker ps | grep ${CONTAINER_ID}    #查看容器状态
docker commit ID new_image_name     #镜像打包 (保存对容器的修改)
docker commit -m="提交的描述信息" -a="作者" 容器id  要创建的目标镜像名:[标签名]
docker inspect <id/container_name>   #查看容器内部详情细节
docker inspect +容器ID      #获取容器的具体信息
docker login #登录
docker port dotnetapicontains     #查看容器端口配置
docker attach   xxx            #进入容器命令
docker-enter +容器id    #进入容器 
 ctrl+d 、exit                       #退出容器
 Ctrl+P+Q     #退出而不关闭容器
docker restart +容器id  #重启容器 
docker cp containerName:etc/mysql /home/mysql  把容器目录拷贝到宿主机

