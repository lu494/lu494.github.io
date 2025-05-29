---
layout: default
title: 04管理dnf软件源
---

# 什么是dnf源？

> * dnf<mark> 是yum源的升级版</mark>

# dnf源配置文件在哪？

```bash
cd /etc/yum.repos.d/
ls -lah
vim openEuler.repo
```

# 更改dnf源配置文件，重新构建缓存

```bash
mv openEuler.repo openEuler.guanfang
vim openEuler.Tsinghua
vim openEuler.aliyun
vim openEuler.huaweicloud
mv openEuler.Tsinghua openEuler.repo
# 使用清华源
dnf clean all
dnf makecache
```

# 仓库列表

```bash
dnf repolist
```

# 检查是否有更新

```bash
dnf check-update
```

# 更新

```bash
dnf upgrade -y
```
