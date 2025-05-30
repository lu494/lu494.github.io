---
layout: default
title: 06Linux文件、目录管理
---

# 打印工作目录、改变工作目录

```bash
pwd
cd ..
cd /
# ~/ 当前用户家目录
# ~zhangsan/ 张三用户家目录 /home/zhangsan/
# ~root/ root用户家目录 /root/
```

# 查看文件、目录

```bash
ls # 一般查看
ls -lah # 查看所有，包括隐藏
ls -lah /etc/
ls -lah /etc/*.conf
# 可以指定文件、目录
ls -ldh /etc/
# 查看文件、目录本身
ls -lAh /etc/
# 不看./ ../
ls -lahR /etc/ 
# 递归查看所有
# - 文件
# d 目录
# l link
# b block
# c 字符设备
```

# 查看文件内容

```bash
cat /etc/services
less /etc/services
# 上下翻行、pageup pagedown上下翻页、q退出、/systemd搜索
head /etc/passwd -n 20
tail /etc/tasswd -n 20
```

# 目录文件增删改

## 创建

```bash
mkdir -p /root/linux运维/Linux网络管理/
# 递归创建目录
touch 1.txt
# 创建空文件
```

## 复制

```bash
cp -r /root/linux运维/ /root/Linux管理/
# 复制目录或文件
# 可以结合* 通配符
```

## 移动、重命名

```bash
mv /root/1.txt /root/2.txt
# 移动文件、或递归的移动目录，不用参数
# 重命名
# 可以结合通配符*
```

## 删除

```bash
rm -rf /root/1/
# 删除文件或目录
# 可以结合通配符*
rm -rf /*
# 删根操作，搞破坏的，谨慎使用！！！
```
