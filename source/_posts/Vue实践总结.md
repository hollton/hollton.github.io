title: Vue实践总结
date: 2017-05-12 19:53:27
tags: [vue]

---
## JS中使用filters
	参考：https://github.com/vuejs/Discussion/issues/405
	Vue.filter('indexToLetterFilter', index => {
		return String.fromCharCode(65+index);
	});
	this.$options.filters.indexToLetterFilter(index);

	<!-- html中传参+串联 -->
	{{index | indexToLetterFilter('arg1', arg2) | anotherFilter}}

## data、computed、props优先级
data > computed > props

## 图片资源引用
方案1：把资源放在static文件夹下再引用；

方案2：使用require引入；

	<img :src="logo">

	data:{
		logo: require('../asset/logo.png')
	}