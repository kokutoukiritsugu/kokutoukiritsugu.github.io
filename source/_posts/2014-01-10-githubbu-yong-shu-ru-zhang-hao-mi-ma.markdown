---
layout: post
title: "git push不用输入帐号密码"
date: 2014-01-10 15:42:30 +0800
comments: true
categories: 
---
首先看看有没有sshkey：

    cd ~/.ssh

有的话，就要备份或者删除。  
添加一个key：

    ssh-keygen -t rsa -C "邮箱@xx.com"

提示设置密码的时候直接回车，不然push的时候还是要输入密码。  
成功后复制.ssh/id_rsa.pub到github的账户设置里的sshkey里面就可以了。


** 2015-02-15 更新 **  

用了goagent代理之后，用ssh感觉很慢。。。  
https的有办法临时储存密码。  
方法如下：

    git config --global credential.helper 'cache --timeout=3600'
    # Set the cache to timeout after 1 hour (setting is in seconds)

--timeout=3600 不带这个参数的时候默认为15分钟。  
git官方说明：[点我](https://help.github.com/articles/caching-your-github-password-in-git/)  
