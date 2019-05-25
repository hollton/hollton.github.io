title: Chrome开发者工具
date: 2019-05-25 11:14:27
---

面板上包含了Elements面板、Console面板、Sources面板、Network面板、
Timeline面板、Profiles面板、Application面板、Security面板、Audits面板这些功能面板。

<!-- more -->

# Elements

## HTML结构面板

* 双击可编辑dom标签、属性attr及其值
* 右键选项：
    - Edit as HTML: 实时编辑dom
    - Break on: 断点监听
        - subtree modifications(树结构改变)
        - attributes modifications(属性改变)
        - node removal(节点移除)

## 样式等面板

* Styles
    - 查看各级样式控制，修改及时生效
    - 图标+：增加选择器，控制样式
    - 图标鼠标箭头：调出伪类选择lvha
    - 盒模型
* Computed
    - 最终生效的样式
* Dom Breakpoints
    - 断点列表，可勾选生效

# Network

监控当前网页所有的http请求的面版
![](/img/chrome/network.png)
* Preserve log: 即使页面重定向仍保留请求
* Offline Online:模拟弱网环境，可有效复现偶发问题
* Filter: 匹配请求
* XHR: 只显示http请求类型，常用
* 点击某个请求：
    - Headers:请求地址及方法（request url/method）
        + Request Headers: 请求头，如Authorization
        + Query String Parameters:请求参数
        + Request Payload: 请求体（put post时有）
    - Preview: 数据结果（对象格式，Response为json串格式）

# Console

* $0-$4：依次返回五个最近的元素面板选择的DOM历史记录，$0常用当前选择的dom元素
* Preserve log：类似 Network 保留log记录
* ~~$('selector'):返回指定选择器匹配的第一个元素，同document.querySelector('selector')~~

# Sources

![](/img/chrome/source.png)
* 调试js代码，若代码是混淆的，可点击左下角"{}"增强可读性
* ctrl+p 可查找具体代码文件，当输入':'+行数m时（或ctrl+g快捷键），跳转到当前文件第m行
* ctrl+f 可在当前代码中查找指定字段
* ctrl+shift+f 可全局查找指定关键字段
* 设置断点,右上角图标依次表示
    - 停止当前的断点调试
    - 执行下行代码，遇到函数不跳入执行（f10）
    - 跳入函数中执行（f11）
    - 从执行的函数中跳出
    - 禁用所有断点
    - 程序遇到异常是是否中断
* 修改source文件，ctrl+s保存，可直接生效。若需要重刷页面时就跑新增的代码时，则可以先设置断点拦截再修改代码。

<p style="display: none;">
    https://www.cnblogs.com/constantince/category/712675.html
    https://www.cnblogs.com/onetwo/p/5519121.html
    https://blog.csdn.net/m0_37438418/article/details/80702459
</p>
