# Node 基础

## 全局变量

### require

- 内建模块直接从内存加载
- 文件模块通过路径定位到文件
- 包通过`package.json`配置文件内`main`字段查找入口

#### JSON 文件

通过 `fs.readFileSync()` 加载
通过 `JSON.parse()` 解析

#### 大文件

`require` 加载完成后会缓存文件
大量使用会导致大量数据驻留在内存中，导致内存泄漏

### export

`require` 一个模块实际得到的是该模块的 `exports` 属性，`exports`是 `module` 的属性，默认情况是空对象

- `module.exports`： 导出一个对象
- `exports.xxx`： 导出具有多个属性的对象

### 路径变量

- `__dirname`：执行文件所在目录
- `__filename`：执行文件文件名称

### process

`process` 对象是一个全局变量，它提供有关当前 `Node.js` 进程的信息并对其进行控制

+ env：
+ argv：返回启动进程时命令行参数数组，第一个参数是 `node` 的执行文件路径，第二个参数正在执行的脚本路径
