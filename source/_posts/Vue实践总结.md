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

<!-- more -->

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

# 动态创建slot（未果）
尝试：模版是动态返回的，尝试在模版中插入具名slot，调用相应的组件，但无法实现。
总结：slot需在模版中预先定义

# 隔代使用slot（未果）
需求：最初父组件调用组件并使用具名slot，子组件插入slot，但发现多个子组件中写了一些重复类似的代码（其中包含slot部分），因此想把这部分代码抽出来。
尝试：尝试在父组件使用具名slot组件，子组件中调用孙子组件，孙子组件中插入具名slot，但无法实现渲染。

# nextTick
需求：服务端返回音视频数据更新dom后再用插件渲染
尝试：mounted之后执行渲染，最初是可行的，但后面测试数据量增多时发现有时部分渲染失败
总结：有对dom操作的需求时，应在mounted之后调用nextTick，确保dom更新后再执行操作

# 对数组元素新增属性 $set
需求：遍历数组并对每个元素新增hidden属性
尝试：常规使用array.forEach，直接写item = true/false，在渲染dom时无效
解决：官网也有提到，由于Vue不能检测属性被添加，如果想要确保添加的元素是响应式的，需要通过Vue.set(object,key,value)设置，会触发视图更新。