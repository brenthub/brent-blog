layout: post
categories:
- linux

tags:
- linux
- systemctl

comments: true
title: systemctl命令
permalink: systemctl-command
date: 2016-12-7 17:03:15
---

Systemctl是一个systemd工具，主要负责控制systemd系统和服务管理器。
Systemd是一个系统管理守护进程、工具和库的集合，用于取代System V初始进程。Systemd的功能是用于集中管理和配置类UNIX系统。

Systemctl接受服务（.service），挂载点（.mount），套接口（.socket）和设备（.device）作为单元

# 列出所有可用单元
```shell
systemctl list-unit-files
```

# 列出所有运行中单元
```shell
systemctl list-units
```

# 列出所有失败单元
```shell
systemctl --failed -a
```

# 检查某个单元或服务是否运行
```shell
systemctl status firewalld.service
```
# 列出所有服务（包括启用的和禁用的）
```shell
systemctl list-unit-files --type=service
```
# 启动、重启、停止、重载服务
```shell
# systemctl start httpd.service
# systemctl restart httpd.service
# systemctl stop httpd.service
# systemctl reload httpd.service
# systemctl status httpd.service
```
# 系统启动时自动启动服务
```shell
# systemctl is-active httpd.service
# systemctl enable httpd.service
# systemctl disable httpd.service
```
# 屏蔽（让它不能启动）或显示服务
```shell
systemctl mask httpd.service
systemctl unmask httpd.service
```
# 检查某个服务的所有配置细节
```shell
systemctl show httpd
```
# 分析某个服务（httpd）的关键链

```shell
systemd-analyze critical-chain httpd.service
```

# 获取某个服务（httpd）的依赖性列表
```shell
systemctl list-dependencies httpd.service
```
