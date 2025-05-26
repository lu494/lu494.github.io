# 从指定url下载文件

## 1.1 Windows远程桌面协议

> * 像VMware tools一样，支持图形化文件的上传和下载

## 1.2 Linux 从web上下载

1.

```bash
wget http://112.125.18.22/welcome.png # 1
```

2.

```bash
curl http://112.125.18.22/welcome.png -o /root/1.png # 1
```

## 1.3 windows从web上下载

1.

```batch
::1
curl http://112.125.18.22/welcome.png -o welcome.png
```

2.

```batch
::1
certutil.exe -urlcache -split -f http://112.125.18.22/welcome.png welcome.png
```

3.通过<mark>power shell</mark>

```powershell
Invoke-WebRequest -Uri "http://112.125.18.22/welcome.png" -OutFile "welcome.png" # 1
# 这是power shell命令  # 2
# power shell 区分大小写 # 3
# power shell 是跨平台shell，支持Windows、Linux、macos # 4
# invoke 调用  # 5
# uri uniform resource id 统一资源id ，就是url # 6
```

4.

```batch
::1
bitsadmin /transfer "job1" "http://112.125.18.22/welcome.png" "c:\users\xlu\welcome.png"
::2
rem bitsadmin工具不支持https协议
```

## 1.4 通过python

```batch
::1
pip install requests 
::2
rem pip 是python库下载工具
::3
rem 通过pip安装python requests 库
::4
python -c "import requests；url='http://112.125.18.22/welcome.png';filename='welcome.png';response=requests.get(url);open(filename,'wb').write(response.content)"
::5
rem c 指的是python解释器不执行脚本文件，而是从命令行中获取脚本来执行
::6
rem wb指的是write binary 书写二进制的方式
```

## 1.5 还可以通过<mark>***ruby、Perl、java***</mark>等语言的脚本从指定url下载文件

## 1.6 可以通过<mark>***菱角社区***</mark>进行查询

> * [查询文件下载命令](https://forum.ywhack.com/bountytips.php?download)

