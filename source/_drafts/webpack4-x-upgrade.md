---
title: webpack4.x升级
date: 2018-07-10 18:11:56
tags:
---
目前项目开发还在用webpack 2.x的版本，打包的太耗时，一个稍微大点的项目，在本地打包花费74.6秒，在jenkins的服务器打包花费132s，太耗时了，想着[webpack4.x](https://webpack.docschina.org)出来了，据说优化了很多。所以升级一波。

安装官网的指引，首先升级webpack依赖
``` bash
# webpack 目前最新版本到4.15.1了
yarn add -D webpack@4.15.1 webpack-cli
```

然后我build项目的时候，抱错了：
```
D:\workspace\work\angue\node_modules\webpack\lib\webpack.js:157
                        throw new RemovedPluginError(errorMessage);
                        ^

Error: webpack.optimize.UglifyJsPlugin has been removed, please use config.optimization.minimize instead.
    at Object.get [as UglifyJsPlugin] (D:\workspace\work\angue\node_modules\webpack\lib\webpack.js:157:10)
    at Object.<anonymous> (D:\workspace\work\angue\build\webpack.prod.conf.js:33:26)
    at Module._compile (internal/modules/cjs/loader.js:702:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:713:10)
    at Module.load (internal/modules/cjs/loader.js:612:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:551:12)
    at Function.Module._load (internal/modules/cjs/loader.js:543:3)
    at Module.require (internal/modules/cjs/loader.js:650:17)
    at require (internal/modules/cjs/helpers.js:20:18)
    at Object.<anonymous> (D:\workspace\work\angue\build\build.js:12:21)
    at Module._compile (internal/modules/cjs/loader.js:702:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:713:10)
    at Module.load (internal/modules/cjs/loader.js:612:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:551:12)
    at Function.Module._load (internal/modules/cjs/loader.js:543:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:744:10)
```

应该是UglifyJsPlugin这个插件不兼容了：
``` bash
yarn add -D uglifyjs-webpack-plugin
```
在webpack的uglifiy插件配置替换成：
```
new UglifyJsPlugin({
  warningsFilter: () => false,
  sourceMap: true
}),
```

继续build，报错：
```
D:\workspace\work\angue\node_modules\webpack\lib\webpack.js:169
                        throw new RemovedPluginError(errorMessage);
                        ^

Error: webpack.optimize.CommonsChunkPlugin has been removed, please use config.optimization.splitChunks instead.
    at Object.get [as CommonsChunkPlugin] (D:\workspace\work\angue\node_modules\webpack\lib\webpack.js:169:10)
    at Object.<anonymous> (D:\workspace\work\angue\build\webpack.prod.conf.js:75:26)
    at Module._compile (internal/modules/cjs/loader.js:702:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:713:10)
    at Module.load (internal/modules/cjs/loader.js:612:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:551:12)
    at Function.Module._load (internal/modules/cjs/loader.js:543:3)
    at Module.require (internal/modules/cjs/loader.js:650:17)
    at require (internal/modules/cjs/helpers.js:20:18)
    at Object.<anonymous> (D:\workspace\work\angue\build\build.js:12:21)
    at Module._compile (internal/modules/cjs/loader.js:702:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:713:10)
    at Module.load (internal/modules/cjs/loader.js:612:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:551:12)
    at Function.Module._load (internal/modules/cjs/loader.js:543:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:744:10)
```
CommonsChunkPlugin插件不兼容了，需要使用SplitChunksPlugin，去掉CommonsChunkPlugin相关的插件配置，给webpack增加配置：
``` json
optimization: {
  splitChunks: {
    chunks: 'all'
  }
}
```


```
- building for production...(node:25712) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
(node:25712) DeprecationWarning: Tapable.apply is deprecated. Call apply on the plugin directly instead
/ building for production...D:\workspace\work\angue\node_modules\html-webpack-plugin\lib\compiler.js:81
        var outputName = compilation.mainTemplate.applyPluginsWaterfall('asset-path', outputOptions.filename, {
                                                  ^

TypeError: compilation.mainTemplate.applyPluginsWaterfall is not a function
    at D:\workspace\work\angue\node_modules\html-webpack-plugin\lib\compiler.js:81:51
    at compile (D:\workspace\work\angue\node_modules\webpack\lib\Compiler.js:296:11)
    at hooks.afterCompile.callAsync.err (D:\workspace\work\angue\node_modules\webpack\lib\Compiler.js:553:14)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (D:\workspace\work\angue\node_modules\tapable\lib\HookCodeFactory.js:24:12), <anonymous>:6:1)
    at AsyncSeriesHook.lazyCompileHook [as _callAsync] (D:\workspace\work\angue\node_modules\tapable\lib\Hook.js:35:21)
    at compilation.seal.err (D:\workspace\work\angue\node_modules\webpack\lib\Compiler.js:550:30)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (D:\workspace\work\angue\node_modules\tapable\lib\HookCodeFactory.js:24:12), <anonymous>:6:1)
    at AsyncSeriesHook.lazyCompileHook [as _callAsync] (D:\workspace\work\angue\node_modules\tapable\lib\Hook.js:35:21)
    at hooks.optimizeAssets.callAsync.err (D:\workspace\work\angue\node_modules\webpack\lib\Compilation.js:1283:35)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (D:\workspace\work\angue\node_modules\tapable\lib\HookCodeFactory.js:24:12), <anonymous>:6:1)
    at AsyncSeriesHook.lazyCompileHook [as _callAsync] (D:\workspace\work\angue\node_modules\tapable\lib\Hook.js:35:21)
    at hooks.optimizeChunkAssets.callAsync.err (D:\workspace\work\angue\node_modules\webpack\lib\Compilation.js:1274:32)
    at _err1 (eval at create (D:\workspace\work\angue\node_modules\tapable\lib\HookCodeFactory.js:24:12), <anonymous>:16:1)
    at D:\workspace\work\angue\node_modules\uglifyjs-webpack-plugin\dist\index.js:282:11
    at _class.runTasks (D:\workspace\work\angue\node_modules\uglifyjs-webpack-plugin\dist\uglify\index.js:63:9)
    at UglifyJsPlugin.optimizeFn (D:\workspace\work\angue\node_modules\uglifyjs-webpack-plugin\dist\index.js:195:16)
```
这个表面上看不出什么头绪，google一波，发现是[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin/issues/841)的问题，升级一下
```
yarn add -D html-webpack-plugin@3.2.0
```
后面又出问题了，
```
- building for production...(node:27884) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
/ building for production...(node:27884) UnhandledPromiseRejectionWarning: Error: Chunk.entrypoints: Use Chunks.groupsIterable and filter by instanceof
Entrypoint instead
    at Chunk.get (D:\workspace\work\angue\node_modules\webpack\lib\Chunk.js:802:9)
    at D:\workspace\work\angue\node_modules\extract-text-webpack-plugin\index.js:260:40
```
升级[extract-text-webpack-plugin](https://github.com/webpack/webpack/issues/6568),
```
yarn add -D extract-text-webpack-plugin@4.0.0-beta.0
```

下一个问题，是一个警告：
```
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
```
给webpack的配置文件加上mode配置项就可以了
``` json
// mode: production | development
{
  mode: 'production'
}
```

接着一个eslint的问题：
```
Module build failed (from ./node_modules/eslint-loader/index.js):
TypeError: Cannot read property 'eslint' of undefined
    at Object.module.exports (D:\workspace\work\angue\node_modules\eslint-loader\index.js:148:18)
```
