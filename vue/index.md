<link rel="stylesheet" type="text/css" href="style.css" />

# vue.js精粹

## vue的安装和引入方式
- `<script>` 标签引入
- npm
- 命令行工具

能够支持上述多种引入方式的重要原因在于其实现了vue的多版本构建：
- 完整版 vs. 只包含运行时版
- umd / cjs / es module
  
[参考](https://v2.cn.vuejs.org/v2/guide/installation.html#Vue-Devtools)

## 组件基础
* 组件如何定义？最原始的定义方式？
  
 `Vue.component(name, instance)`
```javascript
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

* data为什么一定是一个函数？ 
  
  A: 组件无限使用 => 状态维护 => 函数闭包

* 父子组件通信方法？ A：组件传值 / 事件监听
* 动态组件

## 组件深入
* 注册的方式有哪些？
  
  > 全局注册: `vue.component()`  
  > 局部注册:  `components: {
        ComponentA
    }`   

## 插件
* 插件的基本定义和使用范式？
    >`plugin.install` 和 `Vue.use(plugin)`

## 深入响应式原理
