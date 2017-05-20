title: "Unveil.js图片延时加载"
date: 2015-04-23 11:37:34
tags: [Unveil.js,Layzr.js]
categories: [前端]
---
## Unveil.js是一个jQuery图片延时加载插件且极轻量。支持IE7。
### 使用方法
	<img src="bg.png" data-src="original-img.jpg" data-src-retina="img-retina.jpg" />

	<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
	<script src="jquery.unveil.js"></script>
	<script>
		$(document).ready(function() {
			$("img").unveil();
		});
	</script>
首先src属性里需要一个占位图片。
data-src为要加载的原始图片，当页面滑动到其可视窗口时再显示。
如果想支持视网膜设备加载高分辨率图片，则需要使用data-src-retina，但这不是必需的。

### 如果担心用户没有启用javascript，则使用下面降级。
	<noscript>
		<img src="original-img.jpg.jpg" />
	</noscript>
### 使用参数可使图片距离视口100px就开始加载。
	$("img").unveil(100);
### 更好的是它支持回调函数，如：
	$("img").unveil(100,callback);
### 图片淡出加载例子
	img {
		opacity: 0;
		transition: opacity .3s ease-in;
	}

	$("img").unveil(100, function() {
		$(this).load(function() {
		this.style.opacity = 1;
		});
	});
### 手动触发
	$("img#tri").trigger("unveil");
手动触发加载特定图片而不使用此方法。
### Layzr.js
顺便再说一个刚发布的轻巧、不依赖jQuery的延时加载，同样支持视网膜设备。
### 代码
	<img src="bg.png" data-src="original-img.jpg" data-src-retina="img-retina.jpg" />
	<script src="layzr.min..js"></script>
	<script type="text/javascript">
		var layzr = new Layzr({
			selector: '[data-layzr]', 
			attr: 'data-layzr', 
			retinaAttr: 'data-layzr-retina', 
			bgAttr: 'data-layzr-bg', 
			threshold: 0, 
			callback: null 
		});
	</script>
### 说明
支持IE10+。
