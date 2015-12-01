---
layout: post
title: "markdown代码语法和vi的替换"
date: 2014-01-10 15:52:20 +0800
comments: true
categories: markdown
---
markdown 的代码语法就是简单的前面加四个空格。  
就会像下面这样显示了：

    markdown前面四个空格。

vi的替换如下：

    :%s/someword/someword/g

说明：  
%的意思是全部替换，否则只替换一行。  
g的意思是整行替换，否则只替换开头。  
someword可以用\t表示tab符号。  
