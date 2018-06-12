---
title: 图片预览控件：支持旋转，缩放，拖动
author: 刘浩东
date: 2018-06-11 21:08:04
categories:
- 前端
- 公共控件
---
## 背景
项目中需实现简易的图片预览功能，满足图片的旋转、缩放及放大后的拖动查看即可。考虑到现有的开源组件与其交互、UI、功能需求等差异，自己封装成组件实现。
[demo](http://htmlpreview.github.io/?https://github.com/hollton/image-viewer/blob/master/demo/index.html)
![](/img/clipboard.png)

<!-- more -->

## 使用

### 引入

```
npm i es-image-viewer --save
import imageViewer from 'es-image-viewer'
```

### 初始化 initImage
捕获待操作图片，配置初始化参数。
```
/**
 * imgDom: 必须，操作图片dom
 * containerDom: 非必须，容器dom, 默认为图片dom父节点。用于计算与限制图片拖动范围
 * options: 非必须，可选配置
 * {
 *   disableWheel: bool, // 禁用滚轮缩放，默认false
 *   rotateStep: Number, // 旋转角度步进，默认90
 *   scaleStep: Number, // 缩放步进，默认0.25
 *   scaleRange: Array // 缩放约束范围，默认[0.5, 2]
 * }
 */
initImage(imgDom, containerDom, options)
```

### 图片旋转，缩放操作 handleImage
```
/**
 * {
 *   type: string, // 操作类型，支持'rotate'、'scale'
 *   inc: Number // 操作增量，1为右旋或放大；-1为左旋或缩小
 * }
 */
handleImage({type: 'string', inc: Number})
```

### 图片操作回调 handleCallback
图片操作返回当前图片旋转、缩放信息，便于后续操作，如提示缩放比例。
```
/**
 * callbackFunc: function //回调执行函数
 * return: 
 * {
 *   operateType: 'string', // 操作类型 'rotate' || 'scale'
 *   rotate: Number, // 图片旋转角度
 *   scale: Number, // 图片缩放比例
 * }
 */
handleCallback(callbackFunc)
```

## 实现

### 初始化 initImage
接收options参数并初始化数据，添加鼠标及滚轮事件监听。
```
const _initData = {
    imgDom: null, // 操作元素
    containerDom: null, // 容器元素，默认为父容器，用于限制imgDom拖动范围
    disableWheel: false, // 禁用滚轮缩放
    operateType: '', // 操作类型，'rotate' || 'scale'
    rotate: 0, // 旋转角度
    rotateStep: 90, // 旋转步进
    scale: 1, // 缩放比例
    scaleStep: 0.25, // 缩放步进，建议0.25（0.1在加减运算中计算不精确）
    scaleRange: [0.5, 2], // 缩放约束范围
    dragData: { // 拖拽
        draggable: false, // 是否可拖拽
        posX: 0, // 拖拽点相对于图片位置坐标
        posY: 0,
        isRotate: false // 是否已旋转，对调调整图片宽高，再用于与容器宽高比较
    }
};

Object.assign(_imageData, {
    ..._initData,
    imgDom: imgDom,
    containerDom: containerDom,
    ...options
});

_handleMouseDown();
_handleWheel();
```

#### 鼠标按下事件监听 _handleMouseDown
处理图片拖拽，判断仅当图片的缩放宽高超出容器宽高，才可执行拖动，并记录图片位置信息。监听鼠标移动mousemove、抬起mouseup及移出mouseleave事件。鼠标移出需等同处理mouseup事件，取消拖动。否则在iframe引用场景下，移出iframe后松开鼠标并未触发mouseup事件，再移入仍会保留拖动效果。
```
let imagePos = _getImagePos();
if (imagePos.scaleWidth > imagePos.containerWidth || imagePos.scaleHeight > imagePos.containerHeight) {
    dragData.draggable = true;
    dragData.posX = event.clientX - imagePos.left;
    dragData.posY = event.clientY - imagePos.top;

    // 鼠标事件监听
}
```

#### 鼠标移动事件监听 _handleMouseMove
在允许拖动范围内移动，计算设置图片位置信息
```
event.preventDefault();
let { dragData, imgDom } = _imageData;
let imagePos = _getImagePos();
if (dragData.draggable) {
    // 横向拖动设置图片位置信息
    if (imagePos.scaleWidth > imagePos.containerWidth) {
        var x = event.clientX - dragData.posX;
        x = x > imagePos.leftMax ? imagePos.leftMax : (x < imagePos.leftMin ? imagePos.leftMin : x);
        imgDom.style.left = `${x}px`;
    }
    // 纵向
    
    _setImageData({
        imgDom
    })
}
```

#### 鼠标抬起事件监听 _handleMouseUp
拖动结束，设置不可拖动标识
```
let { dragData } = _imageData;
dragData.draggable = false;
```

#### 滚轮事件监听 _handleWheel
滚轮事件执行缩放
```
if (disableWheel) {
    return;
}
let deta = event.deltaY;
let scaleInc = deta >= 0 ? -1 : 1;
_handleScale(scaleInc);
```

### 图片旋转，缩放操作 handleImage
根据操作类型type，执行对应图片操作；根据操作增量inc，执行操作数据。
```
let inc = handleOption.inc >= 0 ? 1 : -1;
case 'rotate':
    _handleRotate(inc);
    break;
case 'scale':
    _handleScale(inc);
```

#### 设置旋转角度 _handleRotate
增量inc===1为逆时针旋转，inc===-1为顺时针旋转。根据旋转步进rotateStep设置旋转角度。执行回调handleCallback返回当前操作及图片信息。

#### 设置缩放值 _handleScale
增量inc===1为放大，inc===-1为缩小。判断缩放值在允许范围scaleRange内时，根据缩放步进scaleStep设置缩放。执行回调handleCallback返回当前操作及图片信息。

### 图片操作回调 handleCallback
```
let { rotate, scale, operateType } = _imageData;
if (operateType) {
    callbackFunc({
        rotate,
        scale,
        operateType
    })
}
```