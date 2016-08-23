layout: post
categories:
- server

tags: 
- VMware

comments: true
title: VMware网络模式及设置
permalink: vmware-network
date: 2016-08-23 09:03:15
---

本章将介绍VMware三种网络模式bridged(桥接模式)、NAT(网络地址转换模式)和host-only(主机模式)的区别，以及在Centos中NAT模式的设置。

## 模式介绍

### Bridged 模式
在桥接模式下，VMware虚拟机里的系统就像是 局域网 中的一台 独立 的主机，它可以访问同一个网段内任何一台机器，即可以相互ping通。

* 使用场景
如果是用做服务器，网络ip是固定的，推荐使用这种方式。

### NAT 模式
NAT 即 Network Address Translation 缩写，即网络地址转换，由 NAT服务完成，在vmware里默认为VMnet8虚拟交换机，它将虚拟系统的IP地址转换成宿主机的IP地址，从而借用宿主机访问其他主机。使用NAT模式，也可以让虚拟系统通过宿主机器所在的网络来访问公网。

在这种模式下，虚拟系统是不能被LAN内其他PC访问的（宿主机可以），只能虚拟机以宿主机的名义访问LAN内的计算机。默认情况下NAT模式的虚拟系统的TCP/IP配置信息由VMnet8(NAT)虚拟网络的DHCP服务器提供，因此采用NAT模式最大的优势是虚拟系统接入互联网非常简单，你不需要进行任何其他的配置，只需要宿主机器能访问互联网即可。使用NAT方式时，宿主机（Windows）网络管理里会多出一块虚拟网卡，名为VMware Network Adepter VMnet8

* 使用场景
如果网络ip不是固定的，比如笔记 本上搭建VM，ip不稳定，推荐这种方式。这样，无论网络ip怎么改变，虚拟机跟宿主机的ip是固定的。

### host-only 模式
在Host-Only模式下，虚拟系统所在的虚拟网络是一个全封闭的网络，它唯一能够访问的就是宿主机。其实Host-Only网络和NAT网络很相似，不同的地方就是Host-Only网络没有NAT服务，所以虚拟网络不能连接到Internet，即虚拟系统无法上网。

* 使用场景
无法联网，封闭的网络环境使用

## Centos中NAT模式的设置

在自己学习的一般情况下，我们都使用NAT模式，故在这里着重介绍下Centos中NAT模式的设置

打开VM的虚拟网络编辑器
![image](https://cloud.githubusercontent.com/assets/10822807/17879899/02fc7ed6-6928-11e6-8833-5de599904db1.png)

修改NAT模式的IP，网关，NDS（或采取默认）
如：我把网段设为30

### 指定虚拟机静态ip
```shell
vi /etc/sysconfig/network-scripts/ifcfg-eno0
```
注：可能不叫ifcfg-eno0

修改为类似内容
```shell
BOOTPROTO=static

ONBOOT=yes
IPADDR=192.168.30.8
NETMASK=255.255.255.0
GATEWAY=192.168.30.1
DNS1=192.168.30.1
DNS2=114.114.114.114
```

### 修改主机名(可以不修改，只是为了标识)

```shell
vi /etc/sysconfig/network
```
修改主机名称

```shell
HOSTNAME=hadoopNameNode
```

* 这个有什么用
![image](https://cloud.githubusercontent.com/assets/10822807/17880042/578dccb0-6929-11e6-8ae0-96bec4bc9d6d.png)

### 重启网络服务
```shell
service network restart
```
