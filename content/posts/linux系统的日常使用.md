---
title: "linux系统的日常使用"
categories: [ "技术" ]
tags: [ "Linux" ]
draft: false
slug: "8"
date: "2021-06-16 09:12:00"
---

linux发行版有很多，这里使用的是Ubuntu

个人喜欢使用无界面版本（ubuntu-20.04-live-server-amd64）

有关于树莓派的看这个

修改为国内源

nano /etc/apt/sources.list

替换内容为


    # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe     multiverse
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

    # 预发布软件源，不建议启用
    # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
    # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse

注：该源为清华大学开源软件镜像站的,可以修改为其他镜像站的源

163镜像源

    # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
    deb http://mirrors.163.com/ubuntu/ focal main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ focal-security main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ focal-updates main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ focal-backports main restricted universe multiverse
    # deb-src http://mirrors.163.com/ubuntu/ focal main restricted universe multiverse
    # deb-src http://mirrors.163.com/ubuntu/ focal-security main restricted universe multiverse
    # deb-src http://mirrors.163.com/ubuntu/ focal-updates main restricted universe multiverse
    # deb-src http://mirrors.163.com/ubuntu/ focal-backports main restricted universe multiverse
    # 预发布软件源，不建议启用
    # deb http://mirrors.163.com/ubuntu/ focal-proposed main restricted universe multiverse
    # deb-src http://mirrors.163.com/ubuntu/ focal-proposed main restricted universe multiverse



sudo apt update #更新源
sudo apt upgrade #更新系统

安装app

sudo apt install App名字

例如安装ssh服务：

apt update
apt install openssh-server

poweroff 立刻关机

shutdown -h now 立刻关机

shutdown -r now 立刻重启

shutdown -r 10 10分钟后重启

shutdown -r 1:00 当时间为1:00时重启

wget 下载文件链接

安装.deb文件

sudo dpkg -i 文件名

例如：

sudo dpkg -i code_1.40.2-1574694120_amd64.deb

clear 清除

ls和cd

cd命令是前往指定目录

cd ~ 前往目前使用的用户的Home目录（家目录）

cd .. 返回上一级目录

cd /etc/python3 前往指定目录

cd - 返回刚刚所在的目录

cd ../../ 向上返回两级目录

cd ./ 前往当前目录

pwd 显示当前目录路径

ls -ah  将所有目录（包括隐藏目录）显示出来

如果有很多层目录，那么需要一个绝对目录

绝对目录：指目录的绝对位置，从根目录开始的路径，完整的描述文件位置的路径就是绝对路径
相对目录：指目标相对于当前文件的路径

ls (list 的简写)

ls 返回当前所在的目录的所有子目录和子文件

ls -l 返回文件的一些信息，权限，用户，文件大小，修改时间。文件名，total后面的数字是指这个目录下的文件数，包括隐藏文件

ls -a 显示所有文件，包括已经隐藏的文件(以 . 开头的)

ls -lh 这个指令和-l一样，返回权限，用户，文件大小，修改时间。文件名，不过文件大小是使用k，m这些来表示

ls --help 查看ls的全部功能

cp，mv，touch，mkdir

touch 新建一个文件
例如：
touch hallo.py

如果想同时建立多个文件，那么输入多个文件的名字，使用空格分开，例如：

touch hello.py world.py

cp 是复制文件或者文件夹的指令，使用方法：

cp hello.py world.py

注意: 如果复制的文件已经存在, 它将会直接覆盖已存在的文件，那么可以：
cp -i hello.py world.py
在返回的提问中回复任何大小写形式的y或者yes，将执行覆盖操作，回复其他则取消这个执行

cp -R hello/ world/ 复制文件夹

cp go* gogo/ 复制所有的以go开头的文件到gogo文件夹中

cp *go gogo/ 复制所有的以go结尾的文件到gogo文件夹中

mv 是用来移动（剪切）文件

mv 要移动的文件 要移动到的文件夹，例如：

mv hello.py hello/

mv world.py hallo.py 重命名文件

mv --help 查看其他mv的用法

mkdir hello 新建一个文件夹

rmdir hello/world 删除hello目录下的world目录，不能删除有文件的文件夹，只能删除一个空的文件夹

rm world/ 可以删除不是空的文件夹

rm -i world/ 删除文件时提示

rm -I world/ 删除超过3个文件时才提示，少于3个时不提示

rm -r world/ 删除目录及目录下的所有文件及目录

rm -rf world/ 删除目录下及目录下的所有文件及目录，没有提示地强制递归删除文件

nano hallo.py
nano是一个，^是ctrl键，例如ctal+x 是退出的指令

cat hallo.py 输出一个文件的文本内容

cat hallo.py > world.py 把一个文件复制成另一个文件

cat hallo.py hello.py > world.py 把两个文件的内容合并到另一个文件

cat world.py >> gogo.py 补充到另一个文件的内容的后面

文件权限
例如
drwxr-xr-x

d是说明这是一个文件夹，如果是-那么就是说明这是一个文件

rwx
r 可读，w 可写， x 可执行，如果有是-说明当前使用的用户账号不能完成某一个操作，User是当前使用的用户

r-x
r 可读，w 不可写， x 可执行，说明Others这个组的用户不能完成写入的能力，没有写入的权限，Others是指当前用户所在的组

r-x
r 可读，w 不可写， x 可执行，说明User 和 Group 以外的人不能完成写入的能力，没有写入的权限，Others是除当前用户和组之外的人

修改文件权限

chmod u+r hallo.py

chmod ug+rw hello.py
给当前用户添加可读权限

\+ 为加上某一个权限
\- 为去掉某一个权限
= 为等于某个权限

u: 对于 User 修改,
g: 对于 Group 修改
o: 对于 Others 修改
a: 对于所有人修改

chmod 755 hello.py
修改文件权限
u为rwx，可读可写可执行，g为r-x可读不可写可执行，o为r-x可读不可写可执行

r=4，w=2，x=1



ifconfig 这个命令是用来查看当前linux设备的ip

ip addr 这个命令也是用来查看当前linux设备的ip的

apt-get install net-tools 安装网络工具，用于解决ip没有分配之类的网络管理工具

exit 退出ssh控制


top 查看cpu占用的一些信息




---

linux-ssh安全配置

ssh安全

last -f /var/log/wtmp  查看IP登陆日志

修改ssh端口

nano /etc/ssh/sshd_config

找到Port，修改端口，如果修改为2222，那么默认ssh连接端口为2222，原来的22端口报废（不能使用22端口登录了，计算机有65535个端口，扫描端口都要扫半天）


禁止root用户ssh登录

找到PermitRootLogin，把yes修改为no

其他ssh安全配置

LoginGraceTime  登录一次花费多少时间，当超过这个时间，则要求重新登录

MaxAuthTries   限制尝试登录错误的次数，当超过这个次数之后拒绝登录尝试
MaxSessions    限制每个连接可以并行开启多少个会话

StrictModes，修改为no，为禁用密码登录

重启sshd服务 service ssh restart


SSH密钥登录

ssh-keygen -t rsa -b 4096  生成私钥和公钥

在.ssh的目录下有两个文件，id_rsa 为私钥，id_rsa.pub 为公钥

cat ~/.ssh/id_rsa.pub >> authorized_keys

然后上传authorized_keys到服务器上

上传文件可以用ftp或者scp

例如
scp -P 2222 .\\authorized_keys pi@192.168.1.106:~/.ssh


给.ssh目录和authorized_keys文件设置权限

chmod -R 0700  ~/.ssh

chmod -R 0644  ~/.ssh/authorized_keys

ssh pi@192.168.1.106 -p 2222

就可以不用密码进行登录了

---


linux下Operation not permitted

先看看文件权限ll，能使用root用户或者sudo最好，然后尝试chmod

如果在root用户或者sudo下也不能对文件或者文件夹进行操作，那么

chattr +i hello.py // 使用i属性来锁定文件，避免修改文件

lsattr  hello.py // 查看加锁

chattr -i hello.py // 去除i属性，便可以修改该文件

a：可以修改文件，但是不能删除

i: 不能删除，不能修改，不能移动

---


ifconfig // 查看网络配置，或者ip addr ls

网卡配置文件在/etc/sysconfig/network-scripts下

网卡配置文件名称一般为ifcfg-xxx

默认为关闭的，需要使用DHCP获取ip，需要修改ONBOOT=no，修改为yes


配置静态ip，需要修改BOOTPROTO为static

添加下面内容

IPADDR=192.168.186.128   // 设置IP地址
NETMASK=255.255.255.0     // 设置子网掩码
GATEWAY=192.168.186.1    // 设置网关
DNS1=114.114.114.114     // 设置dns


nmcli c reload // 重启网络服务

ip addr // 查看本地IP地址以及其他网卡信息


ip link set dev eth0 up // 启用网卡 或者ifup eth0

ifconfig eth0 192.168.186.128 // 修改指定网卡的ip

ifconfig eth0 192.168.186.128 netmask 255.255.255.0 // 修改子网掩码

route -n // 显示和操作IP路由表

route add default gw 192.168.186.1 // 增加指定网关，其中的default是指0.0.0.0的，可自己选择想要的，例如route add -host 192.168.186.2 gw 192.168.186.1

route add -net 192.168.0.0 netmask 255.255.255.0 gw 192.168.186.1 // 设置网段的网关

route del default gw 192.168.186.1  // 删除指定网关


---

