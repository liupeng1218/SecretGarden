<!-- TOC -->

- [概念](#%e6%a6%82%e5%bf%b5)
- [参数配置](#%e5%8f%82%e6%95%b0%e9%85%8d%e7%bd%ae)
- [项目配置](#%e9%a1%b9%e7%9b%ae%e9%85%8d%e7%bd%ae)
- [开发环境](#%e5%bc%80%e5%8f%91%e7%8e%af%e5%a2%83)
- [区分环境](#%e5%8c%ba%e5%88%86%e7%8e%af%e5%a2%83)
- [性能优化](#%e6%80%a7%e8%83%bd%e4%bc%98%e5%8c%96)
  - [减少打包时间](#%e5%87%8f%e5%b0%91%e6%89%93%e5%8c%85%e6%97%b6%e9%97%b4)
    - [合理的配置 mode 参数与 devtool 参数](#%e5%90%88%e7%90%86%e7%9a%84%e9%85%8d%e7%bd%ae-mode-%e5%8f%82%e6%95%b0%e4%b8%8e-devtool-%e5%8f%82%e6%95%b0)
    - [缩小搜索范围](#%e7%bc%a9%e5%b0%8f%e6%90%9c%e7%b4%a2%e8%8c%83%e5%9b%b4)
    - [HappyPack](#happypack)
    - [webpack-parallel-uglify-plugin](#webpack-parallel-uglify-plugin)
    - [抽离第三方模块](#%e6%8a%bd%e7%a6%bb%e7%ac%ac%e4%b8%89%e6%96%b9%e6%a8%a1%e5%9d%97)
    - [配置缓存](#%e9%85%8d%e7%bd%ae%e7%bc%93%e5%ad%98)
  - [减少打包体积](#%e5%87%8f%e5%b0%91%e6%89%93%e5%8c%85%e4%bd%93%e7%a7%af)
    - [webpack-bundle-analyzer 分析打包文件](#webpack-bundle-analyzer-%e5%88%86%e6%9e%90%e6%89%93%e5%8c%85%e6%96%87%e4%bb%b6)
    - [externals](#externals)
    - [Tree-shaking](#tree-shaking)
    - [按需加载](#%e6%8c%89%e9%9c%80%e5%8a%a0%e8%bd%bd)

<!-- /TOC -->

# 概念

本质上，`webpack` 是一个现代 `JavaScript` 应用程序的静态模块打包器(`module bundler`)。当 `webpack` 处理应用程序时，它会递归地构建一个依赖关系图(`dependency graph`)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 `bundle`。

1. 入口起点(`entry point`)指示 `webpack` 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，`webpack` 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
2. `loader` 可以将所有类型的文件转换为 `webpack` 能够处理的有效模块，然后你就可以利用 `webpack` 的打包能力，对它们进行处理。
3. `loader` 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。
4. `output` 属性告诉 `webpack` 在哪里输出它所创建的 `bundles`，以及如何命名这些文件，默认值为 `./dist`。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中

**[webpack](https://github.com/liupeng1218/SecretGardenDemo/tree/master/m-webpack-cli)**

# 参数配置

webpack 开箱即用，可以无需使用任何配置文件。然而，webpack 会假定项目的入口起点为 src/index，然后会在 dist/main.js 输出结果，并且在生产环境开启压缩和优化。

1. mode：告知 `webpack` 使用相应模式的内置优化，可以设置为 `none` ， `development` ， `production`，默认值是 `production`，对应内置优化为[mode](https://webpack.js.org/configuration/mode/#root)
2. entry：起点或是应用程序的起点入口。从这个起点开始，应用程序启动执行。如果传递一个数组，那么数组的每一项都会执行。
   1. 单页应用(SPA)：一个入口起点，多页应用(MPA)：多个入口起点。
   2. 命名：如果传入一个字符串或字符串数组，`chunk` 会被命名为 `main`。如果传入一个对象，则每个键(key)会是 `chunk` 的名称，该值描述了 `chunk` 的入口起点。
3. output： 位于对象最顶级键(key)，包括了一组选项，指示 `webpack` 如何去输出、以及在哪里输出
   1. path：output 目录对应一个**绝对路径**
   2. filename：此选项决定了每个输出 `bundle` 的名称。这些 `bundle` 将写入到 `output.path` 选项指定的目录下。注意此选项被称为文件名，但是你还是可以使用像 `js/[name]/bundle.js` 这样的**文件夹结构**。
   3. | 模板        | 描述                                             |
      | ----------- | ------------------------------------------------ |
      | [hash]      | 模块标识符(module identifier)的 hash             |
      | [chunkhash] | chunk 内容的 hash                                |
      | [name]      | 模块名称                                         |
      | [id]        | 模块标识符(module identifier)                    |
      | [ext]       | 模块类型                                         |
      | [query]     | 模块的 query，例如，文件名 ? 后面的字符串        |
      | [function]  | The function, which can return filename [string] |
   4. publicPath：该选项的值是以 `runtime`(运行时) 或 `loader`(载入时) 所创建的每个 `URL` 为前缀。因此，在多数情况下，此选项的值都会以 `/` 结束。
   5. output.chunkFilename：此选项决定了非入口(non-entry) chunk 文件的名称，命名方式参考 filename
4. module：用于对模块的源代码进行转换。`loader` 可以使你在 `import` 或"加载"模块时预处理文件。因此，`loader` 类似于其他构建工具中“任务(task)”，在`module.rules`中配置`loader`
   1. test ，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
   2. use ，表示进行转换时，应该使用哪个 loader。多个 user 时，处理顺序为从后向前
   3. include：指定解析范围
   4. exclude：排除解析目录
   5. noParse：排除无需解析的特定库
5. resolve：处理对应的解析规则
   1. alias：别名，确保模块引入变得更简单快捷。`alias:{' @':path.resolve(__dirname,'../src')},`
   2. extensions：自动解析确定的扩展，默认为`[".js", ".json"]`
6. devtool：此选项控制是否生成，以及如何生成 `source map`。开发环境使用`cheap-module-eval-source-map`，生产环境
7. optimization：`webpack` 会根据当前`mode`提供默认的内置优化，也可以在`optimization`手动优化配置
   1. splitChunks：如何对通用的块进行提取打包[splitChunks](https://webpack.js.org/plugins/split-chunks-plugin/)
   2. minimizer：对打包文件最小化处理，在这个声明 js，css 等压缩插件

# 项目配置

在项目中，需要很多插件等配合参数配置共同实现开发需求

1. html 模板：每次打包的文件都会有响应的 hash 等变动，不需要每次手动修改，配合`html-webpack-plugin`插件实现引入资源的自动更新，参考文档[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin#configuration)
   1. 多页面时，需要配置`chunks`参数配合入口文件的对应的模块名实现文件的匹配
2. 清空打包文件：每次执行`npm run build`会发现`dist`文件夹里会残留上次打包的文件，`clean-webpack-plugin`可以在打包输出前清空文件夹
3. bable 转义：bable 帮助 js 实现更好的兼容性，`babel-loader` 实现 loader 处理 ，`@babel/core` 处理 js 转义，`@babel/preset-env` 配置转换规则
4. es6+polyfill：bable 只能转义语法，新的 api 需要`babel-polyfill`进行转换
5. 打包 css：css 需要特定的 `loader` 来进行处理，`style-loader`，`css-loader`，如果使用了预处理器，需要下载对应的处理器和 loader
6. css 前缀处理：`postcss-loader`配合`autoprefixer`可以自动为样式添加兼容前缀
   1. 使用`postcss-loader`的同时需要在插件或者直接在 loader 配置中引入`autoprefixer`
7. 拆分 css：默认情况下，css 会通过 style 标签的形式插入到 html 中，如果想使用 css 文件引入的方式，可以通过`mini-css-extract-plugin`插件
8. 其他资源：打包 图片、字体、媒体、等
   1. `file-loader`可以将文件在进行一些处理后（主要是处理文件名和路径、解析文件 url），并将文件移动到输出的目录中。
   2. `url-loader` 一般与`file-loader`搭配使用，功能与 `file-loader` 类似，如果文件小于限制的大小。则会返回 `base64` 编码，否则使用 `file-loader` 将文件移动到输出的目录中
9. 解析 vue：`vue-loader`解析 vue 文件（**需配合 VueLoaderPlugin 插件一起使用**）， `vue-template-compiler`用于编译模板（也是 vue-loader 的前置依赖）， `vue-style-loader`配置在解析对应的样式处理 loader 中
10. 拷贝静态文件：使用`copy-webpack-plugin`插件拷贝不需要打包的静态文件
11. 压缩 css：使用`optimize-css-assets-webpack-plugin` 压缩 css
12. 压缩 js：使用`uglifyjs-webpack-plugin` 压缩 js。`webpack mode` 设置 `production` 的时候会自动压缩 js 代码。原则上不需要引入 `uglifyjs-webpack-plugin` 进行重复工作。但是 `optimize-css-assets-webpack-plugin` 压缩 `css` 的同时会破坏原有的 `js` 压缩，所以这里我们引入 `uglifyjs` 进行压缩
13.

# 开发环境

webpack 提供了一套完整的前端开发环境， `webpack-dev-server` 能够用于快速开发应用程序，配置在 `devServer` 属性中

1.  contentBase：告诉服务器从哪里提供内容。只有在你想要提供静态文件时才需要
2.  port：端口号
3.  hot：启动热模块更新，需要 `Webpack.HotModuleReplacementPlugin` 插件配合
4.  compress：启动 gzip 压缩
5.  headers：向所有响应添加头部内容
6.  host：指定 host
7.  open：启动服务自动打开浏览器
8.  proxy：请求代理转发，使用[http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)
9.  publicPath：此路径下的打包文件可在浏览器中访问

# 区分环境

- 开发环境：开发环境主要实现的是热更新,不要压缩代码，完整的 `sourceMap`
- 生产环境：生产环境主要实现的是压缩代码、提取 css 文件、合理的 `sourceMap`、分割代码

可以利用一些模块来对 `webpack` 的配置进行合并，分割

- `webpack-merge` 合并配置
- `copy-webpack-plugin` 拷贝静态资源

# 性能优化

## 减少打包时间

### 合理的配置 mode 参数与 devtool 参数

`mode` 可设置 `development` `production` 两个参数
如果没有设置，`webpack4` 会将 `mode` 的默认值设置为 `production`
`production` 模式下会进行 `tree shaking`(去除无用代码)和 `uglifyjs`(代码压缩混淆)

### 缩小搜索范围

- alias: 当我们代码中出现 `import 'vue'`时， `webpack` 会采用向上递归搜索的方式去 `node_modules` 目录下找。为了减少搜索范围我们可以直接告诉 `webpack` 去哪个路径下查找。也就是别名(alias)的配置。
- include exclude： 同样配置 `include` `exclude` 也可以减少 `webpack loader` 的搜索转换时间。
- noParse： 当我们代码中使用到 `import jq from 'jquery'`时，`webpack` 会去解析 `jq` 这个库是否有依赖其他的包。但是我们对类似 `jquery` 这类依赖库，一般会认为不会引用其他的包(特殊除外,自行判断)。增加 `noParse` 属性,告诉 `webpack` 不必解析，以此增加打包速度。
- extensions： `webpack` 会根据 `extensions` 定义的后缀查找文件(频率较高的文件类型优先写在前面)

### HappyPack

受限于 Node 是单线程运行的，所以 Webpack 在打包的过程中也是单线程的，特别是在执行 Loader 的时候，长时间编译的任务很多，这样就会导致等待的情况。

HappyPack 可以将 Loader 的同步执行转换为并行的，这样就能充分利用系统资源来加快打包效率了

```JS
module: {
  loaders: [
    {
      test: /\.js$/,
      include: [resolve('src')],
      exclude: /node_modules/,
      // id 后面的内容对应下面
      loader: 'happypack/loader?id=happybabel'
    }
  ]
},
plugins: [
  new HappyPack({
    id: 'happybabel',
    loaders: ['babel-loader?cacheDirectory'],
    // 开启 4 个线程
    threads: 4
  })
]
```

### webpack-parallel-uglify-plugin

使用 `webpack-parallel-uglify-plugin` 使用多进程压缩代码

### 抽离第三方模块

对于开发项目中不经常会变更的静态依赖文件。类似于我们的`elementUi`、`vue`全家桶等等。因为很少会变更，所以我们不希望这些依赖要被集成到每一次的构建逻辑中去。 这样做的好处是每次更改我本地代码的文件的时候，`webpack`只需要打包我项目本身的文件代码，而不会再去编译第三方库。以后只要我们不升级第三方包的时候，那么`webpack`就不会对这些库去打包，这样可以快速的提高打包的速度。

使用 `webpack` 内置的 `DllPlugin DllReferencePlugin` 进行抽离

```JS
// webpack.dll.config.js
const path = require("path");
const webpack = require("webpack");
module.exports = {
  // 你想要打包的模块的数组
  entry: {
    vendor: ['vue','element-ui']
  },
  output: {
    path: path.resolve(__dirname, 'static/js'), // 打包后文件输出的位置
    filename: '[name].dll.js',
    library: '[name]_library'
     // 这里需要和webpack.DllPlugin中的`name: '[name]_library',`保持一致。
  },
  plugins: [
    new webpack.DllPlugin({
      path: path.resolve(__dirname, '[name]-manifest.json'),
      name: '[name]_library',
      context: __dirname
    })
  ]
};

// webpack.config.js
module.exports = {
  plugins: [
    new webpack.DllReferencePlugin({
      context: __dirname,
      manifest: require('./vendor-manifest.json')
    }),
    new CopyWebpackPlugin([ // 拷贝生成的文件到dist目录 这样每次不必手动去cv
      {from: 'static', to:'static'}
    ]),
  ]
};

```

### 配置缓存

每次构建都会把所有的文件重复编译一遍，这样重复的工作可以被缓存下来，方便下次编译时使用。

大部分 `loader` 都提供 `cache` 选项开始缓存

但如果 `loader` 不支持缓存可以通过 `cache-loader` ，将 `loader` 的编译结果写入硬盘缓存。再次构建会先比较一下，如果文件较之前的没有发生变化则会直接使用缓存。在一些性能开销较大的 `loader` 之前添加此 `loader` 即可

## 减少打包体积

### webpack-bundle-analyzer 分析打包文件

`webpack-bundle-analyzer` 将打包后的内容束展示为方便交互的直观树状图，让我们知道我们所构建包中真正引入的内容

### externals

如果我们想引用一个库，但是又不想让 `webpack` 打包，并且又不影响我们在程序中以`CMD`、`AMD`或者`window/global`全局等方式进行使用，那就可以通过配置 `Externals`

```HTML
<script
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous">
</script>

```

```js
module.exports = {
  //...
  externals: {
    jquery: 'jQuery'
  }
}
```

### Tree-shaking

`tree-shaking`的主要作用是用来清除代码中无用的部分。在 `webpack4` 我们设置`mode`为`production`的时候已经自动开启了`tree-shaking`。但是要想使其生效，生成的代码必须是`ES6`模块。不能使用其它类型的模块如`CommonJS`之流。如果使用`Babel`的话，这里有一个小问题，因为`Babel`的预案（preset）默认会将任何模块类型都转译成`CommonJS`类型，这样会导致`tree-shaking`失效。修正这个问题也很简单，在`.babelrc`文件或在`webpack.config.js`文件中设置`modules： false`就好了

```JS
// webpack.config.js

module: {
    rules: [
        {
            test: /\.js$/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['@babel/preset-env', { modules: false }]
                }
            }，
            exclude: /(node_modules)/
        }
    ]
}

```

### 按需加载

想必大家在开发 SPA 项目的时候，项目中都会存在十几甚至更多的路由页面。如果我们将这些页面全部打包进一个 JS 文件的话，虽然将多个请求合并了，但是同样也加载了很多并不需要的代码，耗费了更长的时间。那么为了首页能更快地呈现给用户，我们肯定是希望首页能加载的文件体积越小越好，这时候我们就可以使用按需加载，将每个路由页面单独打包为一个文件。当然不仅仅路由可以按需加载，对于 loadash 这种大型类库同样可以使用这个功能。

按需加载的代码实现这里就不详细展开了，因为鉴于用的框架不同，实现起来都是不一样的。当然了，虽然他们的用法可能不同，但是底层的机制都是一样的。都是当使用的时候再去下载对应文件，返回一个 Promise，当 Promise 成功以后去执行回调。


