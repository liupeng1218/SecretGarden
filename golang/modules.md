# Go Modules

## GOPATH

将包或者别人的包全部放在 `$GOPATH/src` 目录下进行管理的方式，我们称之为 `GOPATH` 模式

目录结构
>bin：存放编译后生成的二进制可执行文件
pkg：存放编译后生成的 `.a` 文件
src：存放项目的源代码，可以是你自己写的代码，也可以是你 `go get` 下载的包

在这个模式下，安装包 `go install module` ，生成的可执行文件会放在 `$GOPATH/bin`
在这个模式下，安装库 `go install package` ，生成的 `.a` 会放在 `$GOPATH/pkg`

## Mod

