---
title: git配置与使用
tags: 
    - git
    - config
    - github
categories: 
    - git
thumbnail: /images/git-config/thumbnail.jpg
excerpt: git简单配置与使用
---

## 下载并安装git

前往[Git官网](https://git-scm.com)点击下载进入下载页面，根据自己的操作系统下载安装即可。

**使用如下command查看是否成功安装git:**

```shell
git --version[-v]
```

## 配置git

{% note info %}
提示: 以下假设在设备上成功下载安装git,并配置好了相关环境变量。
{% endnote %}

```shell
#配置用户和邮箱
git config --global user.name '<username>'
git config --global user.email '<useremail>'
### 
```
至此基本就配置完成，其余的问题可根据实际问题进行特定配置，具体查阅[git-config-doc](https://git-scm.com/docs/git-config)

## 配置github

github主要使用2种方式进行仓库链接本地:

> https: 需要github账号密码验证,手动验证,不常用
> ssh: ssh验证,自动验证，常用

{% note info %}
这里只介绍ssh配置,https配置可自行查阅github-doc
{% endnote %}

### 大致流程

> 注册并登陆github
> 找到设置中关于ssh验证的栏目
> 添加ssh公钥
> 完成ssh配置

### 具体细节

{% note warning %}
配置时所用的OS为linux,如为其它OS,查阅相关教程与文档。
{% endnote %}

```shell
#开启sshd
sudo systemctl start sshd.service
#开启ssh-agent
eval "$(ssh-agent -s)"
#生成密钥和公钥
ssh -t [algorithm] -b [key-length] -C "<user-email>" -f [output-file-name]
eg: ssh -t rsa -b 4096 -C ''
#进入~/.ssh查看公钥与密钥，id_[algorithm]为密钥 id_[algorithm].pub为公钥
cd ~/.ssh
ls -al
#添加密钥到ssh-agent
ssh-add id_[algorithm]
#添加公钥到github,复制下面的输出内容到github的添加ssh-key内
cd ~/.ssh
car ./id_[algorithm].pub
#访问github连接
ssh -T git@github.com
```

## 常用Command

{% note green fa-circle-up %}
此部分持续更新中...
{% endnote %}

```shell
# 添加代码
git add <file-name>
# 添加状态
git status
# 提交到本地存储库
git commit -m 'commit-msg'
# 推送代码到远程存储库
git push local-branch remote-branch
# 拉取代阿
git pull local-branch remote-branch
# 添加远程仓库
git remote add local-branch ssh-link
# 查看本地分支
git branch
# 查看本地远程仓库
git remote -v
# 
```

**关于命令的详细使用:**

```shell
git <command> --help
```

## 总结

git是一个版本控制工具，主要用于针对我们的工程代码进行管理。很多用法和教程在git官方网站都有给出。

**此文章主要用于记录git的一些主要使用，以便将来温习查阅。**

## 相关

> [git官网](https://git-scm.com/)
> [github官网](https://github.com)
> [git-doc](https://git-scm.com/docs)
> [github-doc](https://docs.github.com/en)


