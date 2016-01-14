---
layout: post
title: "Archlinux-2015.02.01 安装"
date: 2015-02-16 20:43:10 +0800
comments: true
categories: archlinux
---
## 准备安装
之前有装有 ubuntu ，分给 / 有 50g ，然后因为我有 16G 内存，  
swap 就只分了 512M ，现在发现就是可能没法休眠，因为 swap 太小了放不下内存的数据。。  
分区状态：

    sda           8:0    0 238.5G  0 disk  
    ├─sda1        8:1    0   384M  0 part  /boot/efi
    ├─sda2        8:2    0   128M  0 part  
    ├─sda3        8:3    0    99G  0 part  
    ├─sda4        8:4    0  88.5G  0 part  
    ├─sda5        8:5    0  49.5G  0 part  /
    └─sda6        8:6    0   512M  0 part  [SWAP]
    sdb           8:16   0 931.5G  0 disk  
    └─md126       9:126  0 931.5G  0 raid1 
      └─md126p1 259:0    0 931.5G  0 md    /run/media/kokutou/HitachiRaid1
    sdc           8:32   0 119.2G  0 disk  
    ├─sdc1        8:33   0   382M  0 part  
    └─sdc2        8:34   0 118.6G  0 part  
    sdd           8:48   0 931.5G  0 disk  
    └─md126       9:126  0 931.5G  0 raid1 
      └─md126p1 259:0    0 931.5G  0 md    /run/media/kokutou/HitachiRaid1

sda 是个 256g 的固态，放了win7，  
然后分区表是 gpt ，  
sda1 是 efi 分区，  
sda2 是微软的保留分区，  
sda3 是 win7 系统盘，  
sda4 是win7 下的 D 盘。  
sda5 装了 ubuntu ，安装的时候就不用分区了，直接格式化掉就行了。  

## 写入安装镜像
找了个 4g 的 u盘 ，直接用 dd 把镜像给写进去了。  
用的是 windows 下的 cygwin 里面的 dd ，没用那个 安装助手，安装助手 在 uefi 下面没一个能启动的。。。  

cygwin 下用 dd 不能用卷标，得 ```cat/proc/partitions``` 来得到 类似 /dev/sdx 的磁盘路径才行。  
*不要搞错，否则数据就没了 ^_^*  
写入命令是：  

    dd if=image.iso of=/dev/sdx（你的u盘） bs=4M

然后重启选择u盘启动，就进到 shell 可以开始安装了。

## 安装
基本就是参考 wiki 上的来的。  

键盘布局我没设置。  
终端字体我也没设置。  

locale 设置了下，注释掉了 en_US.UTF-8 和 zh_CN.* 和 zh_HK.*  
如下：

    en_US.UTF-8 UTF-8
    zh_CN.UTF-8 UTF-8  
    zh_CN.GB18030 GB18030  
    zh_CN.GBK GBK 
    zh_CN GB2312  
    zh_TW.UTF-8 UTF-8  
    zh_TW.EUC-TW EUC-TW  
    zh_TW BIG5  

然后 ```locale-gen``` 就行了。

## 联网
我有线连的路由器，ping 一下 www.baidu.com ，没通就 ```dhcpcd```一下来自动获取 ip ，稍等一会，再 ping 就通了。

## 格式化分区
lsblk 看一下，然后格式化之前的 ubuntu 分区，再挂载，最后建立挂载表。

    sda      8:0    0 238.5G  0 disk 
    ├─sda1   8:1    0   384M  0 part /boot/efi
    ├─sda2   8:2    0   128M  0 part 
    ├─sda3   8:3    0    99G  0 part 
    ├─sda4   8:4    0  88.5G  0 part 
    ├─sda5   8:5    0  49.5G  0 part /
    └─sda6   8:6    0   512M  0 part [SWAP]

/dev/sda1 是 efi , /dev/sda5 是根分区，/dev/sda6 是 swap 分区。

    mkfs.ext4 /dev/sda5
    mkswap /dev/sda6
    swapon /dev/sda6

## 挂载分区
我的home和boot就都放在根分区下了。  
顺便把efi挂上去。

    mount /dev/sda5 /mnt
    mkdir /mnt/home
    mkdir /mnt/boot
    mkdir /mnt/boot/efi
    mount /dev/sda1 /mnt/boot/efi

## 编辑源列表
用 vi 把 /etc/pacman.d/mirrorlist 里面的 163 的源放到第一个就行了。

## 安装基本系统

    pacstrap -i /mnt base base-devel

## 生成 fstab

    genfstab -U -p /mnt >> /mnt/etc/fstab

完了检查下 fstab ，看看是不是对的。

## Chroot 到新系统，并配置系统

    arch-chroot /mnt /bin/bash

## locale

编辑下 /etc/locale.gen 注释掉几个 utf-8 的项目。

    en_US.UTF-8 UTF-8
    zh_CN.UTF-8 UTF-8
    zh_TW.UTF-8 UTF-8

然后 ```locale-gen```  
设置 locale：

    echo LANG=en_US.UTF-8 > /etc/locale.conf

## 字体和键盘映射跳过，除非是非英语
## 时区
这个 archlinux 是建议用 utc 时间的，不过 windows 是用的本地时间，  
然后 osx 默认也是用的 utc 时间，之前我 osx 和 windows 是时间差是用的 osx 下面的一个开机修改时间的小脚本弄的，现在想了想就还是把 windows 修改成 utc 时间，再删掉 osx 那个小脚本好了。  

* 修改 windows 为 utc 时间。*
打开 regedit ，在```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation```项下面，新建一个名为 ```RealTimeIsUniversal``` 的 dword32 值 ，并赋值为 1 ，然后重启，再联网同步一下时间。
当然不用现在就做，装完或者装之前再弄都行。

回到 linux
用 utc+8 的时区，比如上海。

    ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

硬件时间，默认就是 utc ，可不用设置。  
就算时间错了，设置好 windows utc 之后，windows 下同步一下，再进 linux 时间就对了。

    hwclock --systohc --utc

## hostname 主机名

    echo hostname > /etc/hostname

并且在 /etc/hosts 添加一样的主机名（这个好像不用做，不过我还是做了。

    #<ip-address>   <hostname.domain.org>   <hostname>
    127.0.0.1       localhost.localdomain   localhost   *myhostname*
    ::1             localhost.localdomain   localhost   *myhostname*

## 网络
这个倒不用设置，要网的时候临时 dhcpcd 好了。

## 创建初始 ramdisk 
似乎这个自动做了，不用手动来一遍。

    mkinitcpio -p linux

## 设置 root 密码

    passwd

输入密码是没显示的，不是你键盘坏了。

## 安装 grub 
由于是 uefi 启动，参数略有不一样。  
/boot/efi 是之前挂载好了的efi分区 。

    pacman -S grub efibootmgr
    grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub --recheck

然后生成 grub 配置文件就可以重启了。

    grub-mkconfig -o /boot/grub/grub.cfg

## 卸载分区，重启系统
重启的时候分区会自动卸载，直接重启就行了。。。

    exit
    reboot

## 欢迎来到 archlinux
刚装完，啥都没有，不过这种很干净的感觉挺好的。。。

