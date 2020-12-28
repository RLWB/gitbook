### 一、Webpack简介

#### 1.1 webpack是什么

webpack 是一种前端资源构建工具，一个静态模块打包器(module bundler)。 

在 webpack 看来, 前端的所有资源文件(js/json/css/img/less/...)都会作为模块处理。 

它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源(bundle)。

#### 1.2 webpack的五个核心

##### 1.2.1 Entry

入口(Entry)指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

##### 1.2.2 Output

输出(Output)指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名。

**1.2.3 Loader** 

Loader 让 webpack 能 够 去 处 理 那 些 非 JavaScript 文 件 (webpack 自 身 只 理 解 

JavaScript)

**1.2.4 Plugins** 

插件(Plugins)可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩， 

一直到重新定义环境中的变量等。

**1.2.5 Mode** 

模式(Mode)指示 webpack 使用相应模式的配置。 

|    选项     |                             描述                             |            特点             |
| :---------: | :----------------------------------------------------------: | :-------------------------: |
| development | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 | 能让代码本地调试 运行的环境 |
| production  | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。 | 能让代码优化上线 运行的环境 |

 ### 二、Webpack的初体验

####  2.1 初始化配置

##### 2.1.1 初始化 package.json 

输入指令: 

```
npm init -y
```

##### 2.1.2 下载并安装webpack

输入指令：

```
npm i webpack webpack-cli -D
```

#### 2.2 编译打包应用

##### 2.2.1. 创建文件 

##### 2.2.2. 运行指令 

开发环境指令：

```
webpack src/js/index.js -o build/js/built.js --mode=development 
```

功能：

​		webpack 能够编译打包 js 和 json 文件，并且能将 es6 的模块化语法转换成 

浏览器能识别的语法。 

生产环境指令：

```
webpack src/js/index.js -o build/js/built.js --mode=production
```

功能：

​		在开发配置功能上多一个功能，压缩代码。 

##### 2.2.3. 结论

webpack 能够编译打包 js 和 json 文件。 

能将 es6 的模块化语法转换成浏览器能识别的语法。 

能压缩代码。 

##### 2.2.4. 问题

不能编译打包 css、img 等文件。 

不能将 js 的 es6 基本语法转化为 es5 以下语法。

### 三、webpack开发环境的基本配置

#### 3.1  创建配置文件

##### 3.1.1 创建文件 webpack.config.js

##### 3.1.2 配置内容如下

```
const { resolve } = require('path'); // node 内置核心模块，用来处理路径问题。
module.exports = { 
    entry: './src/js/index.js', // 入口文件 
    output: { // 输出配置 
        filename: './built.js', // 输出文件名 
        path: resolve(__dirname, 'build/js') // 输出文件路径配置 
    },
    mode: 'development' //开发环境 
}
```

##### 3.1.3 package.json添加启动脚本

```
"script": {
	"start": "webpack --config webpack.config.json"
}
```

##### 3.1.4. 运行指令: 

```
npm run start
```

##### 3.1.5. 结论: 此时功能与上节一致

#### 3.2 打包样式资源

##### 3.2.1 创建文件

​		css,less

##### 3.2.2 下载loader包

```
npm i css-loader style-loader less-loader less -D
```

##### 3.2.3 修改配置文件

```
const { resolve } = require('path'); 
module.exports = { // webpack 配置 // 入口起点 
	entry: './src/index.js', // 输出 
	output: { // 输出文件名 
		filename: 'built.js', // 输出路径 // __dirname nodejs 的变量，代表当前文件的目录绝对路径 
		path: resolve(__dirname, 'build')
	},
	// loader 的配置 
	module: { 
		rules: [ 
			// 详细 loader 配置 
			// 不同文件必须配置不同 loader 处理 
			{ 
				// 匹配哪些文件 
				test: /\.css$/, // 使用哪些 loader 进行处理 
				use: [ 
					// use 数组中 loader 执行顺序：从右到左，从下到上 依次执行 
					// 创建 style 标签，将 js 中的样式资源插入进行，添加到 head 中生效 					'style-loader', 
					// 将 css 文件变成 commonjs 模块加载 js 中，里面内容是样式字符串 
					'css-loader' 
				] 
			},
			{ 
				test: /\.less$/, 
				use: [ 
					'style-loader', 
					'css-loader', 
					// 将 less 文件编译成 css 文件 
					// 需要下载 less-loader 和 less 'less-loader' 
					 ] 
				  } 
				] 
			},
			// plugins 的配置 
			plugins: [ 
				// 详细 plugins 的配置 
			],
			// 模式 
			mode: 'development', // 开发模式 
			// mode: 'production' 
		}
```

##### 3.2.4运行指令

```
npm run start
```

#### 3.3 打包HTML资源

##### 3.3.1 创建文件

html

##### 3.3.2 下载安装plugin包

```
npm install --save-dev html-webpack-plugin
```

##### 3.3.3 修改配置文件

```
const { resolve } = require('path'); 
const HtmlWebpackPlugin = require('html-webpack-plugin'); 
module.exports = { 
    entry: './src/index.js', 
    output: { 
        filename: 'built.js', 
        path: resolve(__dirname, 'build') 
    },
    module: { 
        rules: [ 
            // loader 的配置 
        ] 
    },
    plugins: [ 
        // plugins 的配置 
        // html-webpack-plugin 
        // 功能：默认会创建一个空的 HTML，自动引入打包输出的所有资源（JS/CSS） 
        // 需求：需要有结构的 HTML 文件 
        new HtmlWebpackPlugin({ 
            // 复制 './src/index.html' 文件，并自动引入打包输出的所有资源（JS/CSS） 
            template: './src/index.html' 
        }) 
    ],
    mode: 'development' 
};
```

##### 3.3.4 运行指令

```
npm run start
```

