笔记
=========

# git 设置全局代理

`git config --global http.proxy 'socks5://127.0.0.1:1080'` 

# comopser 设置源

`composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/` 

# VM

VM15 密钥： `UY758-0RXEQ-M81WP-8ZM7Z-Y3HDA` 

# php-sqlsrv扩展

解压so动态链接库文件到 `/sbin/` 目录下

在PHP配置文件中添加
`extension=/sbin/php_pdo_sqlsrv_72_nts.so` 

# 开启宝塔 FTP 日志

## 开启宝塔FTP日志 

安装很简单，配置不简单，第一次看纯英文的帮助文档，当然偶尔也Google一下，但是发现关于pure-ftp的不是很多……

1)建立文件/var/log/pureftpd.log

2)修改/etc/rsyslog.conf

1>在这行的cron.none后面添加 ; ftp.none 使ftp的日志信息成私有

`*.info; mail.none; authpriv.none; cron.none /var/log/messages` 为 
 
`*.info; mail.none; authpriv.none; cron.none; ftp.none /var/log/messages` 

2>在/etc/rsyslog.conf文件最后加上

## pureftp 日志

ftp.* -/var/log/pureftpd.log

注意: 不要去掉/var前面的-号, 否则日志会在/var/log/messages与/var/log/purefpd.log里各记录一份. 添加了-号, 就只会记录在/var/log/purefptd.log内

3)使/etc/syslog.conf生效

重启系统日志 service rsyslog restart

重启puerftpd service pure-ftpd restart

# Laravel

## telescope

`composer require laravel/telescope` 

`php artisan telescope:install` 

`php artisan migrate` 

`php artisan telescope:prune` 

app/Console/Kernel.php -> public function schedule()

`$schedule->command('telescope:prune --hours=48')->daily();` 

## 上传文件link

`php artisan storage:link` 

public 替换成storage

# mysql

## mysql授权远程登陆

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '1qaz!QAZ' WITH GRANT OPTION;` 

注意普通安装mysql需求检查my.cnf里的bind不要绑定死本地。

## 解决mariadb默认无密码

`UPDATE mysql.user SET authentication_string = PASSWORD('1qaz!QAZ'), plugin = 'mysql_native_password' WHERE User = 'root' AND Host = 'localhost';` 

# Fastadmin

## 一键crud

`php think crud -t 表名 -u 1` 

  更新↓

`php think crud -t users -u 1 --force=true` 

## 自动跳转登录页

`<meta http-equiv="refresh" content="0; url=./xxx.php/index/login" />` 

# Linux

## Debian

### Debian 9 阿里云源

``` 
deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb http://mirrors.aliyun.com/debian-security stretch/updates main
deb-src http://mirrors.aliyun.com/debian-security stretch/updates main
deb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib 
deb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib 
deb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib 
```

## Ubuntu

### Ubuntu 阿里源

``` 
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

## 树莓派

### 远程桌面

``` 
apt install tightvncserver
apt install xrdp
reboot
```

### 设置树莓派静态IP

``` 
interface eth0
static ip_address=121.248.54.54/24
static routers=121.248.54.55
static domain_name_servers=121.248.0.1 8.8.8.8
```

### 树莓派源

``` 
sudo sed -i 's#://raspbian.raspberrypi.org#s://mirrors.ustc.edu.cn/raspbian#g' /etc/apt/sources.list
``` 

``` 
sudo sed -i 's#://archive.raspberrypi.org/debian#s://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian#g' /etc/apt/sources.list.d/raspi.list 
``` 

# geth

## 指定数据目录启动

`./geth --datadir "D:/eth_data/"` 

# nodejs

## cnpm

`npm install -g cnpm --registry=https://registry.npm.taobao.org` 

## 替换阿里源

`将你的xml文件备份，内容全部替换成下方的内容` 
``` 
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd"> 
  <pluginGroups /> 
  <proxies /> 
  <servers /> 
  <mirrors > 
    <mirror> 
      <id>planetmirror.com</id> 
      <name>aliyun</name> 
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
      <mirrorOf>central</mirrorOf> 
    </mirror> 
  </mirrors> 
  <localRepository>C:\manveRepository</localRepository> 
</settings>
```
