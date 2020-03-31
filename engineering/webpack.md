<!-- TOC -->

- [概念](#%e6%a6%82%e5%bf%b5)
- [初始化](#%e5%88%9d%e5%a7%8b%e5%8c%96)
- [参数配置](#%e5%8f%82%e6%95%b0%e9%85%8d%e7%bd%ae)
  - [mode](#mode)
  - [entry](#entry)
    - [单页面](#%e5%8d%95%e9%a1%b5%e9%9d%a2)
    - [多页面](#%e5%a4%9a%e9%a1%b5%e9%9d%a2)
  - [output](#output)
    - [构建库/包参数](#%e6%9e%84%e5%bb%ba%e5%ba%93%e5%8c%85%e5%8f%82%e6%95%b0)
  - [devtool](#devtool)
  - [resolve](#resolve)
  - [optimization](#optimization)
- [模块处理](#%e6%a8%a1%e5%9d%97%e5%a4%84%e7%90%86)
  - [处理 JS](#%e5%a4%84%e7%90%86-js)
  - [vue 文件](#vue-%e6%96%87%e4%bb%b6)
  - [样式文件](#%e6%a0%b7%e5%bc%8f%e6%96%87%e4%bb%b6)
    - [抽离 css](#%e6%8a%bd%e7%a6%bb-css)
    - [压缩 css](#%e5%8e%8b%e7%bc%a9-css)
  - [图片/字体](#%e5%9b%be%e7%89%87%e5%ad%97%e4%bd%93)
    - [html 中本地图片](#html-%e4%b8%ad%e6%9c%ac%e5%9c%b0%e5%9b%be%e7%89%87)
- [插件配置](#%e6%8f%92%e4%bb%b6%e9%85%8d%e7%bd%ae)
  - [html 模板](#html-%e6%a8%a1%e6%9d%bf)
  - [重置 dist](#%e9%87%8d%e7%bd%ae-dist)
  - [静态拷贝](#%e9%9d%99%e6%80%81%e6%8b%b7%e8%b4%9d)
  - [全局变量](#%e5%85%a8%e5%b1%80%e5%8f%98%e9%87%8f)
- [开发环境](#%e5%bc%80%e5%8f%91%e7%8e%af%e5%a2%83)
  - [热更新](#%e7%83%ad%e6%9b%b4%e6%96%b0)
  - [代理](#%e4%bb%a3%e7%90%86)
  - [数据模拟](#%e6%95%b0%e6%8d%ae%e6%a8%a1%e6%8b%9f)
- [按需加载](#%e6%8c%89%e9%9c%80%e5%8a%a0%e8%bd%bd)
- [区分环境](#%e5%8c%ba%e5%88%86%e7%8e%af%e5%a2%83)
- [环境变量](#%e7%8e%af%e5%a2%83%e5%8f%98%e9%87%8f)
- [性能优化](#%e6%80%a7%e8%83%bd%e4%bc%98%e5%8c%96)
  - [量化](#%e9%87%8f%e5%8c%96)
  - [exclude/include](#excludeinclude)
  - [cache-loader](#cache-loader)
  - [HappyPack](#happypack)
  - [thread-loader](#thread-loader)
  - [HardSourceWebpackPlugin](#hardsourcewebpackplugin)
  - [noParse](#noparse)
  - [IgnorePlugin](#ignoreplugin)
  - [externals](#externals)
  - [DllPlugin](#dllplugin)
  - [抽离公共代码](#%e6%8a%bd%e7%a6%bb%e5%85%ac%e5%85%b1%e4%bb%a3%e7%a0%81)
  - [Tree-shaking](#tree-shaking)
  - [webpack-bundle-analyzer](#webpack-bundle-analyzer)

<!-- /TOC -->

# 概念

本质上，`webpack` 是一个现代 `JavaScript` 应用程序的静态模块打包器(`module bundler`)。当 `webpack` 处理应用程序时，它会递归地构建一个依赖关系图(`dependency graph`)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 `bundle`。

1. 入口起点(`entry point`)指示 `webpack` 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，`webpack` 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
2. `loader` 可以将所有类型的文件转换为 `webpack` 能够处理的有效模块，然后你就可以利用 `webpack` 的打包能力，对它们进行处理。处理顺序由右向左进行一次处理
3. `plugins` 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。
4. `output` 属性告诉 `webpack` 在哪里输出它所创建的 `bundles`，以及如何命名这些文件，默认值为 `./dist`。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中

**[webpack](https://github.com/liupeng1218/SecretGardenDemo/tree/master/m-webpack-cli)**

# 初始化

webpack 依赖`webpack` `webpack-cli` 来进行支持

```
npm install webpack webpack-cli -D
```

webpack 开箱即用，可以无需使用任何配置文件。然而，webpack 会假定项目的入口起点为 src/index，然后会在 dist/main.js 输出结果，并且在生产环境开启压缩和优化。

# 参数配置

## mode

告知 `webpack` 使用相应模式的内置优化，可以设置为 `none` ， `development` ， `production`，默认值是 `production`，对应内置优化为[mode](https://webpack.js.org/configuration/mode/#root)

## entry

### 单页面

`entry` 的值可以是一个字符串，一个数组或是一个对象。

字符串：以对应的文件为入口
数组：表示有“多个主入口”，多个依赖文件一起注入

### 多页面

`HtmlWebpackPlugin` 提供了一个 `chunks` 的参数，可以接受一个数组，配置此参数仅会将数组中指定的 `js` 引入到 `html` 文件中，此外，如果你需要引入多个 `JS` 文件，仅有少数不想引入，还可以指定 `excludeChunks` 参数，它接受一个数组。

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: 'index.html', //打包后的文件名
            chunks: ['index']
        }),
        new HtmlWebpackPlugin({
            template: './public/login.html',
            filename: 'login.html', //打包后的文件名
            chunks: ['login']
        }),
    ]
```

## output

位于对象最顶级键(key)，包括了一组选项，指示 `webpack` 如何去输出、以及在哪里输出

1.  path：output 目录对应一个**绝对路径**
2.  filename：此选项决定了每个输出 `bundle` 的名称。这些 `bundle` 将写入到 `output.path` 选项指定的目录下。注意此选项被称为文件名，但是你还是可以使用像 `js/[name]/bundle.js` 这样的**文件夹结构**。
3.  | 模板        | 描述                                                        |
    | ----------- | ----------------------------------------------------------- |
    | [hash]      | 模块标识符(module identifier)的 hash，可以使用:6 来指定长度 |
    | [chunkhash] | chunk 内容的 hash                                           |
    | [name]      | 模块名称                                                    |
    | [id]        | 模块标识符(module identifier)                               |
    | [ext]       | 模块类型                                                    |
    | [query]     | 模块的 query，例如，文件名 ? 后面的字符串                   |
    | [function]  | The function, which can return filename [string]            |
4.  publicPath：该选项的值是以 `runtime`(运行时) 或 `loader`(载入时) 所创建的每个 `URL` 为前缀。因此，在多数情况下，此选项的值都会以 `/` 结束。
5.  output.chunkFilename：此选项决定了非入口(non-entry) chunk 文件的名称，命名方式参考 filename

### 构建库/包参数

1. libraryTarget： 选项用来配置如何暴露库，可配置以 `commonJS` `模块、AMD` 模块，甚至全局变量形式暴露库。
1. library： 对外暴露的变量名或模块名，具体作用与 `output.libraryTarget` 选项的值有关。
1. umdNamedDefine： 当 `output.libraryTarget` 的值为“umd”时，设置该选项的值为 `true` 会对 `UMD` 的构建过程中的 `AMD` 模块进行命名，否则就使用匿名的 `define`，匿名的 `AMD` 模块。
1. globalObject：用来指定挂载这个库的全局对象，默认值是 `window` 。，当构建 `UMD` 包需要兼容浏览器和 `Node.js`环境时，值应该设为 `this`

## devtool

此选项控制是否生成，以及如何生成 `source map`。不同的构建方式映射规则和速度有不用
开发环境使用`cheap-module-eval-source-map`，生产环境使用`none`

## resolve

`resolve` 配置 `webpack` 如何寻找模块所对应的文件。`webpack` 内置 `JavaScript` 模块化语法解析功能，默认会采用模块化标准里约定好的规则去寻找，但你可以根据自己的需要修改默认的规则。

1. modules:`resolve.modules` 配置 `webpack` 去哪些目录下寻找第三方模块，默认情况下，只会去 `node_modules` 下寻找，如果你我们项目中某个文件夹下的模块经常被导入，不希望写很长的路径，那么就可以通过配置`resolve.modules` 来简化。`modules: ['./src/components', 'node_modules']`
2. alias：配置项通过别名把原导入路径映射成一个新的导入路径。`alias:{' @':path.resolve(__dirname,'../src')},`
3. extensions：自动解析确定的扩展，默认为`[".js", ".json"]`

## optimization

`webpack` 会根据当前`mode`提供默认的内置优化，也可以在`optimization`手动优化配置

1.  splitChunks：如何对通用的块进行提取打包[splitChunks](https://webpack.js.org/plugins/split-chunks-plugin/)
2.  minimizer：对打包文件最小化处理，在这个声明 js，css 等压缩插件

# 模块处理

用于对模块的源代码进行转换。`loader` 可以使你在 `import` 或"加载"模块时预处理文件。因此，`loader` 类似于其他构建工具中“任务(task)”，在`module.rules`中配置`loader`

1.  test ：用于标识出应该被对应的 loader 进行转换的某个或某些文件。
2.  use ：表示进行转换时，应该使用哪个 loader。多个 user 时，处理顺序为从后向前
3.  include：指定解析范围
4.  exclude：排除解析目录
5.  noParse：排除无需解析的特定库

## 处理 JS

将 JS 代码向低版本转换，我们需要使用 `babel-loader`处理 js,`babel-loader`需要一些依赖

```
npm install @babel/core @babel/preset-env @babel/plugin-transform-runtime -D

npm install @babel/runtime @babel/runtime-corejs3
```

**@babel/runtime @babel/runtime-corejs3 需要安装在生产依赖中**

**.babelrc**
`babel`需要编制配置规则来进行转换

```JS
{
    "presets": ["@babel/preset-env"],
    "plugins": [
        [
            "@babel/plugin-transform-runtime",
            {
                "corejs": 3
            }
        ]
    ]
}
```

**配置**

```JS
//webpack.config.js
module.exports = {
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                use: ['babel-loader'],
                exclude: /node_modules/ //排除 node_modules 目录
            }
        ]
    }
}
```

## vue 文件

`vue-loader`和`vue-template-compiler`是转换 `vue` 文件所需的依赖
需要配合 `VueLoaderPlugin` 插件使用

**配置**

```JS
// webpack.config.js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  mode: 'development',
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      // 它会应用到普通的 `.js` 文件
      // 以及 `.vue` 文件中的 `<script>` 块
      {
        test: /\.js$/,
        loader: 'babel-loader'
      },
      // 它会应用到普通的 `.css` 文件
      // 以及 `.vue` 文件中的 `<style>` 块
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    // 请确保引入这个插件来施展魔法
    new VueLoaderPlugin()
  ]
}

```

## 样式文件

如果是 `.css`，我们需要的 loader 通常有： `style-loader`、`css-loader`
样式兼容性需要`postcss-loader`和`autoprefixer`配合处理
使用预处理器，需要对应的`loader`进行转换，`less-loader` 和 `sass-loader`

1. style-loader 动态创建 style 标签，将 css 插入到 head 中.
1. css-loader 负责处理 @import 等语句。
1. postcss-loader 和 autoprefixer，自动生成浏览器兼容性前缀 —— 2020 了，应该没人去自己徒手去写浏览器前缀了吧
1. less-loader 负责处理编译 .less 文件,将其转为 css

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    module: {
        rules: [
            {
                test: /\.(s|c)ss$/,
                use: ['style-loader', 'css-loader', {
                    loader: 'postcss-loader',
                    options: {
                        plugins: function () {
                            return [
                                require('autoprefixer')({
                                    "overrideBrowserslist": [
                                        ">0.25%",
                                        "not dead"
                                    ]
                                })
                            ]
                        }
                    }
                }, 'sass-loader'],
                exclude: /node_modules/
            }
        ]
    }
}

```

### 抽离 css

默认情况下，`css` 会通过 `style` 标签的形式插入到 html 中。可以通过`mini-css-extract-plugin`插件抽离`css`

**配置**

```JS
//webpack.config.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/[name].css' //根据个人习惯/规范将css文件放在指定目录下
        })
    ],
    module: {
        rules: [
            {
                test: /\.(s|c)ss$/,
                use: [
                    {loader:MiniCssExtractPlugin.loader,options: {
                        hmr: isDev, // const isDev = process.env.NODE_ENV === 'development';
                        reloadAll: true,
                    }}, //替换之前的 style-loader
                    'css-loader', {
                        loader: 'postcss-loader',
                        options: {
                            plugins: function () {
                                return [
                                    require('autoprefixer')({
                                        "overrideBrowserslist": [
                                            "defaults"
                                        ]
                                    })
                                ]
                            }
                        }
                    }, 'sass-loader'
                ],
                exclude: /node_modules/
            }
        ]
    }
}
```

### 压缩 css

使用 `mini-css-extract-plugin`，`CSS` 文件默认不会被压缩，如果想要压缩，需要配置 `optimization`，首先安装 `optimize-css-assets-webpack-plugin`

**配置**

```JS
//webpack.config.js
const OptimizeCssPlugin = require('optimize-css-assets-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    //....
    plugins: [
        new OptimizeCssPlugin()
    ],
}
```

> 将 `OptimizeCssPlugin` 直接配置在 `plugins` 里面，那么 `js` 和 `css` 都能够正常压缩，如果你将这个配置在 `optimization`，那么需要再配置一下 `js` 的压缩

## 图片/字体

使用 `url-loader` 或者 `file-loader` 来处理本地的资源文件。`url-loader` 和 `file-loader` 的功能类似，但是 `url-loader` 可以指定在文件大小小于指定的限制时，返回 `DataURL`

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    modules: {
        rules: [
            {
                test: /\.(png|jpg|gif|jpeg|webp|svg|eot|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: 10240, //10K
                            esModule: false
                        }
                    }
                ],
                exclude: /node_modules/
            }
        ]
    }
}
```

**url-loader options 主要参数**
limit：资源小于该值会转换为`base64`
name：资源生成命名
outputPath：资源生成目录

### html 中本地图片

图片被处理后，`html`页面找不到对应文件路径，需要配合`html-withimg-loader`进行处理

**配置**

```JS
module.exports = {
    //...
    module: {
        rules: [
            {
                test: /.html$/,
                use: 'html-withimg-loader'
            }
        ]
    }
}
```

# 插件配置

## html 模板

每次打包的文件都会有响应的 hash 等变动，不需要每次手动修改，配合`html-webpack-plugin`插件实现引入资源的自动更新，参考文档[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin#configuration)
多页面时，需要配置`chunks`参数配合入口文件的对应的模块名实现文件的匹配

**配置**

```JS
//首先引入插件
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    //...
    plugins: [
        //数组 放着所有的webpack插件
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: 'index.html', //打包后的文件名
            minify: {
                removeAttributeQuotes: false, //是否删除属性的双引号
                collapseWhitespace: false, //是否折叠空白
            },
            // hash: true //是否加上hash，默认是 false
        })
    ]
}
```

## 重置 dist

`clean-webpack-plugin`可以在打包输出前清空文件夹
cleanOnceBeforeBuildPatterns：不删除指定文件
**配置**

```JS
//webpack.config.js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    //...
    plugins: [
        //不需要传参数喔，它可以找到 outputPath
        new CleanWebpackPlugin()
    ]
}
```

## 静态拷贝

使用`copy-webpack-plugin`插件拷贝不需要打包的静态文件

**配置**

```JS
//webpack.config.js
const CopyWebpackPlugin = require('copy-webpack-plugin');
module.exports = {
    //...
    plugins: [
        new CopyWebpackPlugin([
            {
                from: 'public/js/*.js',
                to: path.resolve(__dirname, 'dist', 'js'),
                flatten: true, // 是否拷贝文件路径
            },
            //还可以继续配置其它要拷贝的文件
        ])
    ]
}
```

## 全局变量

`ProvidePlugin` 的作用就是不需要 `import` 或 `require` 就可以在项目中到处使用。

默认寻找路径是当前文件夹 `./**` 和 `node_modules`
**配置**

```JS
new webpack.ProvidePlugin({
  identifier1: 'module1',
  identifier2: ['module2', 'property2']
});
```

如使用`ESLint`，需要在配置中对全局变量进行忽略

```JS
{
    "globals": {
        "React": true,
        "Vue": true,
        //....
    }
}
```

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
10. quiet：启用 quiet 后，除了初始启动信息之外的任何内容都不会被打印到控制台。
11. overlay：当编译出错时，会在浏览器窗口全屏输出错误，默认是关闭的
12. stats: 终端中仅打印出 error，注意当启用了 quiet 或者是 noInfo 时，此属性不起作用。

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    devServer: {
        port: '3000', //默认是8080
        hot: true,
        inline: true, //默认开启 inline 模式，如果设置为false,开启 iframe 模式
        stats: "errors-only", //终端仅打印 error
        overlay: false, //默认不启用
        clientLogLevel: "silent", //日志等级
        compress: true //是否启用 gzip 压缩
    }
}
```

## 热更新

1. 首先配置 `devServer` 的 `hot` 为 `true`
1. 并且在 `plugins` 中增加 `new webpack.HotModuleReplacementPlugin()`

**配置**

```JS
//webpack.config.js
const webpack = require('webpack');
module.exports = {
    //....
    devServer: {
        hot: true
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin() //热更新插件
    ]
}
```

直接启用热更新会导致整个页面刷新，需要在入口文件进行配置，实现局部刷新

**配置**

```JS
// 入口文件
if(module && module.hot) {
    module.hot.accept()
}
```

## 代理

开发中如经常会需要进行跨域通信，可以代理在开发过程中进行跨域请求

1. api：需要拦截路径的匹配条件，通常用于区分代理请求和非代理请求
2. target：需要代理到的目标
3. pathRewrite：实际请求替换掉代理请求标识

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    devServer: {
        proxy: {
            '/api': {
                target: 'http://localhost:4000',
                pathRewrite: {
                    '/api': ''
                }
            }
        }
    }
}
```

## 数据模拟

利用 `mock-api` 快速模拟 api

在项目中 mock 文件中创建 请求响应处理文件

```JS
module.exports = {
    'GET /user': {name: '刘小夕'},
    'POST /login/account': (req, res) => {
        const { password, username } = req.body
        if (password === '888888' && username === 'admin') {
            return res.send({
                status: 'ok',
                code: 0,
                token: 'sdfsdfsdfdsf',
                data: { id: 1, name: '刘小夕' }
            })
        } else {
            return res.send({ status: 'error', code: 403 })
        }
    }
}
```

在 webpack 配置启用模拟 api 服务

```JS
const apiMocker = require('mocker-api');
module.export = {
    //...
    devServer: {
        before(app){
            apiMocker(app, path.resolve('./mock/mocker.js'))
        }
    }
}
```

# 按需加载

很多时候我们不需要一次性加载所有的 JS 文件，而应该在不同阶段去加载所需要的代码。`webpack` 内置了强大的分割代码的功能可以实现按需加载。

比如，我们在点击了某个按钮之后，才需要使用使用对应的 `JS` 文件中的代码，需要使用 `import()` 语法：
要支持 `import()` 语法，需要增加 `babel` 的配置，安装依赖:`@babel/plugin-syntax-dynamic-import`

**配置**

```JS
{
    "presets": ["@babel/preset-env"],
    "plugins": [
        [
            "@babel/plugin-transform-runtime",
            {
                "corejs": 3
            }
        ],
        "@babel/plugin-syntax-dynamic-import"
    ]
}

```

`webpack` 遇到 `import(****)` 这样的语法的时候，会这样处理：

1. 以`****` 为入口新生成一个 `Chunk`，可以使用`/* webpackChunkName: "plugins/myModule" */` 在路径在注释为模块配置包生成名
1. 当代码执行到 `import` 所在的语句时，才会加载该 `Chunk` 所对应的文件

# 区分环境

不同环境需要根据需求区分不同的配置

- 开发环境：开发环境主要实现的是热更新,不要压缩代码，完整的 `sourceMap`
- 生产环境：生产环境主要实现的是压缩代码、提取 css 文件、合理的 `sourceMap`、分割代码

可以定制不同的规范进行做区分

- `webpack.base.js` 定义公共的配置
- `webpack.dev.js`：定义开发环境的配置
- `webpack.prod.js`：定义生产环境的配置

`webpack-merge` 专为 `webpack` 设计，提供了一个 `merge` 函数，用于连接数组，合并对象。

# 环境变量

使用 `webpack` 内置插件 `DefinePlugin` 来定义环境变量。`DefinePlugin` 中的每个键，是一个标识符.

- 如果 `value` 是一个字符串，会被当做 `code` 片段
- 如果 `value` 不是一个字符串，会被 `stringify`
- 如果 `value` 是一个对象，正常对象定义即可
- 如果 `key` 中有 `typeof`，它只针对 `typeof` 调用定义

**配置**

```JS
//webpack.config.dev.js
const webpack = require('webpack');
module.exports = {
    plugins: [
        new webpack.DefinePlugin({
            DEV: JSON.stringify('dev'), //字符串
            FLAG: 'true' //FLAG 是个布尔类型
        })
    ]
}

//index.js
if(DEV === 'dev') {
    //开发环境
}else {
    //生产环境
}
```

# 性能优化

## 量化

`speed-measure-webpack-plugin` 插件可以测量各个插件和`loader`所花费的时间，使用之后，构建时，会得构建信息，可以对比优化前后信息比较

**配置**

```JS
//webpack.config.js
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();

const config = {
    //...webpack配置
}

module.exports = smp.wrap(config);
```

## exclude/include

通过 `exclude`、`include` 配置来确保转译尽可能少的文件。顾名思义，`exclude` 指定要排除的文件，`include` 指定要包含的文件。
`exclude` 的优先级高于 `include`，在 `include` 和 `exclude` 中使用绝对路径数组，尽量避免 `exclude`，更倾向于使用 `include`。

**配置**

```JS
//webpack.config.js
const path = require('path');
module.exports = {
    //...
    module: {
        rules: [
            {
                test: /\.js[x]?$/,
                use: ['babel-loader'],
                include: [path.resolve(__dirname, 'src')]
            }
        ]
    },
}
```

## cache-loader

在一些性能开销较大的 `loader` 之前添加 `cache-loader`，将结果缓存中磁盘中。默认保存在`node_modueles/.cache/cache-loader` 目录下。`cache-loader` 的配置很简单，放在其他 `loader` 之前即可

**配置**

```JS
module.exports = {
    //...

    module: {
        rules: [
            {
                test: /\.jsx?$/,
                use: ['cache-loader','babel-loader']
            }
        ]
    }
}
```

## HappyPack

受限于 `Node` 是单线程运行的，所以 `Webpack` 在打包的过程中也是单线程的，特别是在执行 `Loader` 的时候，长时间编译的任务很多，这样就会导致等待的情况。

`HappyPack` 可以将 `Loader` 的同步执行转换为并行的，这样就能充分利用系统资源来加快打包效率了

> 当项目不是很复杂时，不需要配置 `happypack`，因为进程的分配和管理也需要时间，并不能有效提升构建速度，甚至会变慢。
> **配置**

```JS
const Happypack = require('happypack');
module.exports = {
    //...
    module: {
        rules: [
            {
                test: /\.js[x]?$/,
                use: 'Happypack/loader?id=js',
                include: [path.resolve(__dirname, 'src')]
            },
            {
                test: /\.css$/,
                use: 'Happypack/loader?id=css',
                include: [
                    path.resolve(__dirname, 'src'),
                    path.resolve(__dirname, 'node_modules', 'bootstrap', 'dist')
                ]
            }
        ]
    },
    plugins: [
        new Happypack({
            id: 'js', //和rule中的id=js对应
            //将之前 rule 中的 loader 在此配置
            use: ['babel-loader'] //必须是数组
        }),
        new Happypack({
            id: 'css',//和rule中的id=css对应
            use: ['style-loader', 'css-loader','postcss-loader'],
        })
    ]
}
```

## thread-loader

除了使用 `Happypack` 外，我们也可以使用 `thread-loader` ，把 `thread-loader` 放置在其它 `loader` 之前，那么放置在这个 `loader` 之后的 `loader` 就会在一个单独的 `worker` 池中运行。
在 `worker` 池(`worker pool`)中运行的 `loader` 是受到限制的。例如：

- 这些 loader 不能产生新的文件。
- 这些 loader 不能使用定制的 loader API（也就是说，通过插件）。
- 这些 loader 无法获取 webpack 的选项设置。

**配置**

```JS
module.exports = {
    module: {
        //我的项目中,babel-loader耗时比较长，所以我给它配置 thread-loader
        rules: [
            {
                test: /\.jsx?$/,
                use: ['thread-loader', 'cache-loader', 'babel-loader']
            }
        ]
    }
}
```

> `Happypack`和`thread-loader`选用一个使用就可以，`thread-loader`配置更为简单

## HardSourceWebpackPlugin

`HardSourceWebpackPlugin` 为模块提供中间缓存，缓存默认的存放路径是: `node_modules/.cache/hard-source`。配置 `hard-source-webpack-plugin`，首次构建时间没有太大变化，但是第二次开始，构建时间大约可以节约 80%。

**配置**

```JS
//webpack.config.js
var HardSourceWebpackPlugin = require('hard-source-webpack-plugin');
module.exports = {
    //...
    plugins: [
        new HardSourceWebpackPlugin()
    ]
}
```

## noParse

如果一些第三方模块没有 `AMD/CommonJS`规范版本，可以使用 `noParse` 来标识这个模块，这样 `Webpack` 会引入这些模块，但是不进行转化和解析，从而提升 `Webpack` 的构建性能
`noParse` 属性的值是一个正则表达式或者是一个 `function`。

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    module: {
        noParse: /jquery|lodash/
    }
}
```

## IgnorePlugin

`webpack` 的内置插件，作用是忽略第三方包指定目录。

例如: `moment`会将所有本地化内容和核心功能一起打包，我们就可以使用 `IgnorePlugin` 在打包时忽略本地化内容。

**配置**

```JS
//webpack.config.js
module.exports = {
    //...
    plugins: [
        //忽略 moment 下的 ./locale 目录
        new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/)
    ]
}

// 如需要语言包，单独引用即可
import moment from 'moment';
import 'moment/locale/zh-cn';// 手动引入
```

## externals

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
    vue: {
      root: 'Vue',
      commonjs: 'vue',
      commonjs2: 'vue',
      amd: 'vue'
    }
  }
}
```

## DllPlugin

有些时候，如果所有的 `JS` 文件都打成一个 `JS` 文件，会导致最终生成的 `JS` 文件很大，这个时候，我们就要考虑拆分 `bundles`。

`DllPlugin` 和 `DLLReferencePlugin` 可以实现拆分 `bundles`，并且可以大大提升构建速度，`DllPlugin` 和 `DLLReferencePlugin` 都是 `webpack` 的内置模块。

我们使用 `DllPlugin` 将不会频繁更新的库进行编译，当这些依赖的版本没有变化时，就不需要重新编译。
我们新建一个 `webpack` 的配置文件，来专门用于编译动态链接库，例如名为: `webpack.config.dll.js`

**配置**

```JS
//webpack.config.dll.js
const webpack = require('webpack');
const path = require('path');

module.exports = {
    entry: {
        vue: ['vue', 'vuex']
    },
    mode: 'production',
    output: {
        filename: '[name].dll.[hash:6].js',
        path: path.resolve(__dirname, 'dist', 'dll'),
        library: '[name]_dll' //暴露给外部使用
        //libraryTarget 指定如何暴露内容，缺省时就是 var
    },
    plugins: [
        new webpack.DllPlugin({
            //name和library一致
            name: '[name]_dll',
            path: path.resolve(__dirname, 'dist', 'dll', 'manifest.json') //manifest.json的生成路径
        })
    ]
}
//webpack.config.js
const webpack = require('webpack');
const path = require('path');
module.exports = {
    //...
    devServer: {
        contentBase: path.resolve(__dirname, 'dist')
    },
    plugins: [
        new webpack.DllReferencePlugin({
            manifest: path.resolve(__dirname, 'dist', 'dll', 'manifest.json')
        }),
        new CleanWebpackPlugin({
            cleanOnceBeforeBuildPatterns: ['**/*', '!dll', '!dll/**'] //不删除dll目录
        }),
        //...
    ]
}
// package
{
    "scripts": {
        "dev": "NODE_ENV=development webpack-dev-server",
        "build": "NODE_ENV=production webpack",
        "build:dll": "webpack --config webpack.config.dll.js"
    },
}
```

## 抽离公共代码

抽离公共代码是对于多页应用来说的，如果多个页面引入了一些公共模块，那么可以把这些公共的模块抽离出来，单独打包。公共代码只需要下载一次就缓存起来了，避免了重复下载。

抽离公共代码对于单页应用和多页应该在配置上没有什么区别，都是配置在 `optimization.splitChunks` 中。

**配置**

```JS
//webpack.config.js
module.exports = {
    optimization: {
        splitChunks: {//分割代码块
            cacheGroups: {
                vendor: {
                    //第三方依赖
                    priority: 1, //设置优先级，首先抽离第三方模块
                    name: 'vendor',
                    test: /node_modules/,
                    chunks: 'initial',
                    minSize: 0,
                    minChunks: 1 //最少引入了1次
                },
                //缓存组
                common: {
                    //公共模块
                    name: 'common',
                    chunks: 'initial',
                    minSize: 100, //大小超过100个字节
                    minChunks: 3 //最少引入了3次
                }
            }
        }
    }
}
```

`runtimeChunk` 的作用是将包含 `chunk` 映射关系的列表从 `main.js` 中抽离出来，在配置了 `splitChunk` 时，记得配置 `runtimeChunk`

**配置**

```JS
module.exports = {
    //...
    optimization: {
        runtimeChunk: {
            name: 'manifest'
        }
    }
}
```

## Tree-shaking

> 主要在构建库/包时使用

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

## webpack-bundle-analyzer

`webpack-bundle-analyzer` 可以直观展示应用包体的详细信息

**配置**

```JS
//webpack.config.prod.js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
const merge = require('webpack-merge');
const baseWebpackConfig = require('./webpack.config.base');
module.exports = merge(baseWebpackConfig, {
    //....
    plugins: [
        //...
        new BundleAnalyzerPlugin(),
    ]
})
```
