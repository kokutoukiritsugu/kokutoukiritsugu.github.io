---
layout: post
title: "archlinux下，普通用户adb无法连接设备"
date: 2015-02-16 11:21:07 +0800
comments: true
categories: android
---
archlinux 下面，装的 google 官网的 Android Studio  
想用真机调试，点了软件里的运行之后，发现设备显示的确是问号：

    ????????????    no permissions

于是估计应该是没权限，进入到 Sdk 的目录下面  
```sudo adb devices``` 发现可以正常连接设备。

搜了下 wiki ，普通用户使用看起来要写 udev 脚本，  
于是上 yaourt 搜了下，发现了个 android-udev 的包，直接安装下试试：

    yaourt -S android-udev

试试普通用户运行```adb devices```，啊，正常了。  

再试试 Android Studio ，普通用户能调试了。  

找了找，发现 android-udev 这个包是安装了 /lib/udev/rules.d/51-android.rules 这个 udev 规则。感兴趣的可以看看。


