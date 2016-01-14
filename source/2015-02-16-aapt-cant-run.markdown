---
layout: post
title: "andorid studio 在 archlinux 上调试程序时报错"
date: 2015-02-16 07:32:20 +0800
comments: true
categories: android
---
用 google 官网的 android studio 安装完后，直接新建个空工程和avd，  
点运行会报错如下：

    Error: Cannot run program "/home/user_name/Android/Sdk/build-tools/21.1.2/aapt": error=2, No such file or directory

到对应目录下看，发现 aapt 这个程序是存在的。  
但是 ```./aapt``` 却不能运行。  
之前尝试```yaourt -S android-studio```的时候会安装一大堆  lib32  的库，  
于是怀疑是没 lib32 的库，  
当然你直接```yaourt -S android-studio```等它解决完依赖后就停下来也行。  
**就可以不用看下面的了**  
  
但我不想装那么多库，就只能排查了。  

搜到了```ldd```可以看程序，但发现这东西和我想的不太一样。  
然后找到了```readelf```，运行：

    readelf -l ./aapt

得到一大堆，看中间一行：

    ...
    Requesting program interpreter: /lib/ld-linux.so.2
    ...

应该就是这个库了，google 了一下发现是 lib32-glibc  
直接```pacman -S lib32-glibc```装上，  
接着```./aapt```会发现可以运行了，这是会直接提示说缺xxx.so.x  
直接照名称装上 lib32-zlib 和 lib32stdc++5 就ok了。  

这时候再运行```./aapt```就正常了。  
再打开 Android Studio 调试试试，能正常运行不报错了。  


