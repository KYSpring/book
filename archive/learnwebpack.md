# webpack笔记

- 管理输出：
  manifest: 描述bundle和映射关系；webpack 通过 manifest，可以追踪所有模块到输出 bundle 之间的映射。
  HtmlWebpackPlugin：生成html文件，自动插入依赖bundle

- 代码分离
  1）代码分离是 webpack 中最引人注目的特性之一；此特性能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。
  2）代码分离的实现方式：
    入口起点：使用 entry 配置手动地分离代码。 
    防止重复：使用 Entry dependencies 或者 SplitChunksPlugin 去重和分离 chunk。 
    动态导入：通过模块的内联函数调用来分离代码。
> 配合vue-router路由懒加载

- 配置文件结构：
```md
    - mode 
    - stats 
    TODO
```

- split chunk用法
  [webpck split chunk官方文档](https://webpack.js.org/plugins/split-chunks-plugin/)
  vendors从何而来
  TODO

- 

