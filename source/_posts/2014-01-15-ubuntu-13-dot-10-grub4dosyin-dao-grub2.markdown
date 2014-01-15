---
layout: post
title: "ubuntu 13.10 grub4dos引导grub2"
date: 2014-01-15 11:10:46 +0800
comments: true
categories: 
---

我是用的gurb4dos管理机器上所有操作系统的。  
3个系统，win7 mac osx，ubuntu，还有杂七杂八的pe之类的。  

那个时候grub4dos引导grub2的菜单是这样的：  
    title ubuntu
    root (hd0,2) #这个根据你grub2所在分而定
    kernel /boot/grub/core.img
    boot
刚装上ubuntu 13.10的时候，正常的，某天重启之后，啊咧，突然进不去了。。。  
这时候我用livecd进去看看/boot/grub下面发现core.img不在这里的，然后多了个i386-pc很可疑。  
ls i386-pc看了下，发现居然core.img到这来了。  
估计是gurb2更新了换了位置。  

于是把上面里面的目录修改下就可以了。  
    kernel /boot/grub/i386-pc/core.img
搞定。  

