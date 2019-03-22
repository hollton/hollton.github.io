title: whistle调试移动端
date: 2018-12-13 14:03:46
tags:
categories:
---

# whistle简介
[whistle](http://wproxy.org/whistle/)，可用于查看、修改HTTP，以及代理拦截，PC联调移动端。

## 安装启动

    $ npm install -g whistle
    $ w2 start

## 调试工作

### PC端

* 访问 http://localhost:8900/#rules 并添加拦截规则
    如：fep-web.beta.web.sdp.101.com weinre://fep，表拦截“fep-web.beta.web.sdp.101.com”域名并为其设置fep分类（分类可任意指定）
* 点击工具栏 Weinre 下对应的分类，可进入拦截监听页面
    如：http://localhost:8900/weinre/client/#fep

### 移动端

* 配置 WI-FI 代理，服务器为 PC 的 ip ,端口为 8899

### 调试页面
* 若刷新监听页面，Targets 显示为none，需要将以下代码嵌入到调试页面。
    `<script src="http://localhost:8080/target/target-script-min.js#anonymous"></script>`
