title: ND北京高精尖项目总结
date: 2017-05-14 17:41:07
categories: [工作]
---
# 工作时间
2016/02/02 - 至今
## 项目概述
## 工作内容
## 工作总结
### Hash定位
需求：业务中移动端使用web html5页面，左上角返回按钮调用`history.back()`关闭当前页回退。

最初实现：设置hash值再定位到此，即：

	$location.hash(idName);
    $anchorScroll();
问题：每次定位会记录到历史记录，使用`history.back()`回退时会回到每次hash定位位置，导致移动端多次点击回退按钮才会关闭页面。

改进：关键使用`$location.replace();`不记录历史信息：先获取当前url地址，并去除hash信息，再添加上hash锚点，使用`replace()`替换当前url地址且不会存入历史记录，最后滚动定位到hash位置。

	var url = $location.url();
	url = url.substr(0,url.indexOf('#'));
	$location.url(url+'#'+idName).replace();
	$anchorScroll();

### 异步加载
问题背景：使用VueJs开发作答组件，对服务端返回的数据进行音视频id解析，并用videojs渲染展示。
问题：IE下存在多个视频的情况下，总会出现视频渲染失败的情况，videojs报错提示`The element or id is not exist`即无法找到要渲染的dom元素。
解决尝试：由于渲染的时机我是放在mounted之后，我最初怀疑ie下mounted后挂载有问题？然而我试着延时5s再渲染问题仍旧存在才意识到问题并没有那么简单[允悲]。
醍醐灌顶：吃晚饭的时候到了，主程唐总来约着吃饭，和他说了这个问题，看了下ie下渲染的视频id，指出可能是因为这重复的id。原来是给id追加indx的部分不够严谨，ie下异步会计算出重复index，若id也一样，就会导致报错，紧接着其他视频也渲染出错。

### ng-repeat内使用编译指令
问题背景：需要在ng-repeat内容动态插入html并可执行ng-click事件获取当前item。如下方案显然不可取，因为compile的scope与repeat的scope不是同一个作用域，并不能获取到

    var html = '<a ng-click=clickFn(item)></a>';
    var chtml = $compile(html)($scope);
    $('p').append(chtml);

解决：compile时新建作用域

    var html = '<a ng-click=clickFn(item)></a>';
    var newScope = $scope.$new();
    newScope.item = item;
    var chtml = $compile(html)(newScope);
    $('p').append(chtml);

### v-model数据更新
问题背景：获取当前题的批阅分值，添加到题目对象上并使用v-model绑定在input。切换题目时若下一题的批阅分值为空且没有为题目对象初始化分值属性，使得v-model数据未更新，仍显示上一题的数据。
解决：每次切换均初始化分值属性，无则设置为null，保证切换时v-model更新。同时注意vue中为数组对象设置false类型值（如false，''，null）时，直接使用`item.score = null`无效，需使用`this.$set(item,'score',null)`