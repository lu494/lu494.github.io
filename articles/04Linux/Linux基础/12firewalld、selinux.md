---
layout: default
title: firewalld、selinux
---

# firewalld防火墙

## 开闭firewalld防火墙

```bash
systemctl disable firewalld.service --now
systemctl enable firewalld.service --now
systemctl status firewalld.service
```

## 列出firewalld当前规则

```bash
firewall-cmd --list-all
```

## 放行端口

```bash
# firewalld防火墙是分区域的
# firewalld防火墙是管进不管出的
# firewalld防火墙默认是拒绝
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --reload
firewall-cmd --zone=public --add-port=53/udp --permanent
firewall-cmd --reload
```

## 删除端口

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --reload
firewall-cmd --zone=public --remove-port=53/udp --permanent
firewall-cmd --reload
```

# selinux

## selinux是啥？

> * security enhanced 安全增强模块
> 
> * 分为三种状态：enforcing（执行）、permissive（放行，会记录日志）、disabled

## 查看selinux状态

```bash
getenforce
```

## 设置selinux状态

```bash
setenforce 0 
# permissive
setenforce 1 
# enforcing
vim /etc/selinux/config
SELINUX=disabled
init 6
```
