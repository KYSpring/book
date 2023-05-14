# Nuxt

## 初体验

```javascript
npx nuxi init <project-name>
```
执行nuxt之后init项目之中，再执行```yarn dev -o```之后即会启动nuxt项目，页面app.vue中代码非常简单，如下：

```javascript
<template>
  <div>
    <NuxtWelcome />
  </div>
</template>
```

其中有点困惑的点是：``` <NuxtWelcome />```组件的具体实现逻辑被隐藏了，看不出怎么实现的。
从components.d.ts中的内容来看，该组件被打包内置在了node_module/@nuxi/ui-template中，并且其中还有很多别的内置组件，用来干啥的：

```javascript
    'NuxtWelcome': typeof import("../node_modules/@nuxt/ui-templates/dist/templates/welcome.vue")['default']
    'NuxtLayout': typeof import("../node_modules/nuxt/dist/app/components/layout")['default']
    'NuxtErrorBoundary': typeof import("../node_modules/nuxt/dist/app/components/nuxt-error-boundary")['default']
    'ClientOnly': typeof import("../node_modules/nuxt/dist/app/components/client-only")['default']
    'DevOnly': typeof import("../node_modules/nuxt/dist/app/components/dev-only")['default']
    'ServerPlaceholder': typeof import("../node_modules/nuxt/dist/app/components/server-placeholder")['default']
    'NuxtLink': typeof import("../node_modules/nuxt/dist/app/components/nuxt-link")['default']
    'NuxtLoadingIndicator': typeof import("../node_modules/nuxt/dist/app/components/nuxt-loading-indicator")['default']
    'NuxtPage': typeof import("../node_modules/nuxt/dist/pages/runtime/page-placeholder")['default']
    'NoScript': typeof import("../node_modules/nuxt/dist/head/runtime/components")['NoScript']
```

## 项目配置

### Nuxt Configuration 

项目初始化后会自带一个配置文件：nuxt.config.ts，有什么作用？

- 环境变量和私钥 Environment Variables and Private Tokens

配置方式：

1）注册：
```javascript
 runtimeConfig: {
    // The private keys which are only available server-side
    apiSecret: '123',
    // Keys within public are also exposed client-side
    public: {
      apiBase: '/api'
    }
  }
```
2）使用：
```javascript
const runtimeConfig = useRuntimeConfig()
```

【疑问】干啥用的？答：环境隔离，主要用于隔离服务端和移动端的配置项Keys

### App Configuration

app.config.ts文件，主要用来配置一些非敏感信息。用法和nuxt config差不多：

```javascript
export default defineAppConfig({
  title: 'Hello Nuxt',
  theme: {
    dark: true,
    colors: {
      primary: '#ff0000'
    }
  }
})
```
```
```



### runtimeConfig vs app.config

app.config也是一种文件配置形式，和runtimeConfig的区别其实官方文档也很明确了:

> runtimeConfig: Private or public tokens that need to be specified after build using environment variables.
> 
> app.config : Public tokens that are determined at build time, website configuration such as theme variant, title and any project config that are not sensitive. Contrary to the runtimeConfig option, these can not be overridden using environment variables.