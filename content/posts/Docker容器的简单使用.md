---
title: "Docker容器的简单使用"
categories: [ "默认" ]
tags: [ "docker" ]
draft: false
slug: "18"
date: "2021-06-16 09:34:00"
---

docker是一个应用容器，可以将应用以及该应用的依赖打包到一个可以移植的容器中，应用在这个容器中运行

可以进行转移，修改，版本管理等操作，不用担心应用转移到别的服务器会出现依赖问题

将容器文件转移到别的服务器上，不会影响到容器，一次配置，可以在多个服务器上使用相同的环境



在以前，根本不可能实现或者保障一个服务器是运行多个应用，直到虚拟机的出现，虚拟机可以让空闲的服务器部署新的应用，但是虚拟机依赖于操作系统，操作系统又会占用CPU和内存，以及存储，而且虚拟机的移植性差启动缓慢，直到容器的出现

运行在相同的宿主机的容器是共享一个操作系统的，而且容器具备启动快，移植性强的特点（不用担心本地运行好好的，到生产环境就出一堆问题）

docker的多数项目和工具是使用golang编写的

Docker容器技术高度依赖于Linux内核，不管是在windwos或者Mac OS上都不是真正的容器化（只有在Linux系统上才能实现无虚拟化的真正容器化），都是在虚拟化Linux之后，在Linux虚拟机的基础上实现的

Docker三大件：镜像，容器，仓库

镜像被用来创建容器（而且镜像文件是复用，只可读的）

容器是镜像文件的实例，容器与容器之间是隔离的，容器可被启动，查看，暂停，退出，重启，删除

仓库是集中存储镜像文件的地方，Docker官方仓库：https://hub.docker.com/


Docker官网文档：https://docs.docker.com/


Docker是基于C/S架构（Client-Server）的系统，Docker的守护进程（Docker Daemon）运行在服务端上，客户端通过Docker Client和守护进程建立通信，守护进程可以接收客户端发送的指令并且可以管理服务端的容器

Docker Engine（引擎）会执行客户端发送的指令，来执行Docker内部的工作，每一项工作以Job的形式存在（一个job表示一个工作任务）

如果job（工作任务）需要镜像时，会从Docker Registry中下载获取镜像（如果本地存在则从本地获取，如果没有再从远程仓库获取）

当Docker容器需要创建网络环境时，通过Network driver创建并且配置Docker容器网络环境（通过网桥来暴露对外的端口和ip）

如果需要限制Docker容器运行资源或者执行用户指令时，通过Exec driver完成

网络的配置以及Exec driver的实现都是通过Libcontainer来实现的，Libcontainer是用来管理容器的包，该包基于go语言开发的，像创建容器，删除容器等等都可通过Libcontainer完成




安装docker（环境为ubuntu20.04）

如果之前安装过docker，需要先卸载

sudo apt remove docker docker-engine docker.io containerd runc

docker官方推荐使用docker的存储库来安装或者升级，而不是通过脚本或者通过下载deb软件的方式来安装


安装依赖

sudo apt install ca-certificates curl gnupg lsb-release

配置docker官方的GPG密钥

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

安装docker引擎

sudo apt install docker-ce docker-ce-cli containerd.io

或者

curl -s https://get.docker.com | sh （不推荐使用）


如果需要安装指定版本的docker，可指定docker-ce和docker-ce-cli的版本

查看源仓库中可安装的docker的版本

apt-cache madison docker-ce

启动docker服务

service docker start

关闭docker服务

service docker stop

重启docker服务

service docker restart

检查是否安装成功（创建一个测试容器，并且执行该容器，该容器会返回信息并且退出）

sudo docker run hello-world

或者docker version

查看所有镜像

docker image ls或者docker images


查看所有容器

docker ps

docker ps -a # 输出所有容器（包括未运行的）

docker ps -q #  只输出容器id

docker ps -a -q # 输出所有容器id

docker ps -n 5 # 输出最近创建的5个容器

docker ps -l # 显示最近创建的容器



REPOSITORY：镜像名称，TAG：镜像的标签（一般为该镜像的版本号），IMAGE ID：镜像id，CREATED：镜像创建时间，SIZE：镜像大小

镜像id实质上为该镜像的sha256

注意：如果不指定版本，将默认使用latest版本的镜像

docker system df # 查看Docker的磁盘使用情况


docker stop 容器id（或者容器名都可以） -f # 关闭指定容器，-f为强制删除

docker stop $(docker ps -a -q) # 关闭所有容器

删除指定容器

docker image rm 容器名称 # 例如 docker image rm centos

docker rmi 容器id # 删除指定镜像

docker rmi $(docker images -a -q) # 删除所有镜像

查看所有正在运行的容器

docker container ls或者docker ps

查看所有的容器（包括已经停止运行的）

docker container ls --all


运行指定容器

docker container run 镜像名

或者

docker run 镜像名


启动容器

docker start 容器ID或者容器名

重启容器

docker restart 容器ID或者容器名

停止容器

docker stop 容器ID或者容器名

强制停止运行容器

docker kill 容器ID或者容器名

删除已经停止运行的容器

docker rm 容器ID

注意：rm是删除容器，rmi是删除镜像

docker exec -it 容器ID bash # 在容器中打开新终端，并且启动新的进程，在这个情况下使用exit退出不会导致容器的暂停，而attach会导致容器的暂停，推荐使用exec

docker attach 容器ID # 直接进入容器启动命令的终端，不启动新的进程


docker run -it ubuntu /bin/bash # i表示以交互模式运行容器，t为给容器创建一个伪终端（搭配使用就是交互式容器）

docker run  --name="容器的新名称" 镜像名 # 为容器指定新名称，如果不指定，容器名称默认为镜像名

docker run -d 镜像名 # 后台运行容器并且返回容器id（以守护进程的方式后台运行容器）

docker run -p 80:8080 镜像名 # 运行并且映射指定容器的80端口到物理机的8080端口

docker run -P 镜像名  # 随机端口映射，不常用


获取容器（从镜像源（docker hub）中下载到本地，docker提供大量已经配置好的容器，可以直接下载使用，修改）

docker image pull 镜像名 # 拉取某个镜像

docker search 镜像名 # 搜索查找某个镜像

NAME：容器仓库名称，DESCRIPTION：容器描述，STARS：类似github的star，关注，喜欢
OFFICIAL：是否为官方，AUTOMATED：是否为自动构建



docker cp 容器id:容器内部的路径 目标主机的路径 # 将容器的文件备份（拷贝）到主机中

导出导入容器（备份容器）

docker export 容器id > 文件x.tar # 导出容器

car 文件x.tar | docker import - 镜像用户/镜像名:镜像版本号 # 导入容器


修改容器源

国内容器源

http://hub-mirror.c.163.com # 网易云

https://docker.mirrors.ustc.edu.cn # 中国科技大学

https://mirror.ccs.tencentyun.com # 腾讯云



docker run hello-world --registry-mirror=https://mirror.ccs.tencentyun.com 容器名 # 只对当前指令起作用

找到/etc/default/docker 插入

DOCKER_OPTS="--registry-mirror=https://mirror.ccs.tencentyun.com"

或者

找到/etc/docker/daemon.json，插入

{"registry-mirrors": ["https://mirror.ccs.tencentyun.com"]}  # 可以插入多个源


然后重启服务


如果dockerhub满足不了，可以在hub已存在的容器中进行修改（安装，卸载...）

或者使用Dockerfile，从零开始构建容器

mkdir hallo && cd hallo # 新建一个文件夹，并且进入这个文件夹

nano Dockerfile # 新建个文件，并且修改它

FROM ubuntu:18.04
RUN apt -y update && apt install -y nginx && apt install -y mariadb-server && apt install -y php*

FROM：基础镜像,操作都是基于这个基础镜像，RUM：执行指定指令（shell）

其他参数，MAINTAINER：容器创建者，CMD：当容器启动时执行命令，只能有一条CMD命令，多条执行最后一条，容器run命令会覆盖cmd命令

ENTRYPOINT：当容器启动时执行命令，不可覆盖

USER：指定哪个用户来启动容器

EXPOSE：容器内部开启端口，一般用于容器端口映射到主机端口上

ENV：环境变量

ADD：复制或者下载到容器，请确保本地或者docker仓库中有需要add的文件，例如：

ADD 文件路径（或者url） 目标容器的绝对路径

VOLUME：切换目录用，类似cd

打包容器

docker build -t 容器名 .

这个“.”表示为当前路径

如果正在运行docker，想退出可以使用ctrl+D，例如

在centos中体验Ubuntu系统

例如docker container run -it ubuntu bash

如果想退出docker，可以在docker（run进去的容器）中输入exit

如果想docker中的容器不停止， 快捷键 ctrl+p+q

推送本地容器到docker仓库中

首先需要一个docker仓库账号，例如https://hub.docker.com/或者https://cr.console.aliyun.com/cn-hangzhou/new # 阿里容器镜像服务，

docker login -u 账号 -p 密码 # 登录DockerHub账号

docker push hallo_word:yes

docker pull会默认推送带有latest标签的容器到本地，所以推荐把容器标签修改为latest，或者直接指定要推送哪个容器，精确到标签

docker tag hallo_word:yes hallo_word:latest

推送到第三方仓库（例如：阿里容器镜像服务）

docker pull registry.cn-hangzhou.aliyuncs.com/xxx/xxx:latest # 推送容器到本地

docker login --username=xxx@qq.com registry.cn-hangzhou.aliyuncs.com # 登录的用户名为阿里云账号全名，密码为开通服务时设置的密码，如果是私有仓库，那么需要推送容器到本地之前执行，推送容器到本地之后再执行一次

docker tag 容器id registry.cn-hangzhou.aliyuncs.com/xxx/xxx:latest # 推送到阿里容器仓库


端口映射（实质上并不是docker容器技术实现的，是利用了iptables）

docker配置文件hostconfig.json

docker run -d -p 8000:80 ubuntu  // 将容器的8000端口映射到宿主机上的80端口


---

docker为啥比虚拟机性能好

docker不需要硬件资源的虚拟化，docker的容器的软件是可以直接使用物理机的硬件资源的（更好的利用硬件资源），而且docker是直接调用的宿主机的内核，不需要重新加载操作系统的OS内核（减少不必要的浪费）


---

虚悬镜像（dangling image）：指的是仓库名，标签都是<none>的镜像，一般出现在删除镜像出错时

查找虚悬镜像

docker image ls -f dangling=true

删除虚悬镜像

docker image prune #删除所有dangling image


---




镜像是一种独立，可执行，轻量级的软件包，包含某个软件需要的所有内容，这个镜像包含应用程序，应用的配置依赖，这个镜像实质上就是一个可交付的运行环境

镜像分层系统实现依赖于UnionFS

UnionFS（联合文件系统）：是一种分层，轻量级的高性能文件系统，支持对文件系统的修改作为一次提交来一层一层的叠加，并且将不同的目录挂载到同一个虚拟文件系统下

镜像可通过分层来基于基础镜像来制造各种应用镜像

Docker镜像加载原理：通过UnionFS来一层一层组成，bootfs（boot file system）加载和引导内核，当boot加载完毕，内存使用权从bootfs转交到内核，系统卸载bootfs

rootfs（root file system）就是操作系统，包含Linux系统中标准的/bin，/etc等目录

Docker镜像为啥这么小的原因就是镜像只包含bootfs和rootfs，复用宿主机的内核


镜像分层的好处：可复用，方便共享（复制迁移）资源，镜像的每一层都可以被共享，可复用

镜像层是只读，容器层是可写的，当一个容器被启动，一个可写层被加载到镜像的顶部时，那么这一层被叫为容器层，容器层之下的都被叫为镜像层


commit命令提交副本来创建新镜像

docker commit -m="注释" -a="作者" 容器id 用户/镜像名:镜像版本号



---


搭建私有仓库




docker私有仓库（Docker Registry）

Docker Registry是docker提供的镜像，专门用来构建docker私有仓库

docker pull registry # 拉取Registry镜像

docker run -d -p 5000:5000 -v /test/registry/:/tmp/registry -- privileged=true registry 

默认情况下，仓库被创建在容器的/var/lib/registry目录下（当然也可以通过容器卷来映射）

docker tag ubuntu 192.168.1.110:5000/ubuntu # 修改tag为符合私有仓库的规范


这个私有仓库默认不支持http推送，需要修改/etc/docker/daemon.json

"insecure-registries": ["192.168.1.110:5000"]


如果不生效，请重启docker服务

推送镜像到私有仓库

docker push 192.168.1.110:5000/ubuntu


查看私有仓库存储的镜像

curl -XGET http://192.168.1.110:5000/v2/_catalog

获取私有仓库镜像到本地

docker pull 192.168.1.110:5000/ubuntu



---






容器数据卷


-v参数为启动自定义容器数据卷

比如说docker run -d -p 5000:5000 -v /test/registry/:/tmp/registry -- privileged=true registry 


/test/registry/为宿主机路径，/tmp/registry为容器内部路径，-- privileged=true为开启privileged，（开启privileged后，容器内部的root才拥有真正的root权限，否则容器的root只是普通用户权限）



容器数据卷用于数据的持久化（独立于容器的生命周期），可绕过联合文件系统来达到持续存在或者共享数据的目的，docker不会在删除容器时，删除容器挂载的数据卷，数据卷指定容器内部路径，任何在该路径存储的数据，都会被映射到宿主机指定的那个目录下

在数据卷中的任何更改都不会认为是镜像的更新，而且数据卷中的更改是实时生效的（不需要重启容器之类的，哪怕容器没有启动），数据卷的生命周期长（可持续到没有容器使用它为止）

数据卷被用于容器和宿主机之间的互通互联，在宿主机中更新的文件，会实时在容器中生效


docker inspect 镜像名或者镜像id # 获取容器/镜像的元数据

数据卷在该镜像的元数据的Mounts属性中找到


数据卷默认可读可写（rw），可通过ro来限制容器内部只能读，不能写

默认情况下

docker run -d -p 5000:5000 -v /test/registry/:/tmp/registry:rw -- privileged=true registry 

因为rw是默认的，可以省略不写

限制容器只能读取数据卷，不能写（ro，read only）

docker run -d -p 5000:5000 -v /test/registry/:/tmp/registry:ro -- privileged=true registry 


数据卷的继承（--volumes-from）


docker run -it -- privileged=true --volumes-from 容器1 --name 容器2 registry 

容器2继承容器1的数据卷规则（哪怕容器1被删除了或者停止运行了，依然不会影响到容器2的数据卷）

---


docker network



docker network可用于容器之间的互联以及通信（端口映射），可通过服务名来直接通信（不受IP变动而影响）

docker会默认创建一个叫docker0的虚拟网桥（bridge网络模式）

docker0网桥会在内核层连通其他网络网卡和虚拟网卡，实现所有的容器和本地主机的网络连接（同一个物理网络）

docker会默认创建三个网络模式，分别为bridge（默认），host，none

可通过docker network ls命令查看

创建网络

docker network create hallo

删除网络

docker network rm hallo

查看网络的详细信息

docker network inspect bridge


bridge网络模式：为容器分配和设置IP，并且将容器分配到docker0网桥（如果不指定-network，创建的容器默认在该模式上）（--network bridge，可忽略）

host网络模式：容器不会虚拟网卡，而是直接使用宿主机的ip和端口和外界通信（不需要额外进行NAT转换，没有独立的Network namespace，而是和宿主机共用一个Network namespace）（--network host）

none网络模式：该模式的容器有自己的独立网络规则（Network namespace），但是没有进行配置，需要手动配置docker容器网络（发挥网络定制功能，没有虚拟网卡，没有IP等等网络信息），在该模式下只有一个127.0.0.1的本地网络回环接口（lo）（--network none）

container网络模式：该模式下的容器不会配置自己的IP，而是和指定的容器共享IP和端口范围（网络共用，文件和进程隔离）（--network container:指定容器名或者容器ID）


自定义网络模式（可通过服务名（容器名/主机名）来通信，docker network create hallo）




---


docker-compose容器编排



docker-Compose是Docker官方提供的Docker容器集群快速编排工具，用于管理多个docker容器组成的应用，通过docker-compose.yml配置文件来管理多个容器之间的调用关系

官方文档https://docs.docker.com/compose/

下载文档：https://docs.docker.com/compose/install/

下载docker-Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

添加写的权限
sudo chmod +x /usr/local/bin/docker-compose

测试是否安装完成
docker-compose --version

构建并且启动容器（执行docker-compose.yml配置文件）（加参数-d为启动服务并且后台运行）
docker-compose up

构建或者重新构建服务
docker-compose build

删除指定服务的容器
docker-compose rm 容器名

停止并且删除容器，镜像，网络，卷
docker-compose down

进入容器
docker-compose exec 服务id

查看当前docker-compose编排的全部容器
docker-compose top

查看当前docker-compose编排的容器进程
docker-compose ps

停止docker-compose服务
docker-compose stop

启动docker-compose服务
docker-compose start

重启docker-compose服务
docker-compose restart

检查配置文件（-q参数表示当配置文件出现问题时才输出信息）
docker-compose config



容器的7种状态：

created（已创建）
restarting（重启中）
running或up（运行中）
removing（迁移中）
paused（暂停）
exited（停止）
dead（死亡）


docker-compose.yml配置文件


    version: "3.9"
    services:
        web:
            build:
                context: .  
                dockerfile: Dockerfile_test
            ports:
                - "5000:5000"
            volumes:
                -  /app/test:/data/app
            links:
                - redis
            networks:
                - hallo_net
        redis:
            image: redis
            command: redis-server /etc/redis/redis.conf
    networks:
        hallo_net:


version字段为版本号（具体支持的版本和docker-compose安装的版本有关，具体看实质版本），services字端表示服务容器，services字端下面的服务名的image字段为从指定镜像中启动容器（镜像可本地，可仓库或者镜像ID），ports字段为端口映射，networks字段为指定网络模式，volumes字段为容器数据卷

外面那个networks字段将会创建hallo_net自定义网络模式，处理基于指定镜像外，还可以基于Dockerfile，build字段下的dockerfile字段就是指定Dockerfile文件所在的路径，context字段为构建的路径，links字段为链接到指定服务中的容器

一般都是通过Dockerfile自动化构建镜像，然后基于这个镜像使用docker-compose管理容器




---

Portainer可视化工具



Portainer是一个Docker轻量级的可视化工具（Portainer官网，https://www.portainer.io，官网文档：https://docs.portainer.io/v/ce-2.11/start/install/server/docker/linux）


可直接pull Portainer的镜像来安装

docker pull portainer/portainer

    docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
        --restart=always \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -v portainer_data:/data \
        portainer/portainer-ce:2.11.1


启动完毕后，访问1192.168.1.110:9443/，进行设置admin用户和密码

如果是监控本地的docker，选择local






---


CAdvisor+InfluxDB+Granfana(CIG重量级容器监控)








---



kubernetes简称k8s，用来自动化容器的部署，监控，容器负载均衡等等

master是容器集群的控制系统，可以用来监控容器的状态，调度负载均衡

node是k8s的工作节点，可以接收master的指令，根据指令来创建和销毁Pod等等

Pod是容器的容器，可以包含多个容器，是k8s中最小的可部署单元，pod内部的网络是互通的，每一个pod都有自己的虚拟ip

k8s将弃用dockershim（dockershim是k8s内置的一个组件，该组件可让k8s能够通过CRI（Container Runtime Interface）来操作docker）