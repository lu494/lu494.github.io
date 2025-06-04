---
layout: default
title: 05Linux电源操作
---

# Linux/cmd电源操作

```batch
shutdown /s /f /t 0
rem 立即关机
shutdown /s /f /t 3600
rem 一小时后关机
shutdown /h
rem 休眠
shutdown /r /f /t 0
rem 重启
```

```bash
init 0 
# 关机
init 6 
# 重启
```


