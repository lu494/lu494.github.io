---
layout: default
title: 08Linux用户组管理 
---

# 查看当前用户

```bash
[root@openeuler-2404 ~]#
  用户root，主机名openeuler-2404，家目录/root/，超级shell
[zhangsan@openeuler-2404 ~]$ 
 用户zhangsan，主机名openeuler-2404，家目录/home/zhangsan/，普通shell
```

# 切换用户

## root切普通

```bash
su - zhangsan # 切到张三
# 完全切过去
```

## 普通切root

```bash
# 首先普通用户得假如wheel组
usermod zhangsan -aG wheel
id zhangsan
cat /etc/group | grep wheel
exit
# 重新登录zhangsan shell
sudo -i
sudo su -
gpasswd wheel -d zhangsan
```

# 查看用户和组

```bash
# 查看root用户
id root
# uid=0(root) gid=0(root) 组=0(root)
# 文件目录的权限有属主、属组、其他组之分
# 你root用户uid就是你自己、gid是同名主组，所有所属的所有组
# root 0
# 系统用户，服务用的，1-999
# 普通用户，人用的，1000-65535
cat /etc/passwd | grep root 
# root:x:0:0:Super User:/root:/bin/bash
cat /etc/shadow | grep root
# root:$y$j9T$TYeRkNgz8NahvH/uSnjO52cV$pR8Fw4/UbR89LS2QcionBry3Qv8EyqwMtCCuf3yIRY1::0:99999:7:::
cat /etc/group | grep root
root:x:0:
cat /etc/gshadow | grep root
root:::

# 查看张三用户
id zhangsan
cat /etc/passwd | grep zhangsan
zhangsan:x:1000:1000::/home/zhangsan:/bin/bash
cat /etc/shadow | grep zhangsan
zhangsan:!:20238:0:99999:7:::
cat /etc/group | grep zhangsan
zhangsan:x:1000:
cat /etc/gshadow | grep zhangsan
zhangsan:!::
```

# 创建用户

```bash
useradd zhangsan
# 创建用户，创建家目录，创建同名主组
ls /home/ # 验证家目录
id zhangsan # 验证用户
cat /etc/group | grep zhangsan # 验证同名主组
```

# 给用户设置密码

```bash
passwd zhangsan # 需要交互
usermod zhangsan -p $(openssl passwd -6 "123456")
# $()指变量
# openssl passwd工具生成加密密码
# 参数6,指定某种hash算法
# 原始密码123456
echo 123456 | passwd zhangsan --stdin
```

# 删除用户

```bash
userdel zhangsan -r
# 删除用户，删除主组，删除家目录
```

# 创建组

```bash
groupadd IT
# 只创建组
# 0 root
# 1-999 系统组
# 1000-65535 普通组
```

# 组加用户、组删用户

```bash
usermod zhangsan -aG root
gpasswd root -a zhangsan
# 用户加组
gpasswd root -d zhangsan
# 组删用户
usermod zhangsan -G lisi,wangwu
# 覆写张三所属组
# 用户张三同名主组永远删不了
# 用户张三永远归属于自己同名主组，去不掉，不用刻意强调
```

# 删除组

```bash
groupdel IT
```
