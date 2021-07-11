## webpack5教程

### 1、什么是webpack

本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 *bundle*。

![image-20210706234258963](./imgs/image-20210706234258963.png)

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
@import url("https://fonts.googleapis.com/css?family=Karla:weight@400;700&display=swap");

$font: "Karla", sans-serif;
$primary-color: #3e6f9e;

body {
  font-family: $font;
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

注意**loader的出现顺序**：首先是sass-loader，然后是css-loader，最后是style-loader。

现在如果运行，npm start 就会看到如下样式：

![在 webpack 中使用 SASS](https://www.valentinog.com/blog/static/28f360788e76a4b957456842642c32c8/7a3d6/webpack-sass.png)

### 11、使用ES6+

webpack并不会自动转换任何js代码。需要用到babel-loader和babel。

babel是一个js编译器、转译器。可以将ES6+转换为几乎可以在任何浏览器运行的兼容代码（ES5或者ES3等）。

我们需要安装以下包：

- **babel/core**，实际的引擎
- **@babel/preset-env**用于将ES6+编译为 ES5 的
- **babel-loader**用于 webpack 

```bash
npm i @babel/core babel-loader @babel/preset-env --save-dev
```

然后通过创建**babel.config.json**文件来配置babel，这里我们将babel配置为使用preset-env：

```json
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

最后，配置如下：

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ["style-loader", "css-loader", "sass-loader"]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ["babel-loader"]
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

要测试有没有配置生效，编写如下ES6代码src/index.js:

```javascript
import "./style.scss";
console.log("Hello webpack!");

const fancyFunc = () => {
  return [1, 2];
};

const [a, b] = fancyFunc();
```

现在运行`npm run dev`以查看`dist`. 打开`dist/main.js`并搜索“fancyFunc”：

```text
\n\nvar fancyFunc = function fancyFunc() {\n  return [1, 2];\n};\n\nvar _fancyFunc = fancyFunc(),\n    _fancyFunc2 = _slicedToArray(_fancyFunc, 2),\n    a = _fancyFunc2[0],\n    b = _fancyFunc2[1];\n\n//# sourceURL=webpack:///./src/index.js?"
```

没有 babel，代码不会被转译：

```text
\n\nconsole.log(\"Hello webpack!\");\n\nconst fancyFunc = () => {\n  return [1, 2];\n};\n\nconst [a, b] = fancyFunc();\n\n\n//# sourceURL=webpack:///./src/index.js?"); 
```

### 12、在webpack中使用ES module

webpack工作时汇报所有文件都视为模块。但是，我们不要忘了它的主要目的：加载ES module。

在2015年之前，js是没有标准的代码重用机制。人们尝试将这方面标准化，结果造成了混乱的碎片化。

我们经常会听到AMD模块、UMD或者common js。这些都是社区实现的规范。直到ECMAScript 2015，ES模块的出现，才算是官方正式的模块系统。

webpack可以让ES module正常的运行在浏览器中。

要在 webpack 中试用 ES 模块，让我们在一个新文件中`src/common/usersAPI.js`使用以下代码创建一个模块：

```javascript
const ENDPOINT = "https://jsonplaceholder.typicode.com/users/";

export function getUsers() {
  return fetch(ENDPOINT)
    .then(response => {
      if (!response.ok) throw Error(response.statusText);
      return response.json();
    })
    .then(json => json);
}
```

现在`src/index.js`您可以加载模块并使用该功能：

```javascript
import { getUsers } from "./common/usersAPI";
import "./style.scss";
console.log("Hello webpack!");

getUsers().then(json => console.log(json));
```

### 13、生产模式

前面介绍过，webpack有两种运行模式：development和production。到目前为止，我们只是在开发模式下工作。

在开发模式下，webpack获取我们编写的原始的js代码，并将其加载到浏览器中。

不会主动去压缩代码。这使得每次更改代码，应用程序加载更快。

在生产模式下，webpack应用许多优化：

- 使用 TerserWebpackPlugin 缩小以减小包大小
- 使用 ModuleConcatenationPlugin 提升作用域

还是这`process.env.NODE_ENV`为'production'。这个环境变量对于在生产或者开发中有条件地做事情很有用。

要在生产模式下配置 webpack，请打开`package.json`并添加一个“build”脚本：

```json
 "scripts": {
    "dev": "webpack --mode development",
    "start": "webpack serve --open 'Firefox'",
    "build": "webpack --mode production"
  },
```

现在运行`npm run build`webpack 时会产生一个缩小的包。

### 14、使用webpack进行代码拆分

code spliting是一种优化技术，旨在：

- 避免大包
- 避免依赖重复

webpack中代码拆分有以下三种方式：

- 配置多个入口点
- 使用`optimization.splitChunks`
- 使用动态导入

第一种基于多个入口点的技术适用于较小的项目，但从长远来看它不具有可扩展性。在这里，我们将只关注`optimization.splitChunks`和动态导入。

**使用 optimization.splitChunks 进行代码拆分**

例如我们在js项目中，使用Moment.js。

安装：

```bash
npm i moment
```

src/index.js编写如下代码：

```javascript
import moment from "moment";
```

运行构建`npm run build`并查看输出：

```text
   main.js    350 KiB       0  [emitted]  [big]  main
```

默认情况下，整个momentjs都被捆绑到我们的入口文件。

通过使用 optimization.splitChunks 我们可以把momentjs从main bundle移出。

配置如下：

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  module: {
  // omitted for brevity
  },
  optimization: {
    splitChunks: { chunks: "all" }
  },
  // omitted for brevity
};
```

执行`npm run build`:

```text
    main.js   5.05 KiB       0  [emitted]         main
vendors~main.js    346 KiB       1  [emitted]  [big]  vendors~main
```

这样的话，就让主入口文件的大小更合理。

**注意：**即使使用了code spliting，momentjs仍是一个巨大的库。我们最好用dayjs代替它。

**使用动态导入进行代码拆分**

动态导入是更强大的代码拆分技术，以此可以有条件的加载代码。ECMAScript2020中提供此功能。webpack在此之前就提供了动态导入。

这种方法在现代前端库中被广泛使用，比如Vue和React。

可以使用代码拆分的场景：

- 在模块级别
- 在路由级别

例如，您可以有条件的加载一些js模块，比如单击或者移动鼠标。

或者，您可以在响应路由更改时，加载代码的相关部分。

修改src/index.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Dynamic imports</title>
</head>
<body>
<button id="btn">Load!</button>
</body>
</html>
```

保持src/common/usersAPI.js:

```javascript
const ENDPOINT = "https://jsonplaceholder.typicode.com/users/";

export function getUsers() {
  return fetch(ENDPOINT)
    .then(response => {
      if (!response.ok) throw Error(response.statusText);
      return response.json();
    })
    .then(json => json);
}
```

src/index.js创建如下逻辑：

```javascript
const btn = document.getElementById("btn");

btn.addEventListener("click", () => {
  //
});
```

如果运行npm run start查看并单击界面按钮，则不会发生任何事情。

如果要实现单击按钮加载用户列表。在src/common/usersAPI.js内，正常我们会编写如下代码：

```javascript
import { getUsers } from "./common/usersAPI";

const btn = document.getElementById("btn");

btn.addEventListener("click", () => {
  getUsers().then(json => console.log(json));
});
```

问题在于ES module是静态的，这意味着我们无法在运行时更改导入。

使用动态导入，我们可以选择何时加载我们的代码：

```javascript
const getUserModule = () => import("./common/usersAPI");

const btn = document.getElementById("btn");

btn.addEventListener("click", () => {
  getUserModule().then(({ getUsers }) => {
    getUsers().then(json => console.log(json));
  });
});
```

里我们创建了一个函数来动态加载模块：

```javascript
const getUserModule = () => import("./common/usersAPI");
```

然后在事件侦听器中，我们链接`then()`到动态导入：

```javascript
btn.addEventListener("click", () => {
  getUserModule().then(/**/);
});
```

这使我们能够通过对象解构来提取`getUsers`函数：

```javascript
btn.addEventListener("click", () => {
  getUserModule().then(({ getUsers }) => {
    //
  });
});
```

最后，我们像往常一样使用`getUsers`函数：

```javascript
btn.addEventListener("click", () => {
  getUserModule().then(({ getUsers }) => {
    getUsers().then(json => console.log(json));
  });
});
```

当您第一次加载页面时，`npm run start`您会看到在控制台中加载的主包：

![主包已加载](https://www.valentinog.com/blog/static/dfbcf49bdf273505b85d3f9264406d59/7a3d6/main-bundle-loaded.png)

现在**“./common/usersAPI”仅在单击按钮时加载**：

![动态导入 webpack](https://www.valentinog.com/blog/static/dfbe402ce1ad36d9e1fad066b7f0060b/7a3d6/dynamic-import-webpack.png)

动态加载的 chunk 就是0.js

通过在导入路径前加上前缀，`/* webpackChunkName: "name_here" */`我们还可以控制块名称:

```javascript
const getUserModule = () =>
  import(/* webpackChunkName: "usersAPI" */ "./common/usersAPI");

const btn = document.getElementById("btn");

btn.addEventListener("click", () => {
  getUserModule().then(({ getUsers }) => {
    getUsers().then(json => console.log(json));
  });
});
```

![网络包块名](https://www.valentinog.com/blog/static/a8ad13d04b27f3986b40fba271dda85e/7a3d6/webpackChunkName.png)

