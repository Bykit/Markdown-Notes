### 1、wget工具安装

> wget 是一个从网络上自动下载文件的自由工具，支持通过 HTTP、HTTPS、FTP 三个最常见的 TCP/IP协议下载，并可以使用 HTTP 代理。"wget" 这个名称来源于 “World Wide Web” 与 “get” 的结合。

```sh
yum -y install wget
# yum命令格式：yum 选项 包名/软件组名/关键字
# -y  自动回答yes
# install 安装
```

### 2、配置阿里云yum源

> CentOS7 配置阿里云yum源

```sh
cd  /etc/yum.repos.d/
wget  http://mirrors.aliyun.com/repo/Centos-7.repo
#用wget下载repo文件,下载的文件会在当前目录
mv  CentOs-Base.repo CentOs-Base.repo.bak
#通过改名，使原yum源不生效，备份系统原来的repo文件
mv Centos-7.repo CentOs-Base.repo
#通过改名，使下载的yum源生效

yum clean all      #清楚缓存
yum makecache      #创建元数据缓存
#上面两条命令不明白
```

