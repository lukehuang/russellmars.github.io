---
title: 微信网页分享
date: 2018-10-12 09:58:22
tags: [微信, 分享]
---
在微信里做网页分享第一步要先在网页里引入jssdk

``` javascript
<script src="//res2.wx.qq.com/open/js/jweixin-1.4.0.js "></script>
```

在单页应用中，微信sdk的分享配置不同，说下两个概念
+ LandingPage 落地页面，刷新，a标签，location.href等方式直接到达的页面
+ CurrentPage 当前页面，当前链接所处的页面

首先说一下android和ios的微信版本对url的识别问题
android里无论怎么跳转当前页面的url都是CurrentPage的url
ios里跳转以后的url还是LandingPage的url

所以android里在每次切换路由的时候都要config一次，并配置分享
而ios里在只需要在一开始配置一次即可，后面直接配置分享的api

以vue router 的history模式为例
``` javascript
function configShare () {
  initWxJsSDK().then(() => {
    configWxShare({
      title: 'titlexxx',
      desc: 'descxxxxxxxx',
      link: window.location.href,
      imgUrl: 'xxxxxx'
    })
  }, (error) => {
    // console.log(error)
  })
}

router.afterEach(route => {
  if (!route.meta || !route.meta.needShare) {
    // 必须用setTimeout做一个异步的操作，因为afterEach钩子中的 url还没有变化，会导致config失败
    if (window.__wxjs_is_wkwebview) { // ios里
      configShare()
    } else {
      setTimeout(() => {
        configShare()
      }, 500)
    }
  }
})

let isConfiged = false
/**
 * 初始化微信JsSDK
 */
export const initWxJsSDK = () => {
  if (!isWeixin()) {
    return Promise.reject('非微信环境无法config')
  }
  if (isConfiged && is.ios()) {
    return Promise.resolve()
  }
  let url = window.location.href.split('#')[0]
  if (is.ios()) {
    url = global.landPageUrl.split('#')[0]
  }

  return api.wechat
    .getJsSdkSign({
      data: {
        url
      }
    })
    .then(result => {
      return new Promise((resolve, reject) => {
        if (result.responseCode === 0) {
          isConfiged = true
          configJsSDK(result)
          wx.ready(enhancer(configJsSDKSuccessHander, resolve))
          wx.error(enhancer(configJsSDKErrorHandler, reject))
        } else {
          reject('JsSDK 接口请求失败')
        }
      })
    })
}
```
以上是通用页面的分享配置，如果有些页面需要单独配置分享信息，则需要给这个路由的meta.needShare设为true
``` javascript
{
  path: '/product-info',
  component: () => import('@/views/moonMall/product/info'),
  meta: {
    needShare: true
  }
}
```
并在页面的mounted钩子里做配置
``` javascript
mounted () {
  this.fetchData().then(() => {
    this.configShare()
  })
}
```

## links
+ [微信JS-SDK说明文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)