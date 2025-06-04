---
layout: default
title: SSH安全配置
---

# 查看ssh服务

```bash
systemctl status ssh
systemctl status sshd
netstat -tunlp | grep 22
# tcp udp listening numberic pid 
```

# 重启ssh服务

```bash
systemctl restart sshd || systemctl restart ssh
/etc/init.d/ssh restart
service ssh restart
```

# SSH配置文件

## 位置

> * `vim /etc/ssh/sshd_config`

## 登录优雅时间

> * `LoginGraceTime 1m`
> 
> * 登录优雅时间1分钟
> 
> * 从连上开始算，给一分钟认证，一分钟后没登上，断开重连

## 最大登录次数

> * `MaxAuthTries 3`
> 
> * 三次错误就锁定

## 允许root登录不？

> * `PermitRootLogin no`

## 白名单

> * `AllowUsers zhangsan@192.168.1.1`

## 端口

> * `Port 22222`

# 超时时间

## 什么是超时时间？

> * 超时时间指的是shell的超时时间
> 
> * 和ssh无关

## 查看当前shell超时时间

```bash
echo $TMOUT
```

## 给当前shell配置超时时间

```bash
export TMOUT=30
# 出口到环境变量里
# 设置环境变量
# 给当前shell设置环境变量
# 超时时间为30秒
```

## 通过系统shell配置给系统设置超时时间

```bash
echo "export TMOUT=30" >> /etc/profile
echo "export TMOUT=30" >> /etc/bashrc
验证
sed -i '$d' /etc/profile
sed -i '$d' /etc/bash.bashrc
```

## 通过用户shell配置给用户设置超时时间

```bash
echo "export TMOUT=30" >> ~/.bash_profile
echo "export TMOUT=30" >> ~/.bashrc
验证
sed -i '$d' ~/.bash_profile
sed -i '$d' ~/.bashrc
```

# 配置ssh连接保活

## 配置文件

> * 配置文件有两个
> 
> * 1是/etc/ssh/sshd_config
> 
> * 2是/etc/ssh/ssh_config

## /etc/ssh/ssh_config

```bash
Host *
ServerAliveInterval 30 
ServerAliveCountMax 100
# 客户端30秒向服务端发一个探测包
# 连续发了100个探测包服务端都没响应，判断为网络故障，断开ssh连接
```

## /etc/ssh/sshd_config

```bash
ClientAliveInterval 60
ClientAliveCountMax 50
# 服务端每60秒向客户端发一个探测包
# 连续50个探测包没有回应，认为客户端失联，断开ssh
```

## 作用

> * 如果服务端和客户端的AliveInterval都是0，都不发探测包：
> 
> * 1）中间设备容易给ssh连接踢掉
> 
> * 2）中间网络断开，两端无感知，永远连接
> 
> * 如果服务端和客户端的AliveInterval都比较小，AliveCountMax都比较大：
> 
> * 1）持续发送保活包，保持中间设备不踢
> 
> * 2）如果网络丢包，有一定的容忍度，在一定时间内保持ssh连接
> 
> * 3）如果网络真坏了，在一定时间后回自动断开连接，释放资源

# 密码有效期、密码长度

## 密码有效期是什么？

> * 用来配置密码最长用多少天？
> 
> * 密码最短用多少天？
> 
> * 过期前多少天开始警告？
> 
> * 密码有效期是系统的和ssh无关

## 配置文件

```bash
vim /etc/login.defs
PASS_MAX_DAYS 99999 # 永不过期
PASS_MIN_DAYS 0 # 立即可以改密码
PASS_WARN_AGE 7 # 过期前7天开始警告
PASS_MIN_LEN 5 # 密码最短5位
# 注意这些配置对新创建的用户生效
# 对已有用户，密码策略保持原样
```

## 修改已有用户密码策略

```bash
chage -m 0 root # 最小使用期限
chate -W 7 root # 过期前警告天数
chage -M 180 root # 最长使用期限
chage -l root # 列出密码策略 
```

# ssh密码认证、ssh密钥 认证

## 公钥私钥作用

> * 私钥端给公钥端发信息，认证、签名
> 
> * 公钥端给私钥端发信息，加密通信
> 
> * 总的来说，主机-私钥-公钥是强绑定的，
> 
> * 我信任了公钥就信任了私钥信任了你
> 
> * 私钥标识了你，你的私钥只有自己有
> 
> * 私钥用来识别身份，也就是我们说的签名、认证

## ssh密码认证过程

> 1. tcp连接，协商参数，加密认证方式
> 
> 2. 服务端自动生成私钥公钥对，把公钥发给客户端，我们接受公钥接受服务端，服务端有私钥认证通过。
> 
> 3. DH算法老牛了，在不传输关键信息的前提下，两端协商出同样的对称密钥，后续通信用对称密钥加密
> 
> 4. 客户端用密码在服务端认证
> 
> 5. 正常通信

## ssh 密钥认证过程

> 1. DH算法生成会话密钥：一个会话一变样
> 
> 2. 协商认证参数，客户端发公钥，说我想用这个公钥认证，服务端检查哟没有
> 
> 3. 服务端发送挑战值
> 
> 4. 客户端用私钥签名（就是加密）
> 
> 5. 服务端解密成功，认证成功

## 配置Windows用私钥登录Kali

### Windows生成密钥对

```batch
ssh-keygen -t rsa -b 4096 -C "xxx@163.com"
rem key generate
rem type rsa
rem bit 4096,更安全
rem comment注释评论，一般用邮箱注释，在公钥结尾标注
cd c:\users\xlu\.ssh\
dir
```

### 把公钥存到Kali中

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

```batch
type c:\users\xlu\.ssh\id_rsa.pub
复制公钥内容
```

```bash
echo "xxxxxxxxx== xxx@163.com" >> ~/.ssh/authorized_keys
# 保存公钥就相当于信任了公钥对应的私钥对应的主机
# 如果是密码认证，就相当于把对方的密码加到数据库里了
chmod 600 ~/.ssh/authorized_keys
```

### 配置ssh密钥登录

```bash
vim /etc/ssh/sshd_config
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes # 可选，保留密码登录
```

### 重启ssh服务

```bash
systemctl restart sshd || systemctl restart ssh
```

### ssh密钥登陆

```batch
ssh -i "c:\users\xlu\.ssh\id_rsa" root@172.20.0.4 -p 22222
rem i 私钥路径
rem cmd和 mobaxterm可使用
ssh root@172.20.0.4:22222
rem xhell使用
rem 选择密码登录或者密钥登录
echo $SHELL
rem 登上了看一下shell
```
