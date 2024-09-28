前端构建工具
===

[TOC]


https://www.bilibili.com/video/BV1Kd4y147gg

github：https://github.com/lilichao/vue-course
gitee：https://gitee.com/ymhold/vue-course

https://gitee.com/Yang-Rachel/learning-notes/blob/master/%E5%85%B6%E4%BB%96%E7%9F%A5%E8%AF%86%E7%AC%94%E8%AE%B0/%E6%9E%84%E5%BB%BA%E5%B7%A5%E5%85%B7.md

---

ES6 的 import, require 语法比 `<script>` 标签写起来爽多了，于是我们不想在浏览器写原始 js

打包工具有两个任务
- 合并 ES6  模块，减少浏览器请求次数
- 降级 js 语法

目前主流的打包工具有 webpack 和 vite。webpack 老一点、复杂一点

## webpack
![](https://notes.sjtu.edu.cn/uploads/upload_602c94884c9a294c8c318d44e8b4c2a5.png)

依赖包
- webpack
- webpack-cli（命令行工具：提供命令行操作命令）



运行 webpack，默认从 `./src/index.js` 打包；会自动构建调用图并 prune 无用的 package。
```bash=
yarn webpack
```

### 配置文件
配置文件是 webpack.config.js。

#### loader
在 webpack 中，默认只打包 js。为了实现将 css, img 插入打包后的 html，我们需要把 css、img 全部交给 webpack 打包

为此，在配置文件中需要引入 loader 加载器。loader 可以将一种代码转换为另一种代码（例如 babel；例如 css 转换成 js 运行时嵌入）

> css-loader 的实现方式，是 src 中类似导包写 require('xxx.css')，然后编译器在 js 中添加插入把 css 插入 html 的代码（运行时）。

```jsx=
// node 环境
module.exports = {
    mode: "production", 
    entry: { // 可以使用 [] 打包到一个里面；{} 打包多个入口
        demo : "./src/index.js",
    },
    output: {
        path: path.resolve(__dirname, "dist"),
        filename: "[name]-[id].js",
        clean: true
    },
    module: { // loaders
        // see asset management https://webpack.js.org/guides/asset-management/#loading-images
        rules: [
            {
                test: /\.css$/i,
                use: ["style-loader", "css-loader"], // wierd: 倒序的    
            },
            {
                test: /\.img$/i,
                type: "asset/resource", // 相当于内置 resloader
            }
        ]
    }
    
    
}
```

#### plugin
loader 扩展的是 webpack 处理文件的能力；plugin 则可以扩展 webpack 的功能。
- 构建时，清空目录后，自动在 dist 目录创建 index.html
```jsx=
const DemoHTMLPlugin = require("html-webpack-plugin")
...
{
    plugins: [new DemoHTMLPlugin({
        template: "./src/index.html"
    })]
}
```

 

#### 调试 line-map
知道有这么个事就行


## vite
vite 是一个比 webpack 速度更快，使用更方便的构建工具
- 调试模式下，不打包、而是直接采用 ES Module 运行
- 构建依赖时，使用 go 编写的 esbuild 比 js 更快
- 仅在 build 部署时，打包到 dist 下

vite 会自动查找 index.html，自动解析其中引入的 module js，然后进行编译（可以直接在 js 中导入 css ）


### vite 配置文件
vite 的配置文件是 vite.config.js。类似 webpack 通过 module.exports 的方式暴露对象进行配置，vite 通过 ES6 的 export 进行导包

```jsx=
import {defineConfig} from "vite" // identity function，写了之后有代码提示

export default {
    plugins: [legacy()]
}


```

## Web manifest
构建工具都会留一个 manifest.json。用于让后续工具能找到编译后名字有变化的文件。    