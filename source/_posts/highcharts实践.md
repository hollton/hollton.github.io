---
title: highcharts实践
author: 刘浩东
date: 2017-10-28 19:53:27
categories:
- 前端
- 第三方库
tags: 
- highcharts

---
## highcharts介绍
Highcharts是一个用纯JavaScript编写的图表库，基于svg技术。可兼容支持IE6+以及移动端，但低版本浏览器的渲染性能则稍差。Highcharts支持多种图表类型，较为常用的是柱状图、饼状图、雷达图。

<!-- more -->


## 引入

### CND服务
    <script src="http://cdn.hcharts.cn/highcharts/highcharts.js"></script>

### npm
    npm install highcharts --save

### 基础与扩展功能模块
使用特殊图表（如雷达图，箱线图等）或额外功能（如导出，3D图表），则需引入对应的功能模块：

<ul>
    <li>更多图表类型扩展模块（highcharts-more.js）</li>
    <li>3D 图表模块 （highcharts-3d.js）</li>
    <li>导出功能模块（modules/exporting.js）</li>
</ul>

需要注意的是highcharts.js需在功能模块之前引入

	var Highcharts = require('highcharts');
	require('highcharts/modules/exporting')(Highcharts);

## 创建图表

### 创建容器
给定一个图表Dom，可指定宽高，默认为600px * 400px

    <div id="container"></div>

### 配置代码
指定基础配置，业务配置项以及数据后，执行highcharts方法即可绘制图表。

	var baseOption = {
	    chart: { type: 'line' },  //图表类型，默认
	    xAxis: {
	        categories : axisData.xData,
	        title: { text: $scope.xxTitle }
	    },
	    yAxis: {
	        min: 0,
	        max: 100,
	        title: { text: $scope.yTitle }
	    },
	    series: []
	};
	
	//业务配置
	if($scope.customOption){
	    baseOption = _.merge(baseOption,$scope.customOption);
	}
	
	//填充数据...
	
	$('#container').highcharts(baseOption);

## 实践总结

### 移动端渲染抖动
渲染图表时有一个从无到有的过程，通过tab切换数据渲染图表时，在移动端会有较明显的页面跳动，可通过对图表容器设置min-height解决。

### 依据数据量调整图表宽高
图表宽高会依据父级宽高自适应，但当数据量较大时，由于宽高已由父级确定，会使得图表的数据项挤在一起。这时需要依据数据量手动计算宽高重置图表的宽高，如例子[图表自适应高度](https://code.hcharts.cn/hollton/HSKjLw)

## Highcharts VS ECharts

<ul>
    <li>ECharts基于Canvas技术，动态渲染功能丰富；Highcharts基于SVG技术，动态增删节点十分灵活</li>
    <li>Highcharts只供个人及非商业用途使用；ECharts开源使用</li>
    <li>Highcharts示例及Api文档十分详尽；ECharts Api文档不够清晰</li>
    <li>Highcharts对低版本Web浏览器及移动端支持较好；ECharts则稍欠缺，低版本Android特定机型出现过渲染失败的状况</li>
</ul>