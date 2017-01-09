title: "利用hexo搭建github博客总结"
date: 2015-01-04 22:09:43
tags: [hexo]
---
##hexo搭建github博客
有用github写博文的想法已经已久了，但时至今日才理出个时间把它搞定。其中要多谢[zippera](http://zipperary.com)的博文的极大帮助，原文出处：[hexo系列教程](http://zipperary.com/categories/hexo/)。你也可以参考[hexo.io](http://hexo.io/)以及其[中文站点](http://hexo.io/zh-cn/)。本文主要是对自己今天的搭建过程做一个回顾与总结，还请大神指教，若有新手也想尝试github博客，看看下面的介绍也是个不错的选择呢。
###hexo是什么
hexo是一个基于Node.js的静态博客程序，可以方便的生成静态网页托管在github以及Heroku上。而且平时的使用过程中仅需要<code>hexo new</code> <code>hexo generate</code> <code>hexo server</code> <code>hexo deploy</code>四个命令即可完成操作。
<!--more-->
###安装Git与Node.js
首先我们需要安装[git-for-windows](https://git-for-windows.github.io/ "git-for-windows")与[node.js](http://nodejs.org/ "node.js")。直接默认安装设置就可以了。
###安装hexo
接着我们安装hexo,打开Git bash或者在电脑任意位置右击，选择Git bash，输入以下命令：
$ <code>npm install -g hexo-cli</code>
然后在你的电脑盘里新建一个文件夹（如E:\hexo）,在此文件夹下依旧右键，选择Git bash,输入命令进行初始化：
$ <code>hexo init</code>
与安装依赖包：
$ <code>npm install</code></p>
或者你也可以依次使用以下命令执行：
$ <code>hexo init e/hexo</code>（：初始化生成该文件夹）
$ <code>cd e/hexo</code>（：进入该文件夹）
$ <code>npm install</code>（：安装hexo）
注：npm install安装失败多是因为网络不畅，耐心多试几次就好。
###本地查看
接下来我们不忙着配置站点，依旧在Git Bash上输入命令保存：
$ <code>hexo generate</code>
启动本地服务运行：
$ <code>hexo server</code>
然后浏览器输入localhost:4000就可以查看到本地博客，默认会有个“hello world”页面。
注：hexo已有更简洁的命令啦，即输入:
<code>hexo g</code>===<code>hexo generate</code>
<code>hexo s</code>===<code>hexo server</code>
<code>hexo n</code>===<code>hexo new</code>
<code>hexo d</code>===<code>hexo deploy</code>
后两个命令的作用我会在后面讲述。
###配置站点
我们可以在<code>/_config.yml</code>中配置站点。以下是我私以为要修改的地方：
title: 网站logo标题
subtitle: 副标题
description: 描述
author: 作者，也就是你
language: 语言（中文为zh-CN）

url: 网址（如我的http://hollton.github.io/
root: 根目录，默认为/，但如果你的比如为http: //hollton.github.io/hexo/，根目录则为/hexo/。

theme: 主题
不喜欢默认主题？有办法，我们可以在[themes](http://hexo.io/themes/)中下载中意的主题。每个主题页面会有预览界面，以及About中有具体的下载地址以及个性化配置。
###使用github库
一般我们使用github存放代码，如果你还没有github帐号，可以在[github](https://github.com/)注册，过程很简单，也就是用户名、邮箱之类的，这里就不再赘述了。接着在右上角选择New repository创建。
![](/img/repo.png)
创建时应该和你的github帐号对应，比如我的github帐号是hollton，因此博客名应该为<code>hollton.github.io</code>。
![](/img/create.png)


接下来我们就需要安装hexo-deployer-git
$ <code>npm install hexo-deployer-git --save</code>
并在<code>/_config.yml</code>中修改配置文件。
deploy:
  type: git
  repo: <库的地址>（如我的：https://github.com/hollton/hollton.github.io.git
注：在修改配置文件时需要使用空格而不是tab缩进，而且每个冒号(:)和后面配置信息需有一个空格。这点很重要，否则会出问题。
###传至github
接着我们就可以传到自己的github上查看博客啦~
输入命令$ <code>hexo deploy</code>
等待出现<code>INFO  Deploy done: git</code>信息时我们就可以访问自己的博客查看效果啦，出现FATAL错误一般是网络问题，多试几次就可以了。