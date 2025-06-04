---
layout: default
title: Linux更改主机名
---

# 查看主机名

```bash
hostname
```

# 使用hostnamectl修改主机名

```bash
hostnamectl set-hostname www9
```

# 验证/etc/hostname配置文件

```bash
vim /etc/hostname
```

# 验证hosts本地dns解析文件

```bash
vim /etc/hosts
```


