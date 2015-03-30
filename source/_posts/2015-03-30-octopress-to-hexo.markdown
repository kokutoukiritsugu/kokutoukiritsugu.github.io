---
layout: post
title: "迁移 octopress 到 hexo"
date: 2015-03-30 14:54:40
comments: true
categories: hexo
tags:
---
最近在 windows 下折腾 cygwin 和 msys2 ，windows 下想用 linux 工具集真是不方便。  
然后突然想起 octopress 了，这货要装 ruby 等等一堆东西，windows 下折腾，想想就醉了。  
google 了下，打算切换到 hexo。  
基于 node.js 的静态博客。而且还有生成速度很快的优点。  
  
主要下载了 msysgit ， io.js （兼容 node.js ，某些情况速度更快，顺便可以试试 io.js）。  
安装的时候 msysgit 提示环境变量的时候，选择的第二个，只把 git 添加到环境变量。  
这样就可以到处使用了。  
先是把原来的给 clone 下来。 顺便 checkout 出 source 分支。

    git clone 博客github地址 博客文件夹
    cd 博客文件夹
    git checkout -t origin/source

重命名 source 分支 为 octopress 来备份以前的博客源文件。  
github 重命名远程分支是先删除远程分支，再重命名本地分支，然后再推送过去成为一个新的分支。  

    git branch -m source octopress
    git push origin --delete source
    git push origin octopress
    git branch --set-upstream-to origin/octopress

现在可以新建一个  hexo 分支了。  

    git branch hexo
    git checkout hexo

原来的文章都在 source/_posts 下面。  
因为我在 windows 下面，我就打开文件夹手动删掉了除 source/_posts 和 .git 这两个文件夹之外的其他文件。
然后推送到 github 上面。

    git add --all .
    git push --set-upstream origin hexo
    git push

开始安装 hexo

    npm install hexo-cli -g
    npm install hexo --save
    hexo init .
    npm install
    npm install hexo-deployer-git --save
    
这就安装完成了。  
接下来是主题的安装，还有设置。  
选了 light 主题，这个最简约，官网还有其他的可选。  

    git submodule add git://github.com/tommy351/hexo-theme-light.git themes/light

这个主题默认启用了 facebook 。  
原来我用的是 disqus 的评论，所以这个主题要编辑一下设置。  
在 theme/light/_config.yml 里面：

    comment_provider: facebook 

修改为  

    comment_provider:

配置 hexo
主要需要修改的就以下几项：

    title: 博客标题
    subtitle: 子标题
    author: 名称
    
    url: 博客url
    
    new_post_name: :year-:month-:day-:title.markdown
    
    theme: light

启用 disqus 评论需要加一行

    disqus_shortname: kokutoukiritsugu

以上这些都可以照着 octopress 原来的复制过来。

然后就是最后面的 deploy 部分，修改为 github 模式。

    deploy:
      type: git
      repo: 你的git地址
      branch: master
      message: Site updated { { now("YYYY-MM-DD HH:mm:ss") } }

上面这一行 大括号与大括号之间我加了个空格，否则 hexo 会生成失败。
然后可以预览试试了。

    hexo g
    hexo s

修改一下默认的新文章格式。
scaffolds/post.md

修改为和 octopress 一样的。

    ---
    layout: post
    title: {{ title }}
    date: {{ date }}
    comments: true
    categories: 
    tags:
    ---

这样 hexo n "title" 的时候生成的东西就和原来 octopress 时一样了。