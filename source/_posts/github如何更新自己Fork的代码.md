title: "github如何更新自己Fork的代码"
date: 2015-05-11 15:27:36
tags: [github,fork]
categories: [前端]
---
github有fork功能，可以将别人的项目复制到自己账号下，但其有一个缺点就是当别人的源项目更新后，你fork的分支并不会一起更新，需要我们手动去更新。

<!-- more -->

下面就以我的帐号：hollton，以及fork的项目：[zoom.js](https://github.com/fat/zoom.js.git)为例为大家讲述如何同步更新自己的代码。
##### 1、clone自己账号里fork的分支
	$ git clone https://github.com/hollton/zoom.js.git
	$ cd zoom.js
##### 2、增加远程原始分支
	$ git remote add fat.zoom.js https://github.com/fat/zoom.js.git
其中fat.zoom.js可随意写
###### 接着可以查看远程分支列表
	$ git remote -v
![](/img/fork.png)
##### 3、fetch源分支的新版本到本地仓库
	$ git fetch fat.zoom.js
##### 4、合并源新版本与自己版本代码
	git merge fat.zoom.js/master
##### 5、把合并后的代码push到自己的github上
	$ git push origin master