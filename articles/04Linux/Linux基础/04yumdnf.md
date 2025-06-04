---
layout: default
title: yumdnf
---



# 本地yum源

## yum是什么？

> * yellowdog update manager黄狗软件升级器

## 光盘插上

## 查看所有块设备

```bash
lsblk
# sr0，光盘设备，类型rom
ls -lah /dev/
# cdrom -> sr0
# sr0是光盘，cdrom是软链接
```

## 把光盘挂上

```bash
mkdir /mnt/dvd/ -p
ls -lah /mnt/dvd/
mount /dev/cdrom /mnt/dvd/
ls -lah /mnt/dvd/
# sr0是光盘，cdrom是软链接，dvd/是目录
umount /mnt/dvd/
```

## 设置开机自动挂载

```bash
vim /etc/fstab
/dev/cdrom /mnt/dvd/ iso9660 ro 0 0
systemctl daemon-reload
# 重新加载守护进程
mount -a 
# 手动挂载fstab文件中指定的设备
# 要么就是重启后自动按fstab文件挂载
ls -lah /mnt/dvd/
```

## 设置yum源为本地源

```bash
ls -lah /etc/yum.repos.d/
mv /etc/yum.repos.d/openEuler.repo /etc/yum.repos.d/guanfang
yum-config-manager --add file:///mnt/dvd/
# 创建本地yum源文件
ls -lah /etc/yum.repos.d/
```

## 配置不对yum源进行gpg公钥检查

```bash
vim /etc/yum.conf
gpgcheck=0 
# 不进行gpg公钥检查，不能确定软件来源是否信任
```

# dnf源

## 什么是dnf源？

> * dnf<mark> 是yum源的升级版</mark>

## dnf源配置文件在哪？

```bash
cd /etc/yum.repos.d/
ls -lah
vim openEuler.repo
```

## 更改dnf源配置文件，重新构建缓存

```bash
mv openEuler.repo openEuler.guanfang
vim openEuler.Tsinghua
vim openEuler.aliyun
vim openEuler.huaweicloud
mv openEuler.Tsinghua openEuler.repo
# 使用清华源
dnf clean all
dnf makecache
```

# 看看指定的yum仓库有多少软件

```bash
dnf repolist -v
yum repolist -v
```

# 检查是否有更新

```bash
dnf check-update
yum check-update
```

# 更新

```bash
dnf update -y && dnf upgrade -y
yum update-y && yum upgrade -y
```

# 看看仓库有httpd没

```bash
yum list httpd
dnf list httpd
```

# 安装httpd

```bash
yum install httpd -y
```

# 卸载httpd

```bash
yum remove httpd -y
```

# 包格式都有什么？

> * .deb是apt包管理器的软件包
> 
> * .rpm是dnf（yum）包管理器的软件包
