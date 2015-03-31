---
layout: post
title: "git 在多账户的情况下使用 SSH 免密码操作"
date: 2015-03-31 17:19:35
comments: true
categories: git
tags:
---
当然先去掉我以前设置的全局 email 和 name

    git config --global --unset user.name
    git config --global --unset user.email

现在每个 repo 要设置自己的 email 和 name

    git config user.name "你的name"
    git config user.email "你的email"

生成两个邮箱的 ssh key  
生成的时候提示输入文件名的时候选择两个不同的文件，  
提示输入密码就直接为空，目标是操作的时候免密码嘛。

    ssh-keygen -t rsa -C "github_email1"
    ssh-keygen -t rsa -C "github_email2"

现在在 ~/.ssh 下面有了这几个文件：

    id_rsa (对应的 github_email1)
    id_rsa.pub (对应的 github_email1)
    id_rsa_second (对应的 github_email2)
    id_rsa_second.pub (对应的 github_email2)

现在要在 github 网页上，分别登录两个账号，  
复制两个账号对应的 .pub 文件内容到 github 网页上的 设置 ssh key  里面。  
  
新建一个 ~/.ssh/config 配置文件  
内容如下：

    host username1
        hostname github.com
        Port 22
        User git
        IdentityFile "id_rsa的路径"
    
    host username2
        hostname github.com
        Port 22
        User git
        IdentityFile "id_rsa_second的路径"

现在可以测试一下是否设置正确了。  

    ssh -T username1
    ssh -T username2

看到了 Hi 你的用户名! You've 这行话，就说明 ssh 配置对了。  
上面的 username1 和 username2 可以随便取，这两个是后面要用到的。  
  
这时候可以开始设置 repo 用 ssh 的方式来连接 github 了  
先看看原来的 remote 地址是什么

    git remote -v

得到

    origin https://github.com/你的账号/你的repo (fetch)
    origin https://github.com/你的账号/你的repo (push)

现在是多账户，这个 repo 用的是 username1 里面的 ssh key 那么 ssh 地址就应该是这样的  

    username1:你的账号/你的repo

这个 username1 就是上面的 ~/.ssh/config 里面设置好的两个 host 名  
git 设置的命令是：  

    git remote set-url origin username1:你的账号/你的repo

当然因为前面去掉了全局账号和邮箱，这里就也要设置这个 repo 的账号和邮箱。  
  
enjoy!