---
layout: post
title: "archlinux-2015.02.01 配置"
date: 2015-02-17 14:40:06 +0800
comments: true
categories: archlinux
---

前面已经装好了 archlinux，一个很基本的系统，  
接下来装个图形界面，和一些基本的软件什么的。  

窗口管理器选的 gdm ，然后图形界面选的 gnome3  

## 安装 zsh
至于为嘛，用了就离不开了~  
虽然有些小蛋疼的地方。。

    pacman -S zsh

## 新建一个用户
参考的 wiki ，直接照打的  
现在想想如果没添加这些权限，现在可能都在解决问题了，这文章肯定要过一两天才看得到了。

    useradd -m -g users -G audio,video,floppy,network,rfkill,scanner,storage,optical,power,wheel,uucp -s /usr/bin/zsh 你的用户名

设置密码

    passwd 你的用户名

## sudo 的设置
默认似乎没法用，  
需要 visudo 编辑一下 sudo 的配置。

    visudo

进去后去掉wheel那一行前面的注释符号 # 即可。

    %wheel ALL=(ALL) ALL

## 安装 xorg 和 gdm 和 gnome 和 nvidia 驱动
输入法选的 fcitx  
顺便安装了一个 xterm 测试 xorg 用  
alsa-utils 安装是因为 gnome 要这个貌似。

    pacman -S xorg-server xorg-xinit wqy-zenhei wqy-microhei wqy-microhei-lite fcitx xterm alsa-utils unrar unzip
    pacman -S fcitx-configtool

xorg 装完可以启动下试试，看看能不能正常使用，  
退出的话，用鼠标叉掉窗口，就退出了。

    startx

我显卡是 nvidia 安装驱动就用 nvidia 驱动吧，之前 ubuntu 下面用着挺好的。

    pacman -S nvidia

会提示 libgl 冲突，选择 nvidia 的就行，mesa-libgl 是开源版本的。  
安装 gnome3 ，archlinux 默认就是 3 了，直接安装就是 gnome3 了。  

安装这个包默认 gdm 貌似也装了，就不用再安装一遍了。

    pacman -S gnome

## 安装 vim
注意要安装 gvim ，这个相比 vim 多了 clipboard 等编译参数，  
有的指令比如 "+y 这种与系统剪切板交互的指令只有 gvim 才有。  
启动不过还是一样的是 ```vim```

    pacman -S gvim

## 配置 vi 指向 vim
archlinux 自带一个 vi ，但真的非常不好用，  
~~虽然在 ```.zshrc``` 里加入 ```alias vi=vim``` 之后，当前用户可以了，~~  
~~但是 sudo vi 之后，又变成了那个阉割版的 vi ，~~  
~~简单粗暴的，直接链接下 vim 到 vi 了，~~

~~ln -s /usr/bin/vim /usr/local/bin/vi~~

卸掉：

    pacman -R vi

加入以下 alias

    alias vi=vim
    alias sudo='sudo '
    alias 'sudo vi'='sudo vim'


## 安装 fcitx 的一些组件  
拼音输入法我选的是 rime ，在 osx 下用了很久了，就这个了。

    pacman -S fcitx-gtk2 fcitx-gtk3 fcitx-rime fcitx-configtool fcitx-qt4 fcitx-qt5

## 安装 gnome 的网络组件  
这个装了之后，在 gnome 右上角才能设置。  
wireless_tools 是 wifi 必须的。

    pacman -S gnome-nettool wireless_tools

装完要开启 networkmanager 服务
顺便启动一下

    systemctl enable NetworkManager
    systemctl start NetworkManager

## 装个 firefox 先用用，后面再安装 chrome
我是 chrome 控~
当然还有 flash plugin

    pacman -S firefox flashplugin

## 启动 gdm
感觉可以启动 gdm 试试了

    systemctl enable gdm
    systemctl start gdm

*啊，登录界面来了~*
n卡还是方便，没碰到啥问题。


*可以用新建的账户登录了*
剩下的配置就用 gnome-terminal 来弄了~

当然登录之后就不是 root 账户了，搞个啥都要 sudo 
我恨 sudo

## 触摸板驱动
大部分都是 synaptics 的触摸板，装了就能用触摸板了。

    sudo pacman -S xf86-input-synaptics

## 配置 fcitx

    vi ~/.xprofile

加入三行：

    export XMODIFIERS=@im=fcitx
    export QT_IM_MODULE=fcitx
    export GTK_IM_MODULE=fcitx

要注销下才能生效。

fcitx 自带一个 ```fcitx-diagnose```  
configure ui 和 xim 报的错误可以不管，其他都修正了，一般 fcitx 就 ok 了。

## 配置 oh-my-zsh
真是个好东西

    curl -L http://install.ohmyz.sh | sh

搞定

## 设置 gnome3
装个 GNOME tweak tool 先  

    sudo pacman -S gnome-tweak-tool

进入设置里，Tweak Tools  
Appearance 里，打开 Global Dark Theme，黑色才高大上嘛。  
Desktop 里，打开 Icons on Desktop，桌面才能放图标。

## 一些小工具
iftop 查看流量的  
aria2 命令行下载工具  
xorg-xinput xinput 工具，编辑鼠标加速用  
gedit gnome3 的文本编辑器  

dosfstools 有了这个才有 mkfs.fat  
ntfsprogs 有了这个才有 mkfs.ntfs  

    sudo pacman -S iftop aria2 xorg-xinput gedit
    sudo pacman -S dosfstools
    sudo pacman -S ntfsprogs

差不多就这些了。  


