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
