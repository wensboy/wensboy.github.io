---
title: 关于搭建个人博客
tags: 
    - hexo
    - vercel
    - github
categories:
    - blog
sticky: 999
thumbnail: /images/create-blog/thumbnail.jpg
excerpt: "hexo+vercel+github简单搭建个人博客"
---

{% note info  %}
提示:这不是一篇详细教程，仅提供搭建思路。
{% endnote %}

## 前言

最近几天一直在捣鼓搭建个人博客。起初的搭建的理由是针对于linux无法科学上网导致很多东西无法查阅资料，遇到问题需要频繁从网上获取解决方案。
于是乎便想着搭建个人博客，在记录一些解决方案的同时，将有用的、重要的信息记录下来。

## 搭建过程

### idea

首先，搭建个人博客，需要思考一个问题： 
**我们需要什么？或者说：我们需要准备什么？**

简单来想，需要：

> 一个网站：充当博客网站
> 一个仓库：存放网站的文章等资源
> 一个编辑器：写博客、文章

现在想复杂一点，需要：

> 一个标识：指向博客网站
> 一种通讯方式：编辑器与仓库，仓库与网站

现在我们需要专业一点了，需要：

> 静态网站：博客网站
> 域名：www.xxx.xxx指向静态网站
> vercel：部署静态网站
> github：代码仓库
> vscode：编辑器
> hexo：博客框架，快速搭建同时美化

{% note red fa-question %}
比对一下，发现了什么不同吗？
{% endnote %}

我们缺少了通讯方式，但其实并没有，至少在仓库与网站方面，vercel帮我们实现了持续部署，这是根据github上对应的仓库变换来完成的。现在，我们只需要考虑**编辑器**与**仓库**之间如何交流了，这很有意思，因为如果经常使用github，那你对git一定不陌生。是的，git仿佛总是必须的。随着我们的博客越来越大，存放在自己电脑上仿佛总是不安全的。

### realization idea

> 注册github,创建名为username.github.io的仓库
> vercel关联github账号注册，创建一个新hexo部署工程关联上述仓库
> 本地安装vscode和hexo，新建博客文件夹
> 依次初始化hexo和git仓库在博客文件夹
> 使用git推送本地至github
> vercel管理自己的域名
> 使用域名访问博客网站

{% notel blue 细节提示 %}
1、上述步骤可不按顺序进行
2、由于vercle域名污染原因，绑定自己的域名是必要的
3、可选用其他部署工具替代vercel，同理，github也可用其他仓库代替
{% endnotel %}

## 总结

在我看来，搭建个人博客总不是必须的，除非你有足够的时间和精力来多了解很多必要的知识。或者你认为这能帮你解决很多问题，当然，开心最好。