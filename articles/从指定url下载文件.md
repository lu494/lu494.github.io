---
layout: default
title: 从指定url下载文件
---

# Windows远程桌面协议

## 图形化

> * 像VMware tools一样，支持图形化文件的上传和下载

# Linux 从web上下载

## 通过wget

```bash
wget http://112.125.18.22/welcome.png
```

## 通过curl

```bash
curl http://112.125.18.22/welcome.png -o /root/1.png
```

# windows从web上下载

## curl

```batch
curl http://112.125.18.22/welcome.png -o welcome.png
```

## certutil

```batch
certutil.exe -urlcache -split -f http://112.125.18.22/welcome.png welcome.png
```

## power shell

```powershell
Invoke-WebRequest -Uri "http://112.125.18.22/welcome.png" -OutFile "welcome.png"
# 这是power shell命令
# power shell 区分大小写
# power shell 是跨平台shell，支持Windows、Linux、macos
# invoke 调用 
# uri uniform resource id 统一资源id ，就是url
```

## bitsadmin

```batch
bitsadmin /transfer "job1" "http://112.125.18.22/welcome.png" "c:\users\xlu\welcome.png"
rem bitsadmin工具不支持https协议
```

# 通过python

```batch
pip install requests 
rem pip 是python库下载工具
rem 通过pip安装python requests 库
python -c "import requests；url='http://112.125.18.22/welcome.png';filename='welcome.png';response=requests.get(url);open(filename,'wb').write(response.content)"
rem c 指的是python解释器不执行脚本文件，而是从命令行中获取脚本来执行
rem wb指的是write binary 书写二进制的方式
```

# 其他

> * 还可以通过<mark>ruby、Perl、java</mark>等语言的脚本从指定url下载文件
> 
> * 通过<mark>菱角社区</mark>查询 [查询文件下载命令](https://forum.ywhack.com/bountytips.php?download)

---
