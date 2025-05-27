---
layout: default
title: kali基本使用
---



# 安装Kali虚拟机

> * <mark>Kali官网</mark>  [kali官网](https://www.kali.org/) 

# 更新软件源

## 理论

> * Debian包管理工具是<mark>apt advanced package tool</mark>
> 
> * apt的软件源路径是<mark>/etc/apt/sources.list</mark>

## 国内软件源

> * deb https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
> 
> * deb-src https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
> 
> * deb https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main non-free contrib non-free-firmware
> 
> * deb https://mirrors.ustc.edu.cn/kali kali-rolling main non-free non-free-firmware contrib

## Kali官方源

> * deb http://http.kali.org/kali kali-rolling main contrib non-free
> 
> * deb-src http://http.kali.org/kali kali-rolling main contrib non-free

# 查看IP掩码、网关、dns

```bash
ifconfig # 查看IP、掩码
route -n # 查看gateway
# n numeric 数字的
cat /etc/resolv.conf 
# cat 查看文本文件的命令
# /etc 目录 等等的意思，意思是其他，后来大家把配置文件都放这里
```

# 图形化修改网络属性

> * 设置》高级网络配置》选中网卡》ipv4设置》
> 
> * 选择自动（DHCP）
> 
> * 或者选择手动：要指定IP掩码、网关、dns
> 
> * 保存
> 
> * 回到桌面：断开网卡》重连网卡

# Kali基础操作

## 为root设置密码

```bash
sudo passwd root
```

## 查看当前shell

```bash
echo $SHELL
```

## 终端更改root权限

```bash
sudo -i
```

## 切换卧底模式

> * win键》undercover

## 打开文件管理器

> * win键》thunar

## 打开终端

> * win键》terminal

## 打开记事本

> * win键》mousepad
