---
layout: default
title: nmap基本使用
---



# 介绍一下nmap

> * nmap 工具是用来<mark>网络探测</mark>的
> 
> * 探测哪些<mark>主机</mark>存活
> 
> * 探测什么<mark>端口</mark>开放
> 
> * 探测操作<mark>系统版本</mark>
> 
> * 探测<mark>中间件版本</mark>

# 主机探测

```bash
nmap -sP 172.20.0.0/24 # ping 主机探测 
```

# syn扫描单台主机端口、服务版本、系统版本

```bash
nmap -sS -sV -O 112.125.18.22
# S syn扫描，V 中间件版本扫描，O 操作系统版本扫描
```

# tcp扫描单台主机端口、服务版本、系统版本

```bash
nmap -sT -sV -O 112.125.18.22 # TCP扫描更准确，更慢，更容易被拒绝
```

# 扫描udp端口

```bash
nmap -sU 112.125.18.22
# udp扫描，扫描udp端口的，老慢了
```

# 指定端口扫描

```bash
nmap -sT -sV -O 112.125.18.22 -p 20-100,443,3389 # 指定端口扫描 
```


