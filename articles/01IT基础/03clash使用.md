---
layout: default
title: clash使用
---

# 基本原理

> * clash 选择全局模式，所有经过clash的流量，都整到clash指定的代理服务器上去
> 
> * 开启虚拟网卡模式（隧道模式），所有的流量都走clash
> 
> * 不开隧道、不开系统代理，clash默认混合监听<mark>tcp7897（HTTP、socks5）</mark>，<mark>udp:53（就是0.0.0.0:53</mark>，所有网卡的53端口）
> 
> * 配置浏览器的http代理指向clash，就可使用，因为浏览器的dns解析使用的是remote dns解析
> 
> * 配置Kali运行redsocks、iptables就可使用，所有tcp流量经由iptables到redsocks到clash，dns流量经由iptables到clash
> 
> * 而其余所有的别的流量，不走clash。dns走本地、tcp走本地。

# 配置Kali虚拟机流量走物理机clash

## 部署redsocks建立Kali系统到clash的隧道

1.安装

```bash
apt install redsocks -y
```

2.配置

```bash
# 验证Kali和物理机联通性
# ping 172.20.0.1
# clash界面可以看到混合代理端口
# 混合代理指的是http和socks的混合，HTTP在浏览器走clash时已验证
vim /etc/redsocks.conf
# 修改如下内容
base {
  log_debug = off;
  log_info = on;
  daemon = on;
  redirector = iptables;
}
redsocks {
  local_ip = 127.0.0.1;
  local_port = 12345;
  ip = 172.20.0.1;
  port = 7897;
  type = socks5;
}
```

## 创建iptables_on.sh脚本，使Kali流量发到redsocks

1.什么是iptables？

> * iptables 使Linux内核的防火墙，默认开启
> 
> * 我们通过添加规则删除规则的方式达到“开闭iptables”

2.

```bash
mkdir -p /root/app/iptables-redsocks-clash/ # p parents 
cd /root/app/iptables-redsocks-clash/
vim iptables_on.sh
# 添加如下内容
#!/bin/bash
# 使用/bin/bash来执行
iptables -t nat -N REDSOCKS
# table：nat，new一个较REDSOCKS
iptables -t nat -A REDSOCKS -d 0.0.0.0/8 -j RETURN
# append 挂到REDSOCKS上，destination是0.0.0.0/8的，jump 到 return，不处理
iptables -t nat -A REDSOCKS -d 127.0.0.0/8 -j RETURN
# 127.0.0.0/8的也不处理
iptables -t nat -A REDSOCKS -d 172.20.0.0/24 -j RETURN
# 172.20.0.0/24的也不处理
iptables -t nat -A REDSOCKS -p tcp -j REDIRECT --to-ports 12345
# protocol是tcp的，redirect到127.0.0.1:12345端口
iptables -t nat -A OUTPUT -p tcp -j REDSOCKS
# 所有tcp的流量都整到OUTPUT里，往外走，走到REDSOCKS里
iptables -t nat -A OUTPUT -p udp --dport 53 -j DNAT --to-destination 172.20.0.1:53
# udp53的dns查询流量，整到clash dns覆写里
chmod +x iptables_on.sh
```

## 创建iptables_off.sh脚本，清除iptables规则

```bash
vim iptables_off.sh
# 添加如下内容
#!/bin/bash
iptables -t nat -D OUTPUT -p tcp -j REDSOCKS
# 删除OUTPUT里的这条规则，tcp的流量不往REDSOCKS里整了
iptables -t nat -D OUTPUT -p udp --dport 53 -j DNAT --to-destination 172.20.0.1:53
# 删除dns查询流量往clash里整
iptables -t nat -F REDSOCKS
# flush REDSOCKS,，清空REDSOCKS中全部规则
iptables -t nat -X REDSOCKS
# X 删除REDSOCKS链
chmod +x iptables_off.sh
```

## 开闭iptables和redsocks

1.iptables

```bash
# iptables是Linux内核进程一直开着
# 添加或删除规则后就会立即生效
cd /root/app/iptables-redsocks-clash/
./iptables_on.sh
./iptables_off.sh
```

2.redsocks

```bash
redsocks -c /etc/redsocks.conf
# 开启redsocks
ps aux | grep redsocks
# process status all user-friendly x 所有
# root 2715  0.0  0.0  6680  2232 pts/0  S+  10:23  0:00 grep --color=auto redsocks，这个不是redsockets进程，而是 grep --color=auto redsocks进程本身
kill 2715 
```
