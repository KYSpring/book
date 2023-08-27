# css学习笔记

## [继承](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Inheritance)

属性继承：
- 概念【继承属性vs.非继承属性】
> 当元素的一个继承属性（inherited property）没有指定值时，则取父元素的同属性的计算值 computed value (en-US)。只有文档根元素取该属性的概述中给定的初始值（initial value）（这里的意思应该是在该属性本身的定义中的默认值）。
>
> 当元素的一个非继承属性(在 Mozilla code 里有时称之为reset property ) 没有指定值时，则取属性的初始值 initial value（该值在该属性的概述里被指定）。

- 控制：【inherit, initial, unset, and revert (en-US)】
> inherit 关键字允许显式的声明继承性，它对继承和非继承属性都生效。
> 可以使用 all 简写属性一次控制所有属性的继承，该属性将其值应用于所有属性，例如：

## [计算属性 Computed value](https://developer.mozilla.org/en-US/docs/Web/CSS/computed_value)

相关的一个api: [Window: getComputedStyle() method](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle)

## column-* 属性族






