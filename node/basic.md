# Node

`Node.js` 是一个内置有 `chrome V8` 引擎的 `JavaScript` 运行环境，他可以使原本在浏览器中运行的 `JavaScript` 有能力跑后端，从而操作我们数据库，进行文件读写等。

**用途**

- 应用中间层
- 构建工具
- 小型项目服务端

**特点**

- 事件驱动
- 非阻塞 IO 模型（异步）
- 轻量和高效

# 模块化

`Node.js` 采用的是 `CommonJs` 规范，在 `NodeJS` 中，一般将代码合理拆分到不同的 JS 文件中，每一个文件就是一个模块，而文件路径就是模块名。 在编写每个模块时，都有 `require` 、 `exports` `、module` 三个预先定义好的变量可供使用。

**分类**

1. 核心模块（已经封装好的内置模块）
2. 自己定义的模块
3. 第三方的模块（npm 下载下来的）

#### require

`require` 函数用来在一个模块中引入另外一个模块。传入一个模块名，返回一个模块导出对象。用法： `let cc = require("模块名")` ，其中模块名可以用绝对路径也可以用相对路径，模块的后缀名.js 可以省略。

`require`函数两个作用：

- 执行导入的模块中的代码
- 返回导入模块中的接口对象

#### exports

`exports` 对象用来导出当前模块的公共方法或属性，别的模块通过 `require` 函数使用当前模块时得到的就是当前模块的`exports`对象。用法：`exports.name` name 为导出的对象名。

```JS
exports.add = function () {
  let i = 0
  console.log(++i)
}

// 导出一个add方法供其他模块使用
```

#### module.exports

`module.exports` 用来导出一个默认对象，没有指定对象名，常见于修改模块的原始导出对象。比如原本模块导出的是一个对象，我们可以通过 `module.exports` 修改为导出一个函数。如下：

```JS
module.exports = function () {
  console.log('hello world！')
}
```

#### 模块的初始化

一个模块中的 JS 代码仅在模块第一次被使用时执行一次，并且在使用的过程中进行初始化，之后缓存起来便于后续继续使用。

#### 主模块

通过命令行参数传递给 `NodeJS` 以启动程序的模块被称为主模块。主模块负责调度组成整个程序的其它模块完成工作。

```JS
node main.js // 运行main.js启动程序，main.js称为主模块
```

#### 加载规则

**加载模块**

1. 判断`node`是否具有该模块，如果有使用自身核心模块
1. `require('模块名')`优先在加载该包的模块的同级目录 `node_modules` 中查找第三方包。
1. 找到该第三方包中的`package.json`文件，并且找到里面的 `main` 属性对应的入口模块，该入口模块即为加载的第三方模块。
1. 如果在要加载的第三方包中没有找到`package.json`文件或者是`package.json`文件中没有`main`属性，则默认加载第三方包中的`index.js`文件。
1. 如果在加载第三方模块的文件的同级目录没有找到 node_modules 文件夹，或者以上所有情况都没有找到，则会向上一级父级目录下查找 node_modules 文件夹，查找规则如上一致。
1. 如果一直找到该模块的磁盘根路径都没有找到，则会报错：`can not find module xxx`

# 核心模块

`Node.js` 中核心模块指的是 `Node.js` 官方封装好的 j`s` 模块，这些模块都有特定的名称标识，可以直接拿来使用。

常见的核心模块： `path`，`os`，`http`，`fs` 等等

# process

`process` 对象是一个全局变量，提供了有关当前 `Node.js` 进程的信息并对其进行控制。 作为全局变量，它始终可供 `Node.js` 应用程序使用，无需使用 `require()`。

1. env：`Node.js` 的 `process` 核心模块提供了 `env` 属性，该属性承载了在启动进程时设置的所有环境变量。环境变量默认情况下被设置为 `development`
2. argv：包含所有命令行调用参数的数组。第一个参数是 `node` 命令的完整路径。第二个参数是正被执行的文件的完整路径。所有其他的参数从第三个位置开始。可以配合[minimist](https://www.npmjs.com/package/minimist)库解析参数键值对

## http

1. 引入 `http` 模块，`const http = require('http')`
2. 创建 `server` 实例， `const server = http.createServer()`
3. 接收 `request` 事件，`server.on('request', (req, res) => { })`
4. 监听端口号开启服务器，`server.listen(3000, () => {})`
5. 返回信息，`res.end('index page')`，响应的数据类型必须是字符串或者二进制数据

## fs

`Node.js` 中赋予了 `JavaScript` 进行文件的读写操作

#### readFile

`readFile` 函数接受两个参数：读取文件路径，回调函数（`error`，`data` 两个参数），读取文件成功：`data` 为文件内容，`error` 为 `null`，读取失败：`error` 为错误对象，`data` 为 `undefined`

```JS
let fs = require('fs')
  fs.readFile('./hello.txt', (error, data) => {
  console.log(data.toString())
})
```

#### writeFile

`writeFile` 接受四个参数：写入文件路径，写入内容，配置，回调函数。写入成功时候：`error` 为 `null`，写入失败时候：`error` 为错误对象

```JS
fs.writeFile('/Users/joe/test.txt', content, { flag: 'a+' }, err => {})
```

可以通过指定标志来修改默认的行为：

- r+ 打开文件用于读写。
- w+ 打开文件用于读写，将流定位到文件的开头。如果文件不存在则创建文件。
- a 打开文件用于写入，将流定位到文件的末尾。如果文件不存在则创建文件。
- a+ 打开文件用于读写，将流定位到文件的末尾。如果文件不存在则创建文件。

## path

`path`模块提供了一些路径操作的 `API`

1. `path.extname()` 获取文件（可以是一个路径文件）的扩展名
2. `path.resolve([...paths])` 方法会将路径或路径片段的序列解析为绝对路径。给定的路径序列会从右到左进行处理，后面的每个 `path` 会被追加到前面，直到构造出绝对路径。
3. `path.join()` 方法会将所有给定的 `path` 片段连接到一起（使用平台特定的分隔符作为定界符），然后规范化生成的路径。

**相关常量**

- `__dirname`： 获得当前执行文件所在目录的完整目录名
- `__filename`： 获得当前执行文件的带有完整绝对路径的文件名
- `process.cwd()`：获得当前执行 `node` 命令时候的文件夹目录名

## os

`os` 模块提供了与操作系统相关的实用方法和属性

1. `os.cpus()` 获取操作系统的 CPU 信息
2. `os.totalmem()` 获取内存信息

## url

`url` 模块用于处理与解析 `URL`

1. `url.parse()` 方法可以解析一个 `url` 地址，通过传入第二个参数（`true`）把包含有查询字符串的 query 转换成对象
2. `url.resolve()` 方法解析相对于基 URL 的目标 URL。第一个参数：基 URL，第二个参数：目标 URL，第二个参数前面的'/'表示根路径，如果省略则取代基 URL 的最后一个子地址

# 非阻塞 I/O

`Node.js` 是以单线程的模式运行，`Node.js` 不为每个客户连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了， 就触发一个内部事件，通过非阻塞 I/O、事件驱动机制，让 `Node.js` 程序宏观上也是并行的。使用 `Node.js`，一个 8GB 内存的服务器，可以同时处理超过 4 万用户的连接

# 事件驱动

1. `Node.js`使用事件驱动模型，当 `server` 收到请求，就关闭请求然后处理，接着处理下一个 `web` 请求。
1. 当一个请求完成，它会返回处理队列，直到队列开头，会被返回给用户。
1. 因为 `server` 一直接受请求而不等待任何读写操作，该模型非常高效，拓展性强。
1. 会有一个主循环来监听事件，当检测到事件时触发回调函数。
