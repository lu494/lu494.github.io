---
layout: default
title: msf部署（git版、方便更新模块库）
---



# 卸载Kali apt版本msf

## 用apt卸载msf及依赖

```bash
apt purge metasploit-framework -y # purge 清洗
apt autoremove --purge -y # 自动清理没用的依赖
```

## 清除/usr/share/metasploit-framework/目录

```bash
ls -lah /usr/share/metasploit-framework/ #usr不是user，而是Unix system resource，放系统资源的，share 目录放一些共享的系统资源,r recursive 递归 f force 强制
sudo rm -rf /usr/share/metasploit-framework/
```

## 清除/opt/metasploit-frame/目录

```bash
ls -lah /opt/metasploit-frame/ # /opt/目录 optional ，可选的，一般应用程序装这
sudo rm -rf /opt/metasploit-framework 
```

## 清除所有用户的~/.msf4/目录

```bash
ls -lah /root/.msf4/ # root家目录下的 .msf4目录，.开头的是隐藏文件，放root用户的msf数据
rm -rf root/.msf4/
ls -lah kali/.msf4/
rm -rf Kali/.msf4/
```

## 清除/usr/bin/目录下的msf命令

### 通过find命令查看/usr/bin/目录下的msf命令

```bash
find /usr/bin/ -name msf* #找名， 在/usr/bin/目录下找文件或目录，咋找呢，按名找，名是啥呢？名是msf*，得加通配符
```

### 通过ls命令查看/usr/bin/目录下的msf命令

```bash
ls -lah /usr/bin/msf*
ls -lah /usr/bin/ | grep msf # grep命令找内容，不用通配符
grep msf /usr/bin/ -r # 找内容，找msf，在/usr/bin/目录下递归的找，不用通配符
```

### 清除/usr/bin/目录下的msf命令

```bash
rm -rf /usr/bin/msf*
```

## 验证msf被卸载

### 通过which寻找msfconsole

```bash
which msfconsole # msfconsole 执行msfconsole 命令 ，系统从path变量中寻找，找到了，执行。which msfconsole ，系统从path变量中寻找，找到了，不执行，而是输出msfconsole程序位置，所以which 用来输出命令程序的位置
```

### 通过dpkg寻找metasploit

```bash
dpkg -l | grep metasploit # dpkg Debian package 管理工具，l列出所有软件包
# ii zsh 5.9-2 amd64 shell with lots of features（feature 特征）
# ii （installed）包名-版本-架构-描述
```

# 安装msf软件依赖

## 挂代理，使用Kali官方软件源

```bash
cd /root/app/iptables-redsocks-clash/
./iptables_on.sh
redsocks -c /etc/redsocks.conf
vim /etc/apt/sources.list
```

## 下载Kali官方源公钥

```bash
\curl -fsSL https://archive.kali.org/archive-key.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/kali-archive-key.gpg
# \ 只用真curl，不用别名curl
# curl 用于从指定url下载文件
# f fail silent s silent S show error L location 自动重定向 ，-fsSL用于干净的下载
# gpg（GNU privacy guard ）管理密钥的工具
# 公钥私钥体系功能：1）加密2）源认证3）完整性认证
# de armor 把armor（装甲、指ascII）变成gpg格式，-o输出
# archive 存档
```

## 安装依赖

```bash
apt update -y
apt install -y git curl wget gnupg2 build-essential libreadline-dev libssl-dev libpq-dev libsqlite3-dev zlib1g-dev libxml2-dev libxslt1-dev libyaml-dev 
```

# 安装rvm

## rvm是啥？

> * rvm是ruby语言 version manager
> 
> * <mark>***msf是ruby语言写的***</mark>
> 
> * 通过rvm在Kali上安装不同版本的ruby运行环境，给不同ruby应用使用

## 安装rvm稳定版

```bash
\curl -fsSL https://get.rvm.io | bash -s stable
# \ 不要别名
# -fsSL 用于干净的拉取
# https://get.rvm.io rvm官方安装脚本
# | bash ，脚本给bash执行
# -s stdin 管道脚本搭配
# stable 稳定的，是参数，传给rvm安装脚本，指明安装rvm稳定版
```

## 根据提示下载rvm公钥，然后重新执行rvm安装命令

### 从公钥服务器下

```bash
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
# --keyserver 密钥服务器
# --recv-keys 下载公钥
# rvm官方给的公钥指纹
```

### 从官方下

```bash
command curl -fsSL https://rvm.io/mpapis.asc | gpg2 --import
# 这个也行
# command 表示执行真正的curl，不要别名，不要函数
# curl下东西
# -fsSL干净的下
# https://rvm.io/mpapis.asc从这下
# 下完给gpg2
# gpg可以、gpg2也行
# --import 引入到gpg2里头
gpg --list-keys
# 查看公钥列表
```

## rvm安装目录

> * 根据提示，可见rvm安装目录为<mark>***/usr/local/rvm/***</mark>

## 自己加到rvm组

```bash
usermod -aG rvm root
id root
reboot
```

## 加载rvm

```bash
source /etc/profile.d/rvm.sh
# rvm加载目录和安装目录不一样
# rvm加载目录是简化了的，用于在shell中使用rvm
# 只有加载了，才能在shell中使用
```

## 配置自动加载rvm

### 原理

> * root用户的shell配置文件有两个：登录式shell配置、交互式shell配置
> 
> * 登录式shell：通过物理机登录时、通过ssh登录时，zsh  -l
> 
> * 交互式shell：每打开一个terminal，zsh -i
> 
> * 在root用户的交互式shell配置里添加source /etc/profile.d/rmv.sh
> 
> * root使用shell，加载配置文件，加载rvm
> 
> * root用户的交互式zsh配置文件路径：/root/.zshrc
> 
> * zshrc：zsh run command

### 配置

```bash
echo 'source /etc/profile.d/rvm.sh' >> /root/.zshrc
# >> 追加到文件末尾
# > 覆盖文件
```

### 新terminal，验证

```bash
rvm --version
type rvm | head -n 1 
# type 查看命令类型 
# head -n 1 查看头1行
# 如果显示shell function加载成功了
# 如果显示/usr/local/rvm/scripts/cli，外部命令，没加载成功
```

## 安装ruby工具bundler

```bash
gem install bundler
# gem是ruby语言的软件模块包
# bundler 捆绑，是用来解决ruby软件依赖的工具
```

# 安装msf

## github拉取msf源码包

### 找到msf项目官方地址

> * 登录github
> 
> * 搜索msf项目url：https://github.com/rapid7/metasploit-framework
> 
> * rapid7是组织
> 
> * metasploit-framework是项目
> 
> * 加上.git就是我们git的目标：https://github.com/rapid7/metasploit-framework.git

### 选择本地安装目录

> * ruby语言是解释型语言，源码下回来直接运行，不用安装编译
> 
> * 第三方应用程序安装目录通常：/opt/

### 开始拉取

```bash
cd /opt/
git clone https://github.com/rapid7/metasploit-framework.git 
ls -lah /opt/metasploit-framework/
```

## 根据源码包指定版本安装ruby

### 查看要求ruby版本

```bash
cat /opt/metasploit-framework/.ruby-version
```

### 安装

```bash
rvm install 3.2.5
# compiling 编译
```

### ruby安装目录

> * <mark>/usr/local/rvm/rubies/ruby-3.2.5</mark>

### 查看安装的ruby版本

```bash
rvm list  
# =* ruby-3.2.5 [ x86_64 ]
# 
# => - current
# =* - current && default
#  * - default
# 安装了ruby-3.2.5，是默认的，是当前的
# 但没用，具体的项目，会根据.ruby-version文件选择自己用的ruby版本
```

## 根据源码创建指定创建gemset

### 概念

> * gemset就是这个项目的gems依赖包安装路径

### 查看项目要求gemset

```bash
cat /opt/metasploit-framework/.ruby-gemset 
```

### 创建gemset

```bash
rvm use ruby-3.2.5@metasploit-framework --create 
```

### gemset目录

> * <mark>/usr/local/rvm/gems/ruby-3.2.5@metasploit-framework*</mark>

### 查看gemset

```bash
rvm gemset list                                
# gemsets for ruby-3.2.5 (found in /usr/local/rvm/gems/ruby-3.2.5)
#    (default)
#    global
# => metasploit-framework
# 使用的是metasploit-framework，但没用，具体项目会根据自己的.ruby-gemset文件使用指定的gemset目录存放依赖gems
```

### 注意

> * 通过rvm use ruby-3.2.5@metasploit-framework指定的version、gemset和具体项目通过.ruby-version、.ruby-gemset指定的version、gemset的关系是谁后执行谁有效。
> 
> * rvm use 指定的version、gemset给当前shell用，用于调试
> 
> * .ruby文件指定的version、gemset给项目用

## 安装gems(依赖)

### 使用bundle命令安装

```bash
cd /opt/metasploit-framework/
bundle install
# bundle是bundler工具的命令，意思是捆绑，用于给项目装gems依赖包
# Gemfile文件指明依赖关系
# Gemfile.lock文件指明具体的gems依赖包
# bundle工具根据Gemfile、Gemfile.lock文件下载gems依赖包
# .ruby-version指明项目的ruby版本
# .ruby-gemset指明这个项目的gems依赖包装到哪
```

### 验证项目的gems安装目录

```bash
rvm gemset list
gem env home
```

# 初始化msf

## 运行

```bash
cd /opt/metasploit-framework/
bundle exec msfconsole
```

## 模块介绍

> * 所有的漏洞相关脚本都在module里
> 
> * auxiliary 辅助模块
> 
> * encoders 编码器
> 
> * evasion 逃跑、绕过模块
> 
> * exploits 利用模块
> 
> * nop no operation 指令，滑行器
> 
> * payloads 载荷，漏洞的载荷，临时内存马
> 
> * post 后渗透模块

## 更新module

```bash
cd /opt/metasploit-framework/
git log -1 # 查看本地msf版本 
git remote update # 存放远程信息的地方 更新
git status # 检查本地msf是否为最新
git pull # 拉取最新源码
bundle install # 安装gems依赖
```

## 配置数据库

```bash
systemctl enable postgresql --now
systemcel status postgresql
psql --version # 查看postgresql 版本
sudo -u postgres psql # sudo 超级执行，-u postgres 不是root执行，而是postgres执行，自行psql，这是postgresql的命令行工具
ALTER DATABASE template1 REFRESH COLLATION VERSION;
-- 替换数据库模板1，模板1用来创建新数据库
-- refresh 刷新 collation 排序规则 version 版本
DROP DATABASE msf;
DROP USER msf;
CREATE USER msf WITH PASSWORD 'msf';
CREATE DATABASE msf OWNER msf;
GRANT ALL PRIVILEGES ON DATABASE msf TO msf;
-- 授权
\q
psql -U msf -h 127.0.0.1 -d msf # 测试postgresql能否连接
netstat -tunlp | grep 5432 # tcp udp 数字 只显示listening 显示pid
mkdir -p ~/.msf4/
vim ~/.msf4/database.yml
# yml 文件是一种配置文件
# 写入以下内容
production:
  adapter: postgresql
  database: msf
  username: msf
  password: msf
  host: 127.0.0.1
  port: 5432
  pool: 75
# pool75,并发连接池75，能同时和数据库建立75个并发连接
  timeout: 5
# 5秒连不上就报错
cd /opt/metasploit-framework/
bundle exec msfconsole
workspace # 工作空间，一个项目一个空间
```


