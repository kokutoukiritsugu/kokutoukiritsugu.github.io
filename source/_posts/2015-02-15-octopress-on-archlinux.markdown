---
layout: post
title: "archlinux 上搭建 octopress 环境 和 升级到 octopress 3.0"
date: 2015-02-15 20:40:11 +0800
comments: true
categories: octopress
---

之前在 ubuntu 上用的 octopress ，后来好长时间没使用了，现在新装了 arch，  
于是记录下 octopress 在另一台电脑上的安装。  

环境是 archlinux + zsh + oh-my-zsh + ruby 2.2.0p0

我习惯把东西放在一个文件夹里面  
然后把之前的 octopress 给 clone 下来。  
由于 octopress 有两个分支，source分支放的是octopress的源码，还有你自己写的markdown文件。master分支则是放的生成的html页面。所以新电脑上搭建，要两个分支都pull下来。  

首先clone下。

    cd 
    mkdir octopress
    cd octopress
    git clone 你的octopress的github地址

然后切换到source分支，并且clone master分支到\_deploy文件夹下面

    git checkout source
    mkdir _deploy
    cd _deploy
    git init
    git remote add origin 你的octopress地址
    git pull origin master
    cd ..

这样就行了。

## 搭建环境

安装 rbenv

    cd ~
    git clone git://github.com/sstephenson/rbenv.git .rbenv

推荐安装的插件：
octopress 3 支持 ruby2.2 的话，下面这两行可以不执行了。

    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build # 用来编译安裝 Ruby
    git clone git://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash # 通过 gem 命令安裝完 gem 后无需手动执行 rbenv rehash 命令

编辑下 .zshrc 或者 .bash_profile 加入这两行环境变量

    export PATH="~/.rbenv/bin:$PATH"
    eval "$(rbenv init -)"

这两行最好放在 source oh-my-zsh 这行的上面，以免出现问题。  
要立即生效可以直接

    source ~/.zshrc

octopress 3.0 可以直接用 ruby2.2  
就不用安装 ruby 1.9.3 了  
直接安装 bundler 和一堆 bundler 组件。

    gem install bundler
    cd ~/octopress/你的octopress
    bundle install

如果最后一句报错：

    Can't 'bundle install' outside a bundled project

那就是 gem 的环境变量没加进去。  
在 .zshrc 里加入下面一句：

    export PATH="/home/kokutou/.gem/ruby/你的ruby版本/bin:$PATH"

其实不带参数运行下 gem ，如果没加环境变量，gem会提示的。  

## 升级 octopress 3.0  
首先切换到source分支

    git checkout source

然后添加 octopress 远程地址

    git remote add octopress https://github.com/imathis/octopress.git
    git pull octopress master

这里可能会提示代码合并冲突，手动解决冲突之后就可以push一下了。

    git add.
    git commit -m "update octopress 3.0"
    gut push origin source

然后升级 bundle 和 source 和 style  
当然你用了其他的主题可以不升级style

    bundle install
    rake update_source
    rake update_style

然后发布一下：

    rake gen_deploy

以上 octopress 的环境就搭建完成了。  
可以开始使用了。



