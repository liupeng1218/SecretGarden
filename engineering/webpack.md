<!-- TOC -->

- [概念](#概念)
- [参数配置](#参数配置)
- [项目配置](#项目配置)
- [性能优化](#性能优化)
  - [减少打包时间](#减少打包时间)
    - [优化 Loader](#优化-loader)
    - [HappyPack](#happypack)
    - [其他](#其他)
  - [减少打包体积](#减少打包体积)
    - [按需加载](#按需加载)

<!-- /TOC -->

# 概念

本质上，`webpack` 是一个现代 `JavaScript` 应用程序的静态模块打包器(`module bundler`)。当 `webpack` 处理应用程序时，它会递归地构建一个依赖关系图(`dependency graph`)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 `bundle`。

1. 入口起点(`entry point`)指示 `webpack` 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，`webpack` 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
2. `loader` 可以将所有类型的文件转换为 `webpack` 能够处理的有效模块，然后你就可以利用 `webpack` 的打包能力，对它们进行处理。
3. `loader` 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。
4. `output` 属性告诉 `webpack` 在哪里输出它所创建的 `bundles`，以及如何命名这些文件，默认值为 `./dist`。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中

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
4. `loader` 用于对模块的源代码进行转换。`loader` 可以使你在 `import` 或"加载"模块时预处理文件。因此，`loader` 类似于其他构建工具中“任务(task)”，在`module.rules`中配置`loader`
   1. test ，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
   2. use ，表示进行转换时，应该使用哪个 loader。多个 user 时，处理顺序为从后向前

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

# 性能优化

## 减少打包时间

### 优化 Loader

对于 Loader 来说，影响打包效率首当其冲必属 Babel 了。因为 Babel 会将代码转为字符串生成 AST，然后对 AST 继续进行转变最后再生成新的代码，项目越大，转换代码越多，效率就越低。当然了，我们是有办法优化的。

首先我们可以优化 Loader 的文件搜索范围

```JS
module.exports = {
  module: {
    rules: [
      {
        // js 文件才使用 babel
        test: /\.js$/,
        loader: 'babel-loader',
        // 只在 src 文件夹下查找
        include: [resolve('src')],
        // 不会去查找的路径
        exclude: /node_modules/
      }
    ]
  }
}
```

对于 Babel 来说，我们肯定是希望只作用在 JS 代码上的，然后 node_modules 中使用的代码都是编译过的，所以我们也完全没有必要再去处理一遍。

当然这样做还不够，我们还可以将 Babel 编译过的文件缓存起来，下次只需要编译更改过的代码文件即可，这样可以大幅度加快打包时间

```JS
loader: 'babel-loader?cacheDirectory=true'
```

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

### 其他

- `resolve.extensions`：用来表明文件后缀列表，默认查找顺序是 ['.js', '.json']，如果你的导入文件没有添加后缀就会按照这个顺序查找文件。我们应该尽可能减少后缀列表长度，然后将出现频率高的+ 排在前面
- `resolve.alias`：可以通过别名的方式来映射一个路径，能让 Webpack 更快找到路径
- `module.noParse`：如果你确定一个文件下没有其他依赖，就可以使用该属性让 Webpack 不扫描该文件，这种方式对于大型的类库很有帮助

## 减少打包体积

### 按需加载

想必大家在开发 SPA 项目的时候，项目中都会存在十几甚至更多的路由页面。如果我们将这些页面全部打包进一个 JS 文件的话，虽然将多个请求合并了，但是同样也加载了很多并不需要的代码，耗费了更长的时间。那么为了首页能更快地呈现给用户，我们肯定是希望首页能加载的文件体积越小越好，这时候我们就可以使用按需加载，将每个路由页面单独打包为一个文件。当然不仅仅路由可以按需加载，对于 loadash 这种大型类库同样可以使用这个功能。

按需加载的代码实现这里就不详细展开了，因为鉴于用的框架不同，实现起来都是不一样的。当然了，虽然他们的用法可能不同，但是底层的机制都是一样的。都是当使用的时候再去下载对应文件，返回一个 Promise，当 Promise 成功以后去执行回调。
