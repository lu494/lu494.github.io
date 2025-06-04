---
layout: default
title: Linux网络管理
---

# 通过network管理网络

## network服务是啥？

> * network组件比较老，要被淘汰了

## 安装network组件

```bash
dnf install -y network-scripts
```

## 开闭network服务

```bash
systemctl status network
systemctl start network
systemctl enable network
systemctl stop network
systemctl disable network
```

## network服务配置文件在哪呢？

```bash
cd /etc/sysconfig/network-scripts/
ls -lah
vim ifcfg-ens160
vim /etc/sysconfig/network
# 这个文件一般不用改，但他里面如果有内容也会影响网络配置
vim /etc/resolv.conf
# 这个是自动生成的，不用手动改
```

## 通过network配置文件修改网络为DHCP

```bash
ONBOOT=yes
BOOTPROTO=dhcp
```

## 通过network配置文件修改网络为静态

```bash
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.10.10
NETMASK=255.255.255.0
GATEWAY=192.168.10.1
DNS1=8.8.8.8

```

## 重启网卡重启服务

```bash
ifdown ens160 && ifup ens160
systemctl restart network
```

## 通过传统命令查看网络配置

```bash
ifconfig
route -n
cat /etc/resolv.conf
# 使用传统工具
```

## 通过新命令查看网络配置

```bash
ip a
ip r
cat /etc/resolv.conf
# 使用新工具
```

# 通过NetworkManager管理网络

## NetworkManager是啥？

> * NetworkManager比较新，比较适用于图形化个人Linux

## 安装NetworkManager组件

```bash
dnf install -y NetworkManager
```

## 开闭NetworkManager服务

```bash
systemctl status NetworkManager
systemctl start NetworkManager
systemctl stop NetworkManager
systemctl enable NetworkManager
systemctl disable NetworkManager
```

## 通过nmcli查看网络配置

```bash
nmcli connection show # 查看连接
nmcli device show # 查看网卡、IP掩码、网关、dns
nmcli con show ens160
nmcli dev show ens160
```

## 重启网卡、重启连接、重启服务

```bash
nmcli device disconnect ens160 && nmcli device connect ens160
nmcli con down ens160 && nmcli con up ens160
systemctl restart NetworkManager
```

## 通过nmcli管理网络

### 删除混乱连接

```bash
nmcli connection delete ens160
nmcli connection show
nmcli device show
```

### 修改（创建）网络为DHCP

```bash
nmcli connection add type ethernet ifname ens160 con-name ens160
nmcli con mod ens160 connection.autoconnect yes
# 配置连接为自动连接
nmcli con mod ens160 ipv4.method auto
```

### 修改（创建）网络为静态

```bash
nmcli con mod ens160 ipv4.method manual ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.dns 114.114.114.114
```

## 通过配置文件管理网络 （keyfile插件）

### 查看（修改）插件

```bash
vim /etc/NetworkManager/NetworkManager.conf
# / plugins 搜索plugins
# 修改格式plugins=keyfile
```

### keyfile插件配置文件路径

```bash
cd /etc/NetworkManager/system-connections/
ls -lah
vim ens160.nmconnection # 权限必须为 600
```

### 修改ens160.nmconnection配置DHCP网络

```bash
[ipv4]
method=auto
```

### 修改ens160.nmconnection配置静态网络

```bash
[ipv4]
method=manual
address1=192.168.1.1/24,192.168.1.254
dns=114.114.114.114;
```

## 通过配置文件管理网络（ifcfg-rh插件）

### 查看（修改）插件

```bash
plugins=ifcfg-rh
```

### ifcfg-rh插件配置文件路径

> * 就是network服务的配置文件路径
> 
> * /etc/sysconfig/network-scripts/

## 通过nmtui管理网络

```bash
nmtui # text user interface文本用户接口
```

> * 编辑连接》激活连接

# 通过systemd-networkd

## systemd-networkd是啥？

> * 是一个轻量的网络管理服务，很高效
> 
> * 和systemd-resolved一起使用

## 安装systemd-networkd组件

```bash
dnf install -y systemd-networkd
dnf install -y systemd-resolved
```

## 开闭服务

```bash
systemctl status systemd-networkd
systemctl start systemd-networkd
systemctl stop systemd-networkd
systemctl enable systemd-networkd
systemctl disable systemd-networkd
systemctl status systemd-resolved
systemctl start systemd-resolved
systemctl stop systemd-resolved
systemctl enable systemd-resolved
systemctl disable systemd-resolved
```

## 把系统dns路径变成软链接指向systemd-resolved服务的dns文件

```bash
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
# s softlink
# f force强制覆盖
# /run/systemd/resolve/resolv.conf是实体文件
# /etc/resolv.conf 是系统dns解析文件
# /etc/resolv.conf本来是文件，现在是软连接，指向systemd-resolved服务的解析文件
ls -lah /etc/resolv.conf
```

## systemd-networkd配置文件介绍

> * 路径/etc/systemd/network/
> 
> * 自己创建
> 
> * *.network文件用来管理配置
> 
> * *.netdev文件是虚设备文件（一般用不到）
> 
> * /etc/systemd/networkd.conf文件是全局配置（一般不用改）
> 
> * 自己创建*.network文件
> 
> * 文件自己命名，名称的字典序就是配置的匹配顺序

## 创建（修改）配置文件，管理systemd-networkd网络

```bash
vim 10-ens160.network
# 静态网络配置如下
[Match]
Name=ens160
[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=114.114.114.114

#DHCP网络配置如下
[Match]
Name=ens160
[Network]
DHCP=yes
```

## 重启服务

```bash
systemctl restart systemd-networkd
```

## 查看网络配置

```bash
networkctl
networkctl status ens160
resolvectl
resolvectl status ens160
```
