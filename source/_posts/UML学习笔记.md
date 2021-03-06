title: UML学习笔记
date: 2017-05-14 16:28:37
tags: [UML]
categories: [前端]

---

# 基础知识

## 定义
UML（Unified Modeling Language）:统一建模语言。


<!-- more -->

## 分类
结构型（Structrue Diagram）：
<ul>
    <li>类图(Class)</li>
    <li>对象图(Object)</li>
    <li>构件/组件图(Component)</li>
    <li>部署图(Deployment)</li>
    <li>包图(Package)</li>
</ul>
行为型（Behavior Diagram）：
<ul>
    <li>活动图(Activity)</li>
    <li>状态机图(State Machine)</li>
    <li>顺序图(Sequence)</li>
    <li>通信图(Communication)</li>
    <li>用例图(use Case)</li>
    <li>时序图(Timing)</li>
</ul>

# 类图（业务模型分析）

## 概念
一个类用矩形方框表示，最上面是类的名字，中间是属性，最下面是操作。
![](/img/class_diagram.png)
上图 +属性1:int ,+表示为public类型，int表数据类型。操作同。

## 设计类步骤
<ul>
    <li>识别出类</li>
    <li>识别出类的主要属性</li>
    <li>描绘出类之间的关系</li>
    <li>对各类进行分析、抽象、整理</li>
</ul>

## 类间关系

### 关联关系（“直线”关系）
暂不确定两个类的关系时，可先用直线连接标明有某种关系（类用矩形框画出，下面用[]示意）
[A]1—1[B]：A、B类之间为一对一关系；
[A]1—\*[B]：一对多关系，\*表示0到多个；
[A]1—x..y[B]：一对（x到y）关系；
[A]—>[B]：由类A可找到类B，可记为“导航”关系。

### 包含关系
可用下类图表示一个部门有多个员工，父元素用菱形标记。
![](/img/class_diagram_include.png)
聚合（弱包含）：空心菱形◇，子元素可以有多个父元素，当父元素不存在时，子元素仍可继续存在；
组合（强包含）：实心菱形◆，子元素只能有一个父元素，当父元素不存在时，子元素也不再存在。

### 泛化关系（继承关系）
[学生]—▷[人]：用三角形表示，由继承者指向被继承者，即类学生继承了类人，正式称谓应为类学生泛化为类人（即有学生类可提炼抽象出人）。

### 依赖关系（虚线+箭头）
[酒鬼]——>[酒]：依赖者用虚线及箭头指向依赖对象。

### 递归关系（自包含关系）
如在表示文件夹与文件的关系时，可用文件夹类包含自己表示文件夹也可包含文件夹
这种递归结构展开时就为树形结构，因此树形结构可用自包含、自关联来表示

### 三角关系
公司与雇员之间还有一个劳动合同类的关联类，对这两个类的关系近一步约束
![](/img/class_diagram_triangle.png)

# 活动图（流程分析）

## 基础语法
初始状态、结束状态、活动、判断、合并、监护、泳道
![](/img/activity_diagram_case.png)
<ul>
    <li>一个开始状态●，一个或多个结束状态⊙</li>
    <li>箭头表示流程的走向</li>
    <li>圆角矩形表示活动，可理解为流程中的一个步骤</li>
    <li>菱形且带有[XXX]表示判断分支条件，[]中称为监护</li>
    <li>分支汇合到另一个菱形，为合并</li>
    <li>泳道/分区：对参与者进行区分，表示动作的发起者</li>
</ul>

# 状态（机）图（流程分析）

## 与活动图的区别
活动图是流程分析的万能图，什么流程都可以用活动图表达，但是如果 流程是围绕某一事物的状态展开时，应首选状态图。

## 基础语法
初始状态、结束状态、状态、转换、监护（标在转换文字上，说明状态分支的转换条件）
![](/img/state_diagram.png)
<ul>
    <li>一个开始状态●，一个或多个结束状态⊙</li>
    <li>圆角矩形表示状态</li>
    <li>状态之间的箭头称为转换，箭头上的说明文字说明发生的事情导致状态发生变化。如果流程涉及多种角色，应说明什么角色做了什么事情从而导致状态发生变化。</li>
</ul>

# 顺序图（流程分析）

## 基础语法
![](/img/sequence_diagram.png)
<ul>
    <li>角色：公仔形象及名字</li>
    <li>生命线：虚线表示</li>
    <li>激活框（会话）：长矩形，每一段长矩形表示一次会话，每次会话表示一次交互</li>
    <li消息：实线箭头，可指向其他角色，也可指向自己，标明对谁做了什么事</li>
    <li返回值：对消息的响应反馈</li>
</ul>

## 实践建议
<ul>
    <li>任何复杂交互都可分解为：自己与自己、自己与他人、他人与他人</li>
    <li>从复杂业务中整理出一条一条流程，对流程逐个分析</li>
    <li>分析出什么角色参与到这个流程</li>
    <li>将流程分解为角色与角色之间的交互</li>
    <li>用顺序图按照时间顺序将这些交互组织起来</li>
    <li>读图顺序：由上至下，从左到右</li>
</ul>

## 循环及分支结构
![](/img/sequence_loop_alt_diagram.png)
这三种流程要放在一个框（frame）中，frame是可以嵌套的，且嵌套的层次没有限制：
<ul>
    <li>loop(循环)：[循环条件]，中括号内容是循环条件，如果满足循环条件，则不断重复执行本框中的内容</li>
    <li>alt(alternative条件分支)：[执行条件]，根据不同条件选择不同分支</li>
    <li>opt(optional可选分支):[执行条件]，如果满足该条件则执行该分支，否则跳过</li>
</ul>

## 流程分析总结

### 活动图、状态图、顺序图
活动图的特点：
<ol>
    <li>强调每个角色做了什么事情，及这些事情的先后关系</li>
    <li>适合表达各种特殊流程，如分支、并发等</li>
</ol>

状态图的特点：
<ol>
    <li>事情围绕某件东西开展</li>
    <li>其有着不同的状态，状态会因发生了一些事情而变化</li>
</ol>

顺序图的特点：
<ol>
    <li>强调各角色间的交互，信息传递很明确</li>
    <li>强调按时间顺序分别发生了什么事情</li>
    <li>不太适合表达复杂的特殊流程，如循环、条件分支</li>
</ol>

### 实践选取
<ul>
    <li>如果事情是围绕某个东西开展的，可以考虑用状态图，否则可用顺序图或活动图</li>
    <li>如果没有复杂的特殊流程，可考虑顺序图，否则可考虑活动图</li>
    <li>可同时使用多种图，从多个角度分析问题，也会加深对各种图的理解</li>
</ul>

# 用例图（系统行为）

## 定义
描述什么角色通过某种系统能做什么事情。其关注的是系统的外在表现、系统与人的交互及系统与其他系统的交互。
![](/img/case_diagram.png)

## 基础知识
执行者：小人表示，不同角色的工作责任不一样，用到的系统功能也不一样。
用例：圆圈及文字表示，表明系统能做的事情。
系统边界：大方框表示，只框住用例，不框住执行者。
关联：用直线或箭头表示角色与用例之间的关系，表某执行者能执行什么用例。
