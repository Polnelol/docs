---
title: "API: router 属性配置"
description: router 属性让你可以个性化配置 Nuxt.js 应用的路由。
---

# router 属性配置

> router 属性让你可以个性化配置 Nuxt.js 应用的路由（[vue-router](https://router.vuejs.org/zh-cn/)）。

## base

- 类型： `String`
- 默认值： `'/'`

应用的根URL。举个例子，如果整个单页面应用的所有资源可以通过 `/app/` 来访问，那么 `base` 配置项的值需要设置为 `'/app/'`。

例如 (`nuxt.config.js`)：
```js
module.exports = {
  router: {
    base: '/app/'
  }
}
```

<div class="Alert Alert-blue">

`base` 被设置后，Nuxt.js 会自动将它添加至页面中： `<base href="{{ router.base }}"/>`。

</div>

> 该配置项的值会被直接传给 vue-router 的[构造器](https://router.vuejs.org/zh-cn/api/options.html)。

## extendRoutes

- 类型: `Function`

您可能希望扩展`Nuxt.js`创建的路由。您可以通过`extendRoutes`选项执行此操作。

例如添加自定义路由:

`nuxt.config.js`
```js
export default {
  router: {
    extendRoutes (routes, resolve) {
      routes.push({
        name: 'custom',
        path: '*',
        component: resolve(__dirname, 'pages/404.vue')
      })
    }
  }
}
```

路由的模式应该遵循[vue-router](https://router.vuejs.org/en/)模式。

## linkActiveClass

- 类型： `String`
- 默认值： `'nuxt-link-active'`

全局配置 [`<nuxt-link>`](/api/components-nuxt-link) 组件默认的激活类名。

例如 (`nuxt.config.js`)：
```js
module.exports = {
  router: {
    linkActiveClass: 'active-link'
  }
}
```

> 该配置项的值会被直接传给 vue-router 的[构造器](https://router.vuejs.org/zh-cn/api/options.html)。

## linkExactActiveClass

- 类型: `String`
- 默认: `'nuxt-link-exact-active'`

全局配置 [`<nuxt-link>`](/api/components-nuxt-link) 默认的active class。

例如 (`nuxt.config.js`):
```js
export default {
  router: {
    linkExactActiveClass: 'exact-active-link'
  }
}
```

> 此选项直接提供给vue-router [linkexactactiveclass](https://router.vuejs.org/api/#linkexactactiveclass).

## middleware

- 类型： `String` 或 `Array`
  - 数值元素类型: `String`

为应用的每个页面设置默认的中间件。

例如：

`nuxt.config.js`
```js
module.exports = {
  router: {
    // 在每页渲染前运行 middleware/user-agent.js 中间件的逻辑
    middleware: 'user-agent'
  }
}
```

`middleware/user-agent.js`
```js
export default function (context) {
  // 给上下文对象增加 userAgent 属性（增加的属性可在 `asyncData` 和 `fetch` 方法中获取）
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```

了解更多关于中间件的信息，请参考 [中间件指引文档](/guide/routing#中间件)。

## mode

- 类型：`String`
- 默认值：`'history'`

配置路由的模式，鉴于服务端渲染的特性，不建议修改该配置。

示例 (`nuxt.config.js`):
```js
module.exports = {
  router: {
    mode: 'hash'
  }
}
```

> 该配置项的值会被直接传给 vue-router 的[构造器](https://router.vuejs.org/zh-cn/api/options.html)。

## scrollBehavior

- 类型： `Function`

`scrollBehavior` 配置项用于个性化配置跳转至目标页面后的页面滚动位置。每次页面渲染后都会调用 `scrollBehavior` 配置的方法。

`scrollBehavior` 的默认配置为：
```js
const scrollBehavior = (to, from, savedPosition) => {
  // savedPosition 只有在 popstate 导航（如按浏览器的返回按钮）时可以获取。
  if (savedPosition) {
    return savedPosition
  } else {
    let position = {}
    // 目标页面子组件少于两个
    if (to.matched.length < 2) {
      // 滚动至页面顶部
      position = { x: 0, y: 0 }
    }
    else if (to.matched.some((r) => r.components.default.options.scrollToTop)) {
      // 如果目标页面子组件中存在配置了scrollToTop为true
      position = { x: 0, y: 0 }
    }
    // 如果目标页面的url有锚点,  则滚动至锚点所在的位置
    if (to.hash) {
      position = { selector: to.hash }
    }
    return position
  }
}
```

举个例子，我们可以配置所有页面渲染后滚动至顶部：

`nuxt.config.js`：
```js
module.exports = {
  router: {
    scrollBehavior: function (to, from, savedPosition) {
      return { x: 0, y: 0 }
    }
  }
}
```

> 该配置项的值会被直接传给 vue-router 的[构造器](https://router.vuejs.org/zh-cn/api/options.html)。

## parseQuery / stringifyQuery

- 类型: `Function`

提供自定义查询字符串解析/字符串化功能。覆盖默认值。

> 此选项直接提供在vue-router [parseQuery / stringifyQuery](https://router.vuejs.org/api/#parsequery-stringifyquery).

## fallback

- 类型: `boolean`
- 默认: `false`

当浏览器不支持history.pushState但模式设置为history时，控制路由器是否应回退。

将此设置为`false`实质上会使每个路由器链接导航在IE9中刷新整页。当应用程序是服务器呈现并且需要在IE9中工作时，这很有用，因为**hash模式**URL不适用于SSR。

> 此选项直接提供在vue-router [fallback](https://router.vuejs.org/api/#fallback).
