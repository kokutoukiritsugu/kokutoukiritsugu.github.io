---
layout: post
title: "使用lowlatency内核"
date: 2014-01-10 23:50:58 +0800
comments: true
categories: 
---
用ubuntu13.10的时候，在鼠标点击，或者键盘切换的时候总有点迟滞感。  
这个是因为ubuntu默认的generic内核是非实时内核。  

开始想装实时内核来着，后来在

    sudo apt-get install linux-image-[tab]

的时候发现有 linux-image-3.11.10-15-lowlatency 内核。  
google发现这个低延迟内核是可以大幅降低操作延时的。  
具体几个内核的区别见：[UbuntuStudio/RealTimeKernel](https://help.ubuntu.com/community/UbuntuStudio/RealTimeKernel)  
里面有内核介绍和内核的选择说明。  

安装lowlatency内核：  
    sudo apt-get install linux-image-[tab]
选你当前内核版本的lowlatency内核  
    sudo apt-get install linux-image-x.xx.x-xx-lowlatency
安装完后发现提示没有内核的headers，把headers也装上  
    sudo apt-get install linux-headers-[tab]
一样选择对应版本的headers  
    sudo apt-get install linux-headers-x.xx.x-xx-lowlatency

重启后进入grub2选择页的时候选下面的**高级启动选项**  

进去后选新装上的 -lowlatency 内核启动即可。  

lowlatency内核关chrome页面的时候再也没有迟滞感了，Alt+Tab秒切有木有！  

