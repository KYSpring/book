# svelte精粹

- [svelte精粹](#svelte精粹)
    - [Q1 项目快速初始化](#q1-项目快速初始化)
    - [Q2 动态属性](#q2-动态属性)
    - [Q3 样式](#q3-样式)
    - [Q4 组件：](#q4-组件)
    - [Q5 响应系统：](#q5-响应系统)
    - [Q6 组件通信 - 属性传递：支持默认属性 / 支持对象解构](#q6-组件通信---属性传递支持默认属性--支持对象解构)
    - [Q7 逻辑](#q7-逻辑)
    - [Q8 事件](#q8-事件)
    - [Q9 绑定](#q9-绑定)
    - [Q10 生命周期](#q10-生命周期)
    - [Q11 stores状态管理](#q11-stores状态管理)
    - [Q12 Motion??? Tweening / Spring](#q12-motion-tweening--spring)
    - [Q13 Transition \& animation](#q13-transition--animation)
    - [Q14 元素级别生命周期函数：use](#q14-元素级别生命周期函数use)
    - [Q15 高级样式语法](#q15-高级样式语法)
  - [sveltekit](#sveltekit)
  - [svelte组件引入](#svelte组件引入)
  - [svelte状态管理：store](#svelte状态管理store)
    - [writable stores](#writable-stores)
    - [auto-subscription自动销毁订阅 语法糖](#auto-subscription自动销毁订阅-语法糖)
    - [readable stores](#readable-stores)
    - [derived stores](#derived-stores)
    - [custom stores](#custom-stores)
    - [store bindings](#store-bindings)
    - [context apis](#context-apis)
      - [Contexts vs. stores](#contexts-vs-stores)
    - [模块级上下文](#模块级上下文)
    - [一些问题](#一些问题)
  - [sveltekit](#sveltekit-1)

### Q1 项目快速初始化
1. <font color="red">【不推荐】</font>~~npx degit直接拷贝官方模版，基于rollup构建~~：已经不维护了（[官方模版地址](https://github.com/sveltejs/template)）

```bash
npx degit sveltejs/template my-svelte-project
# or download and extract 
cd my-svelte-project
# to use  run:
# node scripts/setupTypeScript.js
npm install
npm run devs
```
2. <font color="lightgreen">【官方临时推荐】</font>vite构建

```bash
npm create vite@latest myapp -- --template svelte
cd myapp
npm install
npm run dev
```
其配置项非常简单：
```javascript
import { defineConfig } from 'vite'
import { svelte } from '@sveltejs/vite-plugin-svelte'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [svelte()]
})

```
3. <font color="orange">【官方但不稳定】</font>svelte-kit构建，正在发布中
```bash
npm create svelte@latest my-app
# 可选择lib项目 or demo项目
cd my-app
npm install
npm run dev -- --open
```

### Q2 动态属性
- 属性定义：<br>
  通过花括号使用；速记形式`<img src={src}>`速记为`<img {src}>`<br/>
### Q3 样式
  - 一样用style标签 
  - 如何实现动态绑定样式？
### Q4 组件：
  - 嵌套引用
  - 内嵌html：`<p>{@html string}</p>`

### Q5 响应系统：
  - 赋值
  - 响应式声明记号: 可以组合响应的响应；
  ```javascript
  let count = 0;
  $: doubled = count * 2;
  ```
  - 响应式语句记号：很有趣，需要注意其触发条件： 
  > 由于 Svelte 的反应性是由赋值语句触发的，因此使用数组的诸如 push 和 splice 之类的方法就不会触发自动更新
  ```javascript
  $: if (count >= 10) {
	alert(`count is dangerously high!`);
	count = 9;
}
  ```
### Q6 组件通信 - 属性传递：支持默认属性 / 支持对象解构
```javascript
export let answer = '默认值';
```
```html
<Info {...pkg}/>
```
### Q7 逻辑
- 条件
```javascript
{#if user.loggedIn}
{:else if 5 > x}
{:else}
{/if}
```
- 循环
1）如何获取index; 2)为什么要设置key；
```javascript
{#each cats as cat, i (cat.id)}

{/each}
```
- Await块语句
```javascript
{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

// or
{#await promise then value}
	<p>the value is {value}</p>
{/await}
```

### Q8 事件
- dom事件：` on:mousemove={handleMousemove}`
- 事件修饰符：
  ```
   on:click|once={handleClick}
  ```
  > preventDefault ：调用event.preventDefault() ，在运行处理程序之前调用。比如，对客户端表单处理有用。
  >
  > stopPropagation ：调用 event.stopPropagation(), 防止事件影响到下一个元素。
  >
  > passive ： 优化了对 touch/wheel 事件的滚动表现(Svelte 会在合适的地方自动添加滚动条)。
  >
  > capture : 在 capture 阶段而不是bubbling 阶段触发事件处理程序 ()
  >
  > once ：运行一次事件处理程序后将其删除。
  >
  > self ：仅当 event.target 是其本身时才执行。

- 组件自定义事件：
  ```javascript
  import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
  ```

- 组件事件转发简写
```javascript
// 自定义事件转发
<Inner on:message/>
// dom事件转发
<button on:click>
```

### Q9 绑定
文本输入绑定 / 数字输入绑定 
```html
<!-- 文本输入绑定 -->
<input bind:value={name}>
<!-- 数字绑定 -->
<input type=number bind:value={a} min=0 max=10>
<input type=range bind:value={a} min=0 max=10>
<input type=checkbox checked={yes}>
```
/ each块绑定 / 媒体标签绑定

/ 尺寸绑定（dimension）？？怎么用/和class或者style的区别是什么
> 每个块级标签都可以对 clientWidth、clientHeight、offsetWidth 以及 offsetHeight 属性进行绑定：

/ this绑定：可以获得当前dom节点

/ 组件绑定 / 通过this绑定组件dom节点

### Q10 生命周期

onMounted: 如果有返回回调函数，则将在dom destroy阶段调用。

onDestroy / beforeUpdate

tick(): 
> When you update component state in Svelte, it doesn't update the DOM immediately. Instead, it waits until the next microtask to see if there are any other changes that need to be applied, including in other components. 

### Q11 stores状态管理

- 可写的store
```javascript
import { writable } from 'svelte/store';

export const count = writable(0);

//  It's a writable store, which means it has set and update methods in addition to subscribe.
```
- 自动订阅


1）发布订阅模式：订阅 / 取消订阅
```javascript
const unsubscribe = count.subscribe(value => {
	countValue = value;
});

onDestroy(unsubscribe);
```
2）简写模式：`<h1>The count is {$count}</h1>`

- 只读的store
```javascript
import { readable } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});

```
- 可推导的store
- 定制store
- 绑定store

### Q12 Motion??? Tweening / Spring
### Q13 Transition & animation
### Q14 元素级别生命周期函数：use
```html
<div class="box" use:clickOutside on:outclick="{() => (showModal = false)}">
	Click outside me!
</div>
```
### Q15 高级样式语法
- class指令：
```html
<button
	class:selected="{current === 'foo'}"
	on:click="{() => current = 'foo'}"
>foo</button>

<div class:big>
	<!-- ... -->
</div>
```
- 行内style：
```html
<p style="color: {color}; --opacity: {bgOpacity};">
  This is a paragraph.
</p>`
```
- style指令：
```html
<p style:color style:--opacity={bgOpacity}>This is a paragraph.</p>
```

【未完待续】

## sveltekit

https://kit.svelte.dev/


## svelte组件引入

js引入 vs. 模版引入
引申问题，vue是否支持？

```javascript
import Dialog from '@components/dialog/index.svelte';
// 方式1
new Dialog({
  target: document.body,
  props: {
    title: 'hello world',
    isShowFooter: true,
  },
});
// 方式2:
<Dialog
  title="基础服务授权"
  on:cancel={handleCancel}
  on:confirm={handleConfirm}
  on:close={handleClose}
>
```

## svelte状态管理：store
### writable stores

```javascript
import { writable } from 'svelte/store';

export const count = writable(0);
```

- 拥有两个内置函数：set / update 
- 需要注意在onDestory的时候需要手动unsubscribe;
### auto-subscription自动销毁订阅 语法糖

\$+store名称

### readable stores

```javascript
import { readable } from 'svelte/store';

export const time = readable(null, function start(set) {
	// implementation goes here	
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});
```

### derived stores
根据已有的store变量生成一个新的store变量；

```javascript
import { readable, derived } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});

const start = new Date();

export const elapsed = derived(
	time,
	$time => Math.round(($time - start) / 1000)
);
```

### custom stores
自定义一个store对象，本质是在原生store之上进行了一层封装；

```javascript
function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => update(n => n + 1),
		decrement: () => update(n => n - 1),
		reset: () => set(0)
	};
}
```
### store bindings
> If a store is writable — i.e. it has a set method — you can bind to its value, just as you can bind to local component state.

牛皮，所以vue3支不支持这种双向绑定


### context apis

[官方文档](https://svelte.dev/tutorial/context-api)

使用示例：
```javascript
import { onDestroy, setContext } from 'svelte';
import { mapbox, key } from './mapbox.js';

setContext(key, {
	getMap: () => map
});
```
```javascript
import { getContext } from 'svelte';
import { mapbox, key } from './mapbox.js';

const { getMap } = getContext(key);
const map = getMap();
```

> 注意：只能在组件及其子孙组件中使用

#### Contexts vs. stores
Contexts and stores seem similar. They differ in that stores are available to any part of an app, while a context is only available to a component and its descendants. This can be helpful if you want to use several instances of a component without the state of one interfering with the state of the others.

### 模块级上下文

能够在多个组件之间共享一份数据内容，一个例子：
[参考](https://www.educative.io/answers/how-to-use-module-context-in-svelte)

```html
<script context="module">
    let totalCount=0
    export function totalCount(){
        return totalCount
    }
</script>

<script>
     let count=0
     function btnHandler(){
         count += 1
         totalCount += 1
     }
</script>

    <h2>Cart counter - {count}</h2>
    <button on:click ={btnHandler}>Increment Cart </button>
    <hr />
```

### 一些问题
- 若如下所示的初始化在多个组件中被调用，是否会导致sore反复初始化？过去元素丢失？
```javascript
import { writable } from "svelte/store";

const createHonorBindStore = () => {
	const { subscribe, set, update } = writable({});

	return {
		subscribe,
		updateBind: (gameId: string) => {
			console.log('update/gameId', gameId);
			update((honorState) => {
				honorState[gameId] = true;
				return honorState;
			});
		},
		reset: () => set({}),
	};
}

export const honorBindState = createHonorBindStore();
```

【回答】似乎不会，官方例子即是分布组件中调用的，但需要注意语法糖具体在什么场景下不能使用？
> Auto-subscription only works with store variables that are declared (or imported) at the top-level scope of a component.

- props透传：
  1、支持{...obj}的形式向子组件传递props；
  2、特殊关键字$$props可用于将当前组件自身的全部Props传递给子组件而无论子组件是否export, 但需要解决ts报错的问题；

## sveltekit