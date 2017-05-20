title: Vue实践总结
date: 2017-05-12 19:53:27
tags: [vue]
categories: [前端]

---
# JS中使用filters
	参考：https://github.com/vuejs/Discussion/issues/405
	Vue.filter('indexToLetterFilter', index => {
		return String.fromCharCode(65+index);
	});
	this.$options.filters.indexToLetterFilter(index);

	<!-- html中传参+串联 -->
	{{index | indexToLetterFilter('arg1', arg2) | anotherFilter}}

# data、computed、props优先级
data > computed > props

# 图片资源引用
方案1：把资源放在static文件夹下再引用；

方案2：使用require引入；

	<img :src="logo">

	data:{
		logo: require('../asset/logo.png')
	}

# 动态模版添加方法
需求：服务端返回 `'<p><img src=".." /></p>'`html串，需要实现页面点击图片时弹出预览框。

尝试：最初的想法是通过对html串拼接click方法，即可通过点击事件调用弹窗组件并获取其src预览。结果做不到对拼接的html再编译以使点击事件生效。

解决：通过事件代理实现。对动态模版的父元素绑定方法`@click="imageProxy($event)"`

	imageProxyy(evt){
        if (evt.target.nodeName === 'IMG') {
            evt.stopPropagation();
            this.showPicView = true;
            //evt.target.currentSrc 后续处理
        }
    }