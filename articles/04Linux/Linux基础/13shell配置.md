---
layout: default
title: shell配置
---

# shell配置咋回事？

> * shell配置是shell启动时加载的配置，是这个shell的默认配置
> 
> * 分为系统的和用户的
> 
> * 系统的对所有用户有效，用户的对当前用户有效
> 
> * 分为登陆的和交互的
> 
> * 登录shell是指输入了密码的
> 
> * 交互的是指有来有回的

# 查看当前shell属性

```bash
echo $-
# 有i是交互shell，例如himBHs
echo $0
# 开头有-就是登录shell，例如-bash
```

# 用户登录shell配置

```bash
vim ~/.bash_profile
vim ~/.bash_login
vim ~/.profile
# 按顺序匹配，匹配即停止
# 有哪个用哪个
```

# 用户交互shell配置

```bash
~/.bashrc
```

# 系统登录shell配置

```bash
vim /etc/profile 
ls /etc/profile.d/
ls -lah /etc/profile.d/
 # 系统登录shell配置子模块
chmod 644 /etc/profile.d/tmout.sh
```

# 系统交互shell配置

```bash
vim /etc/bashrc
```
