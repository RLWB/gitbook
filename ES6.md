## ES6知识点

> ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

### 简介

#### ECMAScript 和 JavaScript的关系

ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 方言还有 JScript 和 ActionScript）。日常场合，这两个词是可以互换的。

#### ES6 与 ECMAScript 2015 的关系

ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。本书中提到 ES6 的地方，一般是指 ES2015 标准，但有时也是泛指“下一代 JavaScript 语言”。

#### Babel 转码器

使用ES6代码进行开发的时候，我们需要考虑老版本浏览器的 兼容性问题，所以，当我们的项目打包上线的时候需要对将ES6代码转译为ES5或者ES3的代码，通常我们会使用Babel转码器插件。当我们使用vue脚手架或者webpack去搭建项目进行开发的时候，可以针对性的去配置babel插件。

### 一、let和const命令



