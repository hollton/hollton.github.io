title: "vue-cli构建工具"
date: 2016-12-19 10:41:40
tags: [vue-cli]
categories: [前端]

---
## 准备
安装node.js、git
## shell 命令
`$ npm install -g cnpm --registry=https://registry.npm.taobao.org`(使用taobao NPM镜像，可用cnpm代替npm)

`cd E:/WWW`

`npm install -g vue-cli`(全局安装vue-cli)

`vue init webpack#1.0 vue-project`(可选安装1.0)

`cd vue-project`

`npm install`(安装依赖)

`npm run dev`(启动)
## 官方模版
`vue init template-name project-name`

[browserify](https://github.com/vuejs-templates/browserify)--全功能的Browserify + vueify，包括热加载，静态检测，单元测试

[browserify-simple](https://github.com/vuejs-templates/browserify-simple)--一个简易的Browserify + vueify，以便于快速开始

[webpack](https://github.com/vuejs-templates/webpack)--全功能的Webpack + vueify，包括热加载，静态检测，单元测试

[webpack-simple](https://github.com/vuejs-templates/webpack-simple)--一个简易的Webpack + vueify，以便于快速开始