---
title: 插件(plugins)
sort: 8
contributors:
  - sokra
  - skipjack
  - yatharthk
---

`plugins` 选项用于以各种方式自定义 webpack 构建过程。webpack 附带了各种内置插件，可以通过 `webpack.[plugin-name]` 访问这些插件。请查看[这个页面](/plugins)获取插件列表和对应文档，但请注意这只是其中一部分，社区中还有许多插件。

T> 注意：本页面仅讨论使用插件，如果你有兴趣编写自己的插件，请访问[编写一个插件](/development/how-to-write-a-plugin/)页面。


## `plugins`

一组webpack插件。例如，DefinePlugin允许您创建可以在编译时配置的全局常量。这对于允许开发构建和发布构建之间的不同行为非常有用。

```js
plugins: [
  new webpack.optimize.CommonsChunkPlugin({
    ...
  })
]
```

一个复杂示例，使用多个插件，可能看起来就像这样：

```js
var webpack = require('webpack');
// 导入非 webpack 自带默认插件
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var DashboardPlugin = require('webpack-dashboard/plugin');

// 在配置中添加插件
plugins: [
  // 构建优化插件
  new webpack.optimize.CommonsChunkPlugin({
    name: 'vendor',
    filename: 'vendor-[hash].min.js',
  }),
  new webpack.optimize.UglifyJsPlugin({
    compress: {
      warnings: false,
      drop_console: false,
    }
  }),
  new ExtractTextPlugin({
    filename: 'build.min.css',
    allChunks: true,
  }),
  new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/),
  // 编译时(compile time)插件
  new webpack.DefinePlugin({
    'process.env.NODE_ENV': '"production"',
  }),
  // webpack-dev-server 强化插件
  new DashboardPlugin(),
  new webpack.HotModuleReplacementPlugin(),
]
```
