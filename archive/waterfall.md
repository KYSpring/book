# 瀑布流的实现
[腾讯文档版本](https://docs.qq.com/doc/DZURWSGxNdXhEeEVi)
## 一些问题：
1、有无成熟的瀑布流组件；
2、

## 方法1: multi-column 多栏布局实现瀑布流 

[基础知识：multicol布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_multicol_layout/Basic_concepts)

```css
column-count: 设置共有几列
column-width: 设置每列宽度，列数由总宽度与每列宽度计算得出
column-gap: 设置列与列之间的间距
```
column-count和column-width都可以用来定义分栏的数目，而且并没有明确的优先级之分。优先级的计算取决与具体的场景。

计算方式为：计算column-count和column-width转换后具体的列数，哪个小就用哪个。

- 存在的问题
1、前两列的最后一个元素的文本内容被自动断开，一部分在当前列尾，一部分在下一列的列头。
解决：
```css
.masonry .item{
    break-inside: avoid;
}
```
2、由于是从上到小、从左到右的布局，不适用于滚动加载。【似乎无解】

代码实例
- https://codesandbox.io/s/masonry-column-1-ddbhw?file=/src/main.js
- https://codepen.io/kujian/pen/GJZzBo


## 方法2：grid 布局实现瀑布流
[基础知识：grid布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout)

## 方法3: Flexbox 实现瀑布流

代码实例：
- https://codesandbox.io/s/masonry-flex-1-bigeg?file=/src/App.vue

### 资料

- [「中高级前端」干货！深度解析瀑布流布局](https://juejin.cn/post/6844904004720263176)

- [瀑布流的三种实现方案及优缺点](https://juejin.cn/post/7014650146000470053#heading-12)

### 示例：
- https://www.pinterest.com/

