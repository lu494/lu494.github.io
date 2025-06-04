---
layout: default
title: scp传文件
---

# 清屏

```bash
clear
```

# scp命令传文件

```batch
rem 通过scp命令，通过ssh管道传输文件
rem 我通过ssh连接到你了，本质上你的shell反弹给我了，我是我，你是目标
rem 我的文件能往目标上上传，目标上的文件能下载到我这来
rem 但是操作只能在我这进行，目标上进行不了
scp -r c:\users\xlu\desktop\1.txt root@172.20.0.10:/root/2.txt
scp -r root@172.20.0.10:/etc/passwd passwd.txt
rem r recursive 递归
```
