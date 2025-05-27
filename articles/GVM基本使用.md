---
layout: default
title: GVM基本使用
---



# GVM是啥？

> * GVM是greenbone vulnerability management 绿骨漏洞管理
> 
> * <mark>绿骨漏扫</mark>
> 
> * 是系统、组件漏扫工具
> 
> * 是greenbone公司的（德国的）
> 
> * 前身是openvas，<mark>核心组件就是openvas</mark>（open vulnerability assessment system）
> 
> * openvas是开源的，GVM有社区版本可供我们白嫖
> 
> * GVM的漏洞库现在仍在持续更新
> 
> * 个人学习使用够用了

# 其他漏扫工具

> * <mark>Nessus essential</mark> Nessus 基础版，也是免费的，可以白嫖，好像也挺好用
> 
> * gvm和Nessus 有点关系
> 
> * 如果入职到公司咱就用公司购买的<mark>各个安全厂商的商业版漏扫</mark>

# GVM更新漏洞库

## 更新feed

> * win键》gvm feed update

## 重启gvm

```bash
systemctl restart ospd-openvas
systemctl restart gvmd
systemctl restart gsad
```

# 启动停止GVM

```bash
systemctl start ospd-openvas
systemctl start gvmd
systemctl start gsad
systemctl stop gsad
systemctl stop gvmd
systemctl stop ospd-openvas
```

# web访问GVM

> * https://172.20.0.4:9392

# 使用GVM扫描目标

## 创建端口列表

> * configure》port list》新建

## 创建目标主机

> * configure》targets》新建

## 创建扫描任务

> * sacns》tasks》new tasks







