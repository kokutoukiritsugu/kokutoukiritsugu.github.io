---
layout: post
title: "octopress 迁移到 hexo 后，disqus 评论的转移"
date: 2015-03-31 17:21:23
comments: true
categories: hexo
tags:
---
迁移过来后 disqus 的评论消失了。  
这是因为 hexo 和 octopress 的地址略有不一样。  
  
比如原来 octopress 是：
```raw
http://kokutoukiritsugu.github.io/blog/2015/03/30/octopress-to-hexo/
```
现在 hexo 是：
```raw
http://kokutoukiritsugu.github.io/2015/03/30/octopress-to-hexo/
```
hexo 相比 octopress 的 url 少了一个 /blog  
  
disqus 有个 [url mapper](https://disqus.com/admin/discussions/migrate/)  
说明在此： [url mapper help](https://help.disqus.com/customer/portal/articles/912757-url-mapper)  
  
当然它还有别的工具，我试了下这个比较顺手。。。  
下下来 csv 文件，然后用 excel 打开，复制左边一列到右边  
然后选中右边一列，只对右边一列做 查找与替换，删除掉所有的 blog/ ，再保存为 csv 文件上传就好了。  
  