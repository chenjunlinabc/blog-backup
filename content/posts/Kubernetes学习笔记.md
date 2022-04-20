---
title: "Kubernetes学习笔记"
categories: [ "默认" ]
tags: [ "k8s" ]
draft: false
slug: "127"
date: "2022-01-10 12:31:00"
---

Kubernetes（k8s，8是指k到s之间有8个字母），是谷歌在2014年发布并且开源的容器化集群管理系统（已在谷歌生产环境中工作15年），支持自动化部署，应用容器化管理，大规模升级或回滚，应用扩展等等


k8s的特性：

自动部署与滚动更新：自动化部署应用容器，k8s采取滚动式更新，可以根据应用的情况进行一次或者批量更新（判断该应用添加是否正常使用），也可以进行历史版本回滚

自我修复：当某个容器发生故障，会自动重新启动失败的容器，当某个节点（Pod）出现故障进行容器的替换或者重新部署，并且关闭没有通过检查的容器（不进行处理请求，保证服务不中断），直到容器恢复正常

弹性伸缩：通过命令，UI界面，CPU等资源使用情况，自动对应用容器进行扩容或者缩容，保证在高峰期的高可用性，降低运行成本

服务发现和负载均衡：k8s为容器对外提供了统一访问入口（api server），并且关联全部容器（负载均衡）

密钥与配置管理：允许在不重新构建容器（不需要重新集群，热更新）的情况下更新应用程序配置，可以存储和管理密钥，令牌等敏感信息，不需要暴露这些敏感信息（部署和更新密钥）

存储编排：支持挂载外部存储（网络存储服务，云存储服务）（本地当然是支持），自动完成存储系统的挂载以及应用，保证数据持久化

批处理：支持一次性任务，定时任务


k8s的集群架构分为2个组件，分别为master（主控节点）和node（工作节点）

master组件：api server（集群统一人口，restful），scheduler（节点调度，调度node节点应用部署），controller-manager（处理集群中后台任务，一个资源对应一个控制器，资源控制），etcd（存储系统，用来存储集群的相关数据）

node组件：kubelet（node节点代理，管理k8s容器），kube-proxy（负责网络代理，负载均衡等操作）


Pod：k8s管理系统中最小部署单位，一个或者多个容器的集合（共享同一个网络），容器重启将结束该pod的生命（生命周期短）

Service

Volume

Namespace


Controller：控制器（确保预期pod的数量），状态应用部署（无状态（没有任何限制约定），有状态（有限制条件，依赖需要））负责一次性任务，定时任务，守护进程等等，确保所有node运行同一个pod

Service Ingress：对外接口（定义pod的访问规则）

RBAC：安全机制，权限管理

Helm：包管理器，快速下载，安装软件


---


k8s集群搭建

单master集群：单个master节点，管理多个node节点

多master集群（高可用集群）：多个master节点，管理多个node节点，中间存在着负载均衡的过程

kubeadm

kubeadm是k8s部署工具，用于快速部署k8s集群

官方文档：https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

创建master节点

kubeadm init

将node节点添加到当前集群中

kubeadm join master节点ip和端口


这边使用的是ubuntu-20.04-live-server-amd64，虚拟机（网络连接方式为桥接）

配置方面是4GB内存，2处理器，30GB存储

master1：192.168.1.102
node1：192.168.1.105
node2：192.168.1.104


一些初始化操作

更换系统软件源

vim /etc/apt/sources.list

这边用的腾讯源（https://mirrors.cloud.tencent.com/ubuntu/）

    deb https://mirrors.cloud.tencent.com/ubuntu/ focal main restricted universe multiverse
    deb https://mirrors.cloud.tencent.com/ubuntu/ focal-security main restricted universe multiverse
    deb https://mirrors.cloud.tencent.com/ubuntu/ focal-updates main restricted universe multiverse
    deb https://mirrors.cloud.tencent.com/ubuntu/focal-backports main restricted universe multiverse
    # deb-src https://mirrors.cloud.tencent.com/ubuntu/ focal main restricted universe multiverse
    # deb-src https://mirrors.cloud.tencent.com/ubuntu/ focal-security main restricted universe multiverse
    # deb-src https://mirrors.cloud.tencent.com/ubuntu/ focal-updates main restricted universe multiverse
    # deb-src https://mirrors.cloud.tencent.com/ubuntu/ focal-backports main restricted universe multiverse
    # 预发布软件源，不建议启用
    # deb https://mirrors.cloud.tencent.com/ubuntu/ focal-proposed main restricted universe multiverse
    # deb-src https://mirrors.cloud.tencent.com/ubuntu/ focal-proposed main restricted universe multiverse




apt update

apt upgrade


关闭防火墙

sudo ufw disable


关闭swap

sudo swapoff -a

可以使用free -m或者top检查

设置主机名（因为我在创建虚拟机的时候已经设置了）

sudohostnamectl set-hostname 主机名

设置hosts

sudo vim /etc/hosts

192.168.1.102 master1
192.168.1.105 node1
192.168.1.104 node2

selinux（ubuntu默认没有这个东西，不用管）


桥接的IPV4流量传递到iptables的链

cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF


生效配置
sysctl --system


时间同步


apt install ntpdate
ntpdate cn.pool.ntp.org
hwclock --systohc


设置时区

sudo timedatectl set-timezone Asia/Shanghai

写入硬件时钟

sudo timedatectl set-local-rtc 0

(Kubernetes从v1.20版本开始弃用Docker)


安装docker

配置docker

添加docker官方GPGkey

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

安装依赖

sudo apt install apt-transport-https ca-certificates curl software-properties-common

设置docker仓库

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

重新加载源

sudo apt update

如果报错，可以手动在/etc/apt/source.list添加

deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable

sudo apt dist-upgrade（智能处理包依赖，会自己安装新软件或者删除原有软件包来完成升级）

安装docker

sudo apt install docker-ce docker-ce-cli containerd.io

添加k8s国内源

sudo tee /etc/apt/sources.list.d/kubernetes.list <<-'EOF'
deb https://mirrors.tencent.com/kubernetes/apt/ kubernetes-xenial main
EOF

添加签名

gpg --keyserver keyserver.ubuntu.com --recv-keys 836F4BEB
gpg --export --armor 836F4BEB | sudo apt-key add -
apt update

836F4BEB这个为NO_PUBKEY的后8位

安装kubeadm

apt install kubeadm








