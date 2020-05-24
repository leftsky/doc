笔记
=========

## git 设置全局代理

`git config --global http.proxy 'socks5://127.0.0.1:1080'` 

## comopser 设置源

`composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/` 

## VM

VM15 密钥： `UY758-0RXEQ-M81WP-8ZM7Z-Y3HDA` 

## php-sqlsrv扩展

解压so动态链接库文件到 `/sbin/` 目录下

在PHP配置文件中添加
`extension=/sbin/php_pdo_sqlsrv_72_nts.so` 

## Android Studio

### AS 设置最大使用内存

找到启动目录下的执行文件 例：studio64.exe

编辑执行文件名.vmoptions内容的-Xmx就是允许使用的最大内存

## 开启宝塔 FTP 日志

#### 开启宝塔FTP日志 

安装很简单，配置不简单，第一次看纯英文的帮助文档，当然偶尔也Google一下，但是发现关于pure-ftp的不是很多……

1)建立文件/var/log/pureftpd.log

2)修改/etc/rsyslog.conf

1>在这行的cron.none后面添加 ; ftp.none 使ftp的日志信息成私有

`*.info; mail.none; authpriv.none; cron.none /var/log/messages` 为 
 
`*.info; mail.none; authpriv.none; cron.none; ftp.none /var/log/messages` 

2>在/etc/rsyslog.conf文件最后加上

#### pureftp 日志

ftp.* -/var/log/pureftpd.log

注意: 不要去掉/var前面的-号, 否则日志会在/var/log/messages与/var/log/purefpd.log里各记录一份. 添加了-号, 就只会记录在/var/log/purefptd.log内

3)使/etc/syslog.conf生效

重启系统日志 service rsyslog restart

重启puerftpd service pure-ftpd restart

## Laravel

#### telescope

`composer require laravel/telescope` 

`php artisan telescope:install` 

`php artisan migrate` 

`php artisan telescope:prune` 

app/Console/Kernel.php -> public function schedule()

`$schedule->command('telescope:prune --hours=48')->daily();` 

#### 上传文件link

`php artisan storage:link` 

public 替换成storage

## mysql

#### mysql授权远程登陆

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '1qaz!QAZ' WITH GRANT OPTION;` 

注意普通安装mysql需求检查my.cnf里的bind不要绑定死本地。

#### 解决mariadb默认无密码

`UPDATE mysql.user SET authentication_string = PASSWORD('1qaz!QAZ'), plugin = 'mysql_native_password' WHERE User = 'root' AND Host = 'localhost';` 

## Fastadmin

#### 一键crud

`php think crud -t 表名 -u 1` 

  更新↓

`php think crud -t users -u 1 --force=true` 

#### 自动跳转登录页

`<meta http-equiv="refresh" content="0; url=./xxx.php/index/login" />` 

## Linux

### 开机启动

```
vim /etc/rc.local
nohup /home/your_program >> /home/your_program.txt &
```

& 代表后台运行，不阻塞 此段代码加到exit 0前即可

#### Debian

###### Debian 9 阿里云源

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

#### Centos

##### Centos7 修改为阿里源
```
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bak
wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all
yum makecache
```

#### Ubuntu

###### Ubuntu 阿里源

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

#### 树莓派

###### 远程桌面

``` 
apt install tightvncserver
apt install xrdp
reboot
```

###### 设置树莓派静态IP

``` 
vim /etc/dhcpcd.conf
interface eth0
static ip_address=121.248.54.54/24
static routers=121.248.54.55
static domain_name_servers=121.248.0.1 8.8.8.8
```

###### 树莓派源

``` 
sudo sed -i 's#://raspbian.raspberrypi.org#s://mirrors.ustc.edu.cn/raspbian#g' /etc/apt/sources.list
``` 

``` 
sudo sed -i 's#://archive.raspberrypi.org/debian#s://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian#g' /etc/apt/sources.list.d/raspi.list 
``` 

###### 制造树莓派 img 镜像

在ubuntu上新建sh，树莓派系统TF卡插入系统，会增加两个挂载地址，分别作为1，2参数

./xx.sh /boot /root

```
#!/bin/sh

if [  $# != 2 ]; then
  echo "argument error: Usage: $0 boot_device_name root_device_name"
  echo "example: $0 /dev/mmcblk0p1 /dev/mmcblk0p2"
  exit 0
fi
dev_boot=$1
dev_root=$2
mounted_boot=`df -h | grep $dev_boot | awk '{print $6}'`
mounted_root=`df -h | grep $dev_root | awk '{print $6}'`
img=rpi-`date +%Y%m%d-%H%M`.img

#install tools
sudo apt-get install dosfstools dump parted kpartx

echo =====================   part 1, prepare workspace    ===============================
mkdir ~/backupimg
cd ~/backupimg

echo ===================== part 2, create a new blank img ===============================
# New img file
#sudo rm $img

bootsz=`df -P | grep $dev_boot | awk '{print $2}'`
rootsz=`df -P | grep $dev_root | awk '{print $3}'`
totalsz=`echo $bootsz $rootsz | awk '{print int(($1+$2)*1.3/1024)}'`
sudo dd if=/dev/zero of=$img bs=1M count=$totalsz
#sync
echo "...created a blank img, size ${totalsz}M "

# format virtual disk
bootstart=`sudo fdisk -l | grep $dev_boot | awk '{print $2}'`
bootend=`sudo fdisk -l | grep $dev_boot | awk '{print $3}'`
rootstart=`sudo fdisk -l | grep $dev_root | awk '{print $2}'`
echo "boot: $bootstart >>> $bootend, root: $rootstart >>> end"
#有些系统 sudo fdisk -l 时boot分区的boot标记会标记为*,此时bootstart和bootend最后应改为 $3 和 $4
#rootend=`sudo fdisk -l /dev/mmcblk0 | grep mmcblk0p2 | awk '{print $3}'`

sudo parted $img --script -- mklabel msdos
sudo parted $img --script -- mkpart primary fat32 ${bootstart}s ${bootend}s
sudo parted $img --script -- mkpart primary ext4 ${rootstart}s -1

echo =====================  part 3, mount img to system  ===============================
loopdevice=`sudo losetup -f --show $img`
device=/dev/mapper/`sudo kpartx -va $loopdevice | sed -E 's/.*(loop[0-9])p.*/\1/g' | head -1`
sleep 5
sudo mkfs.vfat ${device}p1 -n boot
sudo mkfs.ext4 ${device}p2 -L rootfs
#在backupimg文件夹下新建两个文件夹，将两个分区挂载在下面
mkdir tgt_boot tgt_Root
#这里没有使用id命令来查看uid和gid，而是假设uid和gid都和当前用户名相同
uid=`whoami`
gid=$uid
sudo mount -t vfat -o uid=${uid},gid=${gid},umask=0000 ${device}p1 ./tgt_boot/
sudo mount -t ext4 ${device}p2 ./tgt_Root/


echo ===================== part 4, backup /boot =========================
sudo cp -rfp ${mounted_boot}/* ./tgt_boot/
sync
echo "...Boot partition done"

echo ===================== part 5, backup / =========================
sudo chmod 777 ./tgt_Root
sudo chown ${uid}.${gid} tgt_Root
sudo rm -rf ./tgt_Root/*
cd tgt_Root/
# start backup
sudo dump -0uaf - ${mounted_root}/ | sudo restore -rf -
sync 
echo "...Root partition done"
cd ..

echo ===================== part 6, replace PARTUUID =========================

# replace PARTUUID
opartuuidb=`sudo blkid -o export $dev_boot | grep PARTUUID`
opartuuidr=`sudo blkid -o export $dev_root | grep PARTUUID`
npartuuidb=`sudo blkid -o export ${device}p1 | grep PARTUUID`
npartuuidr=`sudo blkid -o export ${device}p2 | grep PARTUUID`
sudo sed -i "s/$opartuuidr/$npartuuidr/g" ./tgt_boot/cmdline.txt
sudo sed -i "s/$opartuuidb/$npartuuidb/g" ./tgt_Root/etc/fstab
sudo sed -i "s/$opartuuidr/$npartuuidr/g" ./tgt_Root/etc/fstab
echo "...replace PARTUUID done"

echo "remove auto generated files"
#下面内容是删除树莓派中系统自动产生的文件、临时文件等
cd ~/backupimg/tgt_Root
sudo rm -rf ./.gvfs ./dev/* ./media/* ./mnt/* ./proc/* ./run/* ./sys/* ./tmp/* ./lost+found/ ./restoresymtable
cd ..

echo ===================== part 7, unmount =========================
sudo umount tgt_boot tgt_Root
sudo kpartx -d $loopdevice
sudo losetup -d $loopdevice
rmdir tgt_boot tgt_Root


echo "==== All done. img file is under ~/backupimg/ "
```


## geth

#### 指定数据目录启动

`./geth --datadir "D:/eth_data/"` 

## nodejs

#### cnpm

`npm install -g cnpm --registry=https://registry.npm.taobao.org` 

#### 替换阿里源

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
