---
layout: post
title: "ubuntu 13.10 nvidia驱动装不上/装上后重启黑屏"
date: 2014-01-10 15:31:56 +0800
comments: true
categories: 
---
昨天装nvidia驱动的时候莫名奇妙突然装不上了，表现为重启后黑屏，然后再次重启后正常，导致我以为装好了，但是打开blender的时候，没有检测到CUDA，也就不能用显卡渲染。  
又去了官网下驱动安装，或者 sudo apt-get install nvidia-309 安装ubuntu源里的驱动都没法开启cuda，后来灵光一闪打开dota2发现帧率很低才正常的一半，才知道用的还是开源驱动。  
google了一下，先禁用开源驱动：

    sudo vi /etc/modprobe.d/nvidia-installer-disable-nouveau.conf

新建一个黑名单文件，输入如下：

    blacklist nouveau
    options nouveau modeset=0

然后 :wq 保存。  
我顺便卸载了nouveau驱动：

    sudo apt-get remove xserver-xorg-video-nouveau

然后重建initramfs：

    sudo update-initramfs -u

这时候如果你之前nvidia驱动装好了的话，直接重启就正常了。  
nvidia-settings 打开也能看到温度，频率和型号了。  

