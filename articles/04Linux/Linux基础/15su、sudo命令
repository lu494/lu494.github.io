---
layout: default
title: su、sudo命令
---

# su命令

## su命令啥作用？

> * su命令用于切换用户
> 
> * 但是得知道目标用户的密码
> 
> * 当然了root用户除外，想切谁切谁

## su命令谁可以使用？

> * 如果有wheel组的限制，那只有root、wheel组的可以使用
> 
> * 也可以取消wheel组的限制

## su命令怎么使用

```bash
su - zhangsan
# 完全切换为zhangsan用户
```

## 加入wheel组使用su命令

```bash
gpasswd wheel -a zhangsan
id zhangsan
cat /etc/group | grep wheel
su - root
```

## 取消wheel组限制使用su命令

```bash
vim /etc/pam.d/su
# pam pluggable authentication module Linux安全模块
# auth    required  pam_wheel.so use_uid
# 注释掉这一行
```

## su命令使用日志

```bash
vim /var/log/secure
# / su，搜索su
```

# sudo命令

## sudo命令咋回事？

> * sudo是super user do，sudo权限就是root权限

## 编辑sudo授权表

```bash
EDITOR=vim visudo
vim /etc/sudoers
# root修改只读文件:wq! 保存
@includedir /etc/sudoers.d/
# 包含/etc/sudoers.d/目录下的模块化配置文件
root ALL=(ALL:ALL) ALL
# root 在任何主机上 以任何用户任何组权限  执行任何命令
%wheel ALL=(ALL:ALL) ALL
# wheel组用户，在任何主机上，以任何用户任何组权限，执行任何命令
zhangsan ALL=(ALL:ALL) ALL
```

## 设置sudo命令日志位置

```bash
vim /etc/sudoers
Defaults logfile=/var/log/sudo
:wq!
# 系统会自动在/var/log/目录下生成sudo文件
cat /var/log/sudo
# 查看日志
```
