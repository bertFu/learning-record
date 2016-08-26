# atool-build学习笔记：初识

原文地址：[atool-build](http://ant-tool.github.io/atool-build.html)

`atool-build` 在刚接触react，还不知道 `webpack` 的时候，看到他一脸懵逼。

滚了两个月回来看看他，其实就是 `webpack` 的二次封装，将一些常用的配置方案集成起来，便于使用。

## 安装

```
$ npm install -g atool-build
```

## 配置 package.json

atool-build 要求 package.json 文件里面增加 entry 字段。

> 其实就是指定需要操作的文件，与`webpack` 中的 `entry` 配置功能相同。

```json
"entry": {
    "index": "./src/pathToYourEntry.jsx",
    "another": "./src/anotherEntry.jsx"
}
```

## 执行构建

```
$ atool-build
```

执行 `atool-build` 操作相当于执行 `webpack --config webpack.config.js`

> 会将 `package` 中 `entry` 中指定的文件进行压缩混淆，针对每个文件会按文件名生成对应的 `文件名.js`文件与 `文件名.css` 文件，并且会生成 `common.js` 和 `common.css`

## 参数

- `-o, --output-path<path>` 指定构建后的输出路径

  - 默认是指定 `dist` 文件夹, `atool-build -o aaa` 设置后，会构建到 `aaa` 文件夹下

- `w, --watch [delpay]` 是否监控文件变化，默认为不监控。

  - 默认构建一次就结束命名，开启监控 `atool-build -w` 后命令行会保持构建状态，并且在每次修改保持后会按最小差异再次构建文件。
  - 和热更新类似，我觉得热跟新就是在监控完成自动构建后，在帮我们做一次页面的更新，想想就觉得好牛逼。

- `--no-compress` 不压缩代码。

  - 顾名思义，按文件导入状态整合代码，不做压缩处理。

- `--config [userConfigFile]` 指定用户配置文件。默认为根目录下的 webpack.config.js 文件。这个配置文件不是必须的。

  - `webpack` 指定配置文件: `webpack --config webpack.dev.config.js`，同理， `atool-build --config webpack.dev.config.js` 达到同样的效果。

- `--devtool <devtool>` 生成 sourcemap 的方法，默认为空，这个参数和 webpack 的配置一致。

  - 设置不同的构建模式， `webpack` 可以通过设置json配置的 `sourcemap` 属性来控制构建模式， `atool-build` 通过 `atool-build --devtool source-map` 来设置内部的 `webpack.config.js` 文件来达到此效果。

- `--hash` 使用 hash 模式的构建, 并生成映射表 map.json。

  - 通过该命令构建出来的文件会在末尾加上 `-hash值` 可以来判断构建出来的 `hash` 按需处理构建文件。

# webpack.config.js 配置举例

## 使用

> 对于一些有自定义需求的项目，要是希望进一步做一些配置，按照下面的流程：

> 1. 运行命令 `$ atool-build` ;
> 2. 检测当前文件夹是否有 `webpack.config.js` 文件;
> 3. 要是没有则采用 `atool-build` 内置的 `webpack` 配置，要是有这个文件，则将内置的 `webpack` 配置对象传入进行处理，返回的对象作为实际的 `webpack` 对象。

也就是说，我们可以通过 webpack.config.js 将 atool-build 内置的 webpack 配置进一步进行处理（添加一些 loader，plugin 等），得到项目实际需要的效果。webpack.config.js 里有一个函数，会将内置的 webpack 配置对象作为参数，而这个函数的返回值会作为新的 webpack 配置对象。所以，我们只需要改变这个对象就可以完成个性化配置。比如，我们添加一个 DefinePlugin 来处理项目里面的预留值：

webpack.config.js

```javascript
// 得到 webpack
var webpack = require('atool-build/lib/webpack');

module.exports = function(webpackConfig) {
  // 添加一个plugin
  webpackConfig.plugins.push(
    new webpack.DefinePlugin({
      __DEV__: JSON.stringify('true')
    });
  );
  // 返回 webpack 配置对象
  return webpackConfig;
};
```

## 内置对象

atool-build 内置了一个 webpack 配置对象，来满足一般项目的需求，主要功能为：

### entry

package.json 中 entry 对应内容

### output

默认为 dist 文件夹，每个 entry key 对应 key.js, common.js, key.css, common.css(如果没有样式文件则没有 css 文件)。比如：

```json
{
  "entry":{
    "index":"./src/index.js"
  }
}
```

./dist：index.js, common.js, index.css, common.css

### devtool

通过 atool-build --devtool devtool 得到，默认为 undefined

### resolve

```javascript
{
  modulesDirectories: ['node_modules', path.join(__dirname, '../node_modules')],
  extensions: ['', '.js', '.jsx'],
}
```

模块解析配置项，`extensions` 解析的后缀

### resolveLoader

```javascript
{
  modulesDirectories: ['node_modules', path.join(__dirname, '../node_modules')],
}
```

### node

如果在 `package.json` 中 `browser` 没有设置，则设置 `child_process, cluster, dgram, dns, fs, module, net, readline, repl, tls` 为 `empty`

### loader

- babel 处理 .js，.jsx 文件

  ```javascript
  {
  presets: ['es2015', 'react', 'stage-0'],
  plugins: ['add-module-exports', 'typecheck'],
  }
  ```

- css：处理 .css 文件

- autoprefixer：给 css 属性设置浏览器前缀

- less：处理 .less 文件，支持 less

- url：处理 .woff, .woff2, .ttf, .svg, png, jpg, jpeg, gif 文件

- file：处理 .eot 文件

- json：处理 .json 文件

### plugins

- `webpack.optimize.CommonsChunkPlugin`: (打包成多个资源文件，可以有效利用缓存提升性能，做到文件按需加载)
- `extract-text-webpack-plugin`：（独立出css样式，如果我们希望样式通过 `<link>` 引入，而不是放在 `<style>` 标签内）
- `webpack.optimize.OccurenceOrderPlugin`: (按照引用程度来排序各个模块，引用的越频繁id就越短，达到减小文件大小的效果)

更多内容可见 [getWebpackCommonConfig](https://github.com/ant-tool/atool-build/blob/master/src/getWebpackCommonConfig.js)

## 注意点

- 对于自定义添加的 loader，plugin等依赖，需要在项目文件夹中npm install 这些依赖。但不需要再安装 webpack 依赖，因为可以通过 require('atool-build/lib/webpack') 得到；
- 可以通过 atool-build --config file 来指定需要用到的配置文件，默认是 webpack.config.js，要是找不到则使用内置的 webpack 配置。

上面涉及到的 `webpack` 的具体作用，基本闷逼状态，日后深入学习回来补上...囧 我估计可能很难补上了，到时候在写一篇 `webpack` 的学习笔记 ^_^
