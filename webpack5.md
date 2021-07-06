## webpack5教程

### 1、什么是webpack

本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 *bundle*。

![image-20210706234258963](D:\code\gitbook\imgs\image-20210706234258963.png)

webpack可以将所有的依赖都通过JavaScript导入，并且最终将这些依赖打包输出成浏览器识别的静态资源（html,css,js等等）



### 2、为什么学习webpack

reactjs的脚手架create-react-app 或者 vuejs脚手架 vue-cli这些工具都是通过webpack进行封装的，并且提供了单独的配置文件，让我们无需学习webpack也可以快速配置脚手架。但是，即便如此，我们还是有必要去学习其幕后的工作原理，因为有的时候默认脚手架的配置可能是无法满足我们的开发需求的，这个时候我们就得去对webpack配置进行修改，另一方面，身为合格的前端开发工程师，我们必须要掌握造轮子的技术，所以我们必须要好好学习webpack。

### 3、webpack的核心

* entry（入口）

  ​    	webpack的入口是手机前端项目所有依赖的起点。实际上，它就是一个简单的js文件。这些依赖形成了一个以来图。从webpack4开始，默认的入口就是src/index.js,当然我们可以修改默认的入口。webpack也可以有多个入口。

* output（出口）

  ​		出口是指定webpack生成js和静态文件的地方。webpack4开始默认输出文件夹是dist/,也是可以配置的。

  生成的js文件文件称为bundle。

* loaders（加载器）

  ​		loader是第三方扩展，是专门帮助webpack处理各种类型的文件。比如有用于css，图片，sass或者字体文件的loader。

  ​		loader的目标就是转换模块里面的文件。一旦文件作为模块导入，webpack就可以把它作为依赖，在你的项目中使用。

* plugins（插件）

  ​		插件也是第三方扩展，可以改变webpack的工作方式。例如有用于提取HTML，CSS或者设置环境变量的插件。

* mode（模式）

  ​		webpack有两种操作模式：development和production。主要区别在于生产模式会自动压缩并且优化你的项目js代码。

* code splitting（代码拆分）

  ​		代码拆分是减小单个包体积过大的优化技术。

  ​		通过代码拆分。开发者可以决定何时去加载js代码块。比如说路由改变或者是点击事件的时候。

  ​		被拆分的代码片段会变成单独的chunk。

### 4、开始

首先创建一个项目文件夹，并且进入项目文件夹初始化一个npm项目。

```bash
mkdir webpack-tutorial && cd $_

npm init -y
```

安装 **webpack, webpack-cli, and the webpack-dev-server**:

```bash
npm i webpack webpack-cli webpack-dev-server --save-dev
```

通过npm script运行webpack，打开package.json文件，配置dev命令：

```json
  "scripts": {
    "dev": "webpack --mode development"
  },
```

通过这个脚本，我们设置webpack为开发模式，方便开发调试。

在开发模式下运行webpack：

```bash
npm run dev
```

报错如下：

```bash
ERROR in main
Module not found: Error: Can't resolve './src' in 'D:\code\webpack5-demo'
```

这里webpack默认会寻找入口，src/index.js

```bash
mkdir src

echo 'console.log("Hello webpack!")' > src/index.js
```

再次运行 npm run dev，此时应该就不会报错了，并且会生成dist文件夹，包含有main.js

```bash
dist
└── main.js
```

main.js就是第一个webpack bundle,也叫做output

[https://www.valentinog.com/blog/webpack/]



