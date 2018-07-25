title: 函数去抖（debounce）与函数节流（throttle）
date: 2018-07-25 16:11:14
tags:
categories:
---
# 前言
拖拽mousemove事件、文字输入等由于频繁被触发而执行DOM操作、资源加载等行为时，可导致性能损耗卡顿。针对此可采用函数去抖（debounce）和函数节流（throttle）解决。

# 函数去抖（debounce）
## 定义
简言之某段时间内不再被触发即执行一次操作。例如乘坐电梯，当人进入电梯时，10s后电梯关门。而当10s内有人再次进入，则中断关门操作，再次等待10s后关门。

## 实现

    /**
     * [debounce] 函数去抖：N ms内不再被触发时即执行一次
     * @param  {[Function]} func:执行函数
     * @param  {[Number]} delay:执行间隔，单位毫秒（ms）
     * @return {[Function]} 去抖函数
     */
    let debounce = (func, delay) => {
        let timer;
        let context;
        let args;

        return function() {
            context = this;
            args = arguments;

            clearTimeout(timer);

            timer = setTimeout(() => {
                func.apply(context, args);
            }, delay);
        }
    };

# 函数节流（throttle）
## 定义
保证某段时间后执行一次操作。针对函数去抖乘坐电梯的例子，如果一直有人进出电梯（这里不考虑超重哈哈），电梯岂不是一直无法运行？而函数节流就是为保证如20s后电梯总要关门一次，并进入下一个周期。

## 实现

    /**
     * [throttle] 函数节流：保证N ms后执行一次
     * @param  {[Function]} func:执行函数
     * @param  {[Number]} delay:执行间隔，单位毫秒（ms）
     * @return {[Function]} 节流函数
     */
    let throttle = (func, delay) => {
        let timer;
        let context;
        let args;
        let lastTime;
        let currTime;
        return function() {
            context = this;
            args = arguments;
            currTime = +new Date();

            clearTimeout(timer);

            if (!lastTime) {
                lastTime = currTime;
            }
            if (currTime - lastTime >= delay) {
                func.apply(context, args);
                lastTime = currTime;
            } else {
                timer = setTimeout(() => {
                    func.apply(context, args);
                }, delay)
            }
        }
    };

# 演示
[演示](http://demo.nimius.net/debounce_throttle)