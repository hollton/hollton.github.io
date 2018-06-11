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

### android端报告横向滑动
问题：发现小米、华为机型下，报告panel横向滑动内容4无法滑动
![](/img/work_report_mobile.png)
排查：点击5菜单展开遮罩再收起后，4即可滑动。查看遮罩样式发现有设置height:100%，手动为3内滚动框设置height:100%即可正常滚动（不可思议有什么关联）。但出现其它问题，滚动到底部会使1头部消失。为3设置计算高度height:calc(100% - xx);4panel横向滚动失效(┬＿┬)。设置3height:100%，同时设置1position:fixed解决滚动底部1消失问题，但存在点击5菜单6panel-title无法准确定位问题，排查后发现是因为触发了2外框的滚动，即双滚动条的定位。切图排查也无解后最终采用唐总的思路：点击菜单定位同时控制外框滚动。
解决：针对android端做处理，减少影响范围。设置3内滚动框height:100%+设置1头部fixed定位+点击菜单跳转最后一个目录控制2外滚动框滚动到底部，否则就滚动到顶部。
新问题：三星等特定机型向下滚动时，内滚动条滚动到底部后，不会触发外滚动条。导致不能完全显示全部内容，各种尝试无果后，只能还原样式更改，使用draggable.js拖拽解决横向滑动问题。这里还出现一个小问题：拖拽绑定后，table内的click失效。通过设置拖拽句柄而不把拖拽设置在table上解决

### 未加密URL参数导致前端页面获取400
问题：测试环境jsp页面url带有&question={...}时，服务器返回400，而本地运行则正常。后了解到服务器tomcat有做升级，未加密的url {}无法正确解析。encodeURIComponent处理解决，提供给url参数数据都应做加密处理，获取再解密。

### postMessage IE8 接收不到消息
项目中在iframe页向父级发送postMessage消息，且兼容到IE8，问题就出现在IE8下 `window.attachEvent('message', function(event){})` 监听不到消息，后查找[资料](http://www.zhangxinxu.com/study/201202/web-messing-cross-document-messaging-two-iframe.html)是IE8兼容问题，需使用'onmessage'，完美解决。

iframe.html:
	
	// IE9及以下不支持发送object消息
	window.parent.postMessage(JSON.stringify(msgObj), '*')

parent.html:

	if(window.addEventListener) {
		window.addEventListener('message', function (event) {
           console.log(event.data)
		});
	}else{
		window.attachEvent('onmessage',function (event) {
			console.log(event.data)
		});
	}

### IE8 图片加载失效
使用`image.onload`未触发加载成功响应，触发onload事件的条件是资源从服务器传到本地，即下载完成。但如果资源指向url已在本地有缓存，IE8 及 firefox 下就不会触发onload。因此应当把`image.src`赋值代码放在最后执行，即：

	let image = new Image()
	// image.src = url
	image.onload = () => { ... }
	image.onerror = () => { ... }
	image.src = url

### Github demo 添加预览地址
在 demo.index 源码 url 前添加`http://htmlpreview.github.io/?`，例如：

demo ：`https://github.com/hollton/image-viewer/blob/master/demo/index.html`

添加前缀：`http://htmlpreview.github.io/?https://github.com/hollton/image-viewer/blob/master/demo/index.html`

### git merge push fail
git merge，然后 push 到 gerrit 上提示 no new changes 。此时是线性合并，合并的历史 commit 节点已被评审，所以 gerrit 认为无新提交，不允许提交评审。使用 `git merge xx --no-ff` 合并生产一条新的 commit 。

### shuffle 洗牌排序算法
[Fisher-Yates_shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)

    var shuffle = function(metadata) {
        metadata.forEach((item, index) => {
            var randomIndex = Math.floor(Math.random() * (metadata.length - index));
            var randomItem = metadata[randomIndex];
            metadata[randomIndex] = metadata[index];
            metadata[index] = randomItem;
        })
        return metadata;
    };
    

### chrome type="password" autocomplete="off" 失效
`autocomplete="new-password"`解决

### 函数名判断在混淆代码失效
使用函数名判断是否执行函数，代码混淆后函数名被更改，导致判断失效。

### ios输入框不失焦
输入框完成输入，点击其他处，焦点仍在输入框内。主动触发失焦

	function isTextInput(node) {
		return ['INPUT'].indexOf(node.nodeName) !== -1;
	}

	document.addEventListener('touchstart', function(e) {
		if (!isTextInput(e.target) && isTextInput(document.activeElement)) {
			document.activeElement.blur();
		}
	}, false);