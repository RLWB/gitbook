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

### 5、配置webpack

 真实工作中，我们需要对webpack进行各种配置，这个时候我们就需要创建webpack.config.js进行配置：

 创建webpack.config.js

```bash
touch webpack.config.js
```

webpack是用js编写的，运行在nodejs环境里。配置文件中我们需要通过module.exports导出配置项，这是nodejs的Common.js规范规定的。

```javascript
module.exports = {
  //配置项
};
```

在webpack.config.js里面，我们通过设置以下属性，去指定webpack的打包行为：

* 入口
* 出口
* loader
* plugin
* 代码分割

例如，要去改变打包的入口，我们可以这样做:

```javascript
const path = require("path");

module.exports = {
  entry: { index: path.resolve(__dirname, "source", "index.js") }
};
```

这样的话，webpack就会寻找source/index.js作为打包的入口文件去加载。同样，要想改变打包的输出文件目录，我们可以这样做：

```javascript
const path = require("path");

module.exports = {
  output: {
    path: path.resolve(__dirname, "build")
  }
};
```

这样的话，webpack就会把bundle输出到build目录，替换默认的dist目录。

### 6、处理HTML

web应用离不开HTML。想要让webpack解析HTML。我们需要用到一个插件，叫做html-webpack-plugin:

```bash
npm i html-webpack-plugin --save-dev
```

安装好插件，配置如下：

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
```

上面，我们告诉webpack，从src/index.html去加载HTML模板。

html-webpack-plugin的最终目标是双重的：

* 加载我们的HTML文件
* 将bundle注入到HTML文件

接下来，我们创建一个简单的HTML在src/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Webpack tutorial</title>
</head>
<body>

</body>
</html>
```

稍后我们将使用webpack的开发服务器运行这个应用程序。

### 7、webpack的dev server

开头的时候，我们安装了webpack-dev-server。如果没有安装的话，可以安装一下：

```bash
npm i webpack-dev-server --save-dev
```

**webpack-dev-server**是一个方便的开发包。配置完成后，我们可以启动本地服务器来为我们的文件提供服务。

要配置**webpack-dev-server**，打开`package.json`并添加一个“启动”脚本：

```json
  "scripts": {
    "dev": "webpack --mode development",
    "start": "webpack serve --open 'Chrome'",
  },
```

启动dev-server，运行下面命令：

```bash
npm start
```

Chrome浏览器应该会被打开。控制审查元素，可以看到，main.js被注入到我们的index.html页面。

![webpack 开发服务器](https://www.valentinog.com/blog/static/06dee181263809dde67ecf0e4c4d8d01/7a3d6/webpack-dev-server.png)

### 8、使用webpack的loader

loader是第三方扩展可以帮助webpacl处理不同类型的文件。比如常见的loader有处理css样式文件，处理images图片文件和文本文件。

loader的配置基本如下：

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.filename$/,
        use: ["loader-b", "loader-a"]
      }
    ]
  },
  //
};
```

通过在module对象里，插入rules，去编写loader的配置。

对于作为模块处理的每个文件，使用test，指定要处理的文件，使用use去指定loader：

```javascript
{
    test: /\.filename$/,
    use: ["loader-b", "loader-a"]
}
```

test告诉webpack，把符合文件名的文件作为一个模块。use告诉webpack使用对应的loader去解析模块。

### 9、使用css

要在webpack里面使用css，我们需要至少安装两个loader。这里的loader是用来帮助webpack理解如何处理.css文件所必须的。

为了测试，我们先创建css文件src/style.css：

```css
h1 {
    color: orange;
}
```

在src/index.html添加如下代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Webpack tutorial</title>
</head>
<body>
<h1>Hello webpack!</h1>
</body>
</html>
```

在src/index.js添加如下代码：

```javascript
import "./style.css";
console.log("Hello webpack!");
```

安装如下loader：

```bash
npm i css-loader style-loader --save-dev
```

按照下面配置webpack.config.js

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
```

现在，如果您运行，`npm start`您应该会看到加载在 HTML 头部的样式表

![在 webpack 中使用 CSS](https://www.valentinog.com/blog/static/107120eb641df48da9625bf147954004/7a3d6/webpack-css-loader.png)

如果需要单独提取css文件，你可以使用MiniCssExtractPlugin这个插件。

<b>注意webpack loader的顺序很重要：</b>

以下配置是无效的：

```javascript
//

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["css-loader", "style-loader"]
      }
    ]
  },
  //
};
```

以下配置是有效的：

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  //
};
```

**webpack loader从右到左加载，（或从上到下）。**

### 10、使用SASS

要在webpack中使用SASS，我们需要安装适当的loader。

要在webpack中测试SASS，我们首先创建一个简单的样式表src/style.scss:

```scss
$primary-color: #eee;

body {
  color: $primary-color;
}
```

将SASS文件加载到src/index.js:

```javascript
import "./style.scss";
console.log("Hello webpack!");
```

接下来我们就需要安装对应的loader（以及sass包）

- **sass-loader**用于加载`import`的 SASS 文件
- 用于将 CSS 文件加载为模块的**css-loader**
- **style-loader**用于在 DOM 中加载样式表

```bash
npm i css-loader style-loader sass-loader sass --save-dev
```

然后将它们配置在`webpack.config.js`

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "src", "index.html")
    })
  ]
};
```

[https://www.valentinog.com/blog/webpack/]



