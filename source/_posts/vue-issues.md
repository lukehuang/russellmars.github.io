---
title: vue 开发遇到的问题
date: 2018-06-29 10:27:16
tags: [vue, web]
---
对做vue相关开发时遇到的一些问题的总结
### vue

### vuex

### vue router

### nuxt

#### 同一个页面路由变化时没有触发asyncData
需要给page组件提供key和watchQuery属性，如
``` es6
export default {
  watchQuery: ['code'],
  key: (to) => to.fullPath,
  async asyncData ({query, store, error}) {
    // do something when code is changed
    return {
      code: query.code
    }
  }
}
```
参考[New asyncData is not applied when query is changed](https://github.com/nuxt/nuxt.js/issues/2591)

### other

#### node-sass 安装不上
需要安装[windows-build-tools](https://github.com/felixrieseberg/windows-build-tools)