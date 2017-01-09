title: "vue-cli构建工具"
date: 2016-12-19 10:41:40
tags: vue

---
##准备
安装node.js、git
##shell 命令
<code>$ npm install -g cnpm --registry=https://registry.npm.taobao.org</code>(使用taobao NPM镜像，可用cnpm代替npm)

<code>cd E:/WWW</code>

<code>npm install -g vue-cli</code>(全局安装vue-cli)

<code>vue init webpack#1.0 vue-project</code>(可选安装1.0)

<code>cd vue-project</code>

<code>npm install</code>(安装依赖)

<code>npm run dev</code>(启动)
##官方模版
<code>vue init template-name project-name</code>

[browserify](https://github.com/vuejs-templates/browserify)--全功能的Browserify + vueify，包括热加载，静态检测，单元测试

[browserify-simple](https://github.com/vuejs-templates/browserify-simple)--一个简易的Browserify + vueify，以便于快速开始

[webpack](https://github.com/vuejs-templates/webpack)--全功能的Webpack + vueify，包括热加载，静态检测，单元测试

[webpack-simple](https://github.com/vuejs-templates/webpack-simple)--一个简易的Webpack + vueify，以便于快速开始