title: React源码解析
date: 2018-11-15 14:55:42
tags:
categories:
---
本次分析的源码采用的是16.6.1的版本，和16版本之前的源码对比，入口和构造器有些差异。

# React API

`npm i react --save`
安装好react之后，打开`node_nodules/react/index.js`入口文件。指向打开开发版本代码`react.development.js`。
源码分析自然从对外暴露的接口入手，即最下面的`React`。

<!-- more -->

    var React = {
      Children: {
        map: mapChildren,
        forEach: forEachChildren,
        count: countChildren,
        toArray: toArray,
        only: onlyChild
      },

      createRef: createRef,
      Component: Component,
      PureComponent: PureComponent,

      createContext: createContext,
      forwardRef: forwardRef,
      lazy: lazy,
      memo: memo,

      Fragment: REACT_FRAGMENT_TYPE,
      StrictMode: REACT_STRICT_MODE_TYPE,
      Suspense: REACT_SUSPENSE_TYPE,

      createElement: createElementWithValidation,
      cloneElement: cloneElementWithValidation,
      createFactory: createFactoryWithValidation,
      isValidElement: isValidElement,

      version: ReactVersion,

      __SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED: ReactSharedInternals
    };

## Children 子组件

为处理`this.props.children`封闭的数据结构提供了的工具集。`this.props`对象的属性与组件的属性一一对应，但有个例外，就是`this.props.children`属性，它表示组件的所有子节点。

## Components 组件

Components 是 React 组件使用 ES6 类(classes) 定义时的基类。

    Component, // 创建组件
    PureComponent, // 创建纯组件
React 将UI拆分为独立的可重用的模块，并且每个模块逻辑也是独立的。PureComponent与Component的区别仅在于PureComponent内部实现了shouldComponentUpdate()中的 props 和 state 的浅比较，来提升性能。但若包含复杂数据结构判断时会有偏差，后续详细介绍。

    memo, // 高阶组件，用于包裹函数式组件，而不是类组件

## Refs

    createRef, // 创建一个ref，可以通过ref属性关联react元素
    forwardRef, // 创建一个组件，将它接收的 ref 属性转发给组件树下的另一个组件

## createElement 创建元素

    createElement, // 创建返回一个新的给定类型的 React 元素，JSX 元素即是它的语法糖，因此JSX不必显示调用
    createFactory, // 同上
    cloneElement, // 克隆的元素将浅层合并props

## isValidElement 判断元素

    isValidElement, // 验证对象是否是一个 React 元素。 返回 true 或 false

## Fragments 片段

    Fragment, // 渲染多个元素而无需创建额外的 DOM 元素

## createContext 创建Context组件

    createContext, // 创建一个 Context 对象，可用于跨组件传递内容。
                   //该对象包含 Provider（提供数据） 和 Consumer（消费数据），分别为两个 React 组件

## Code-Spliting 代码拆分

    lazy, // 动态渲染组件
    Suspense, // lazy渲染加载时显示的内容

## 小结

`React`是 React 库的入口，以上方法都可通过`React`变量调用。

# 组件的实现

当我们使用React创建一个组件时，是否有好奇过它是什么？是DOM？JavaScript？还是其他意识形态？接下来我们详细看看。

## 组件本质是对象

编写一个简单的组件，并`log`输出看得到了什么。

    import React, { Component } from 'react';

    class App extends Component {
      constructor(props){
        super(props);
        this.state = {};
      }

      render() {
        return <div>组件</div>;
      }
    }

    console.log(<App />);
    export default App;

`console.log`输出`<App />`
![](/img/react_code/simple_module_log.png)

可以看出`<App />`并不是真实的DOM而是js对象，带有很多属性。此时`props`是空对象，给它加点内容看看。

    console.log(<App><p>新增</p></App>);
![](/img/react_code/simple_module_add_log.png)
`props`发生了变化，由于`<App>`组件嵌套了`<p>`，`<p>`中嵌套文字，因此在描述`<App>`对象中多了`children`属性，用来描述`div`对象。依此类推可进行多层组件嵌套。

## 虚拟DOM

我们知道了react 的组件只是对象，而真正的页面是由一个个 DOM 节点组成的，在jQuery时代，通过JS来操纵真实的DOM元素，而复杂或频繁的DOM操作通常是性能瓶颈产生的原因，因此React引入了虚拟DOM（Virtual DOM）的概念。
[React虚拟DOM浅析 | AlloyTeam](http://www.alloyteam.com/2015/10/react-virtual-analysis-of-the-dom/)
简单来说就是，无论多复杂的操作，都先进行虚拟DOM的JS计算，组件对象计算好之后再一次性通过Diff算法进行渲染或者更新，而不是每次都直接操作真实的DOM。

## 组件的本源

`<App />`组件是基于`React`的`Component`创建，所以我们打开`react.js`追溯到`Component`查看代码：
![](/img/react_code/component.png)
单纯的构造函数，接收`props`、`context`等参数，并且`prototype`原型上定义`setState`方法，接收的两个参数对于我们平常开发来说并不陌生。

## 小结

至此我们发现声明的组件`<App />`是继承自`React.Component`类的子类，其原型具有`setState`等方法。
![](/img/react_code/component.xmind.png)