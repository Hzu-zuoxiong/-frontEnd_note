# Markdown表示文件目录

在编写README.md文件时，经常需要列出整个项目的文件目录结构。

## 方法1

使用一个三重反引号（```）将目录结构包起来：

![](/assets/tools/generateFileDir1.png)

## 方法2

使用一个 `Node` 模块来生成文件目录: `mddir`

例子： `cd` 进入 `mddir/src` 目录进行操作

```
// 安装依赖
npm install mddir --save-dev
// 进入 mddir/src 目录
cd node_modules/mddir/src
// 执行命令
node mddir '../../../'

// 最后会在 middir/src 文件夹下生成directoryList.md文件
// 目前忽略 node-modules 和 .git 文件夹

```