---
title: Webpack
date: 2017-10-16 15:02:14
tags:
---

webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成少量的 bundle - 通常只有一个，由浏览器加载。

1.安装webpack 

新建一个文件夹webpack

	npm init

	此时最好把package.json中的name改掉，不能是webpack，否则局部安装webpack时会报错。改为webpack-test。

	npm install webpack --save-dev

用编辑器打开	

npm install css-loader style-loader --save-dev


2.webpack使用

入口:告诉 webpack 从哪里开始，并根据依赖关系图确定需要打包的内容。可以将应用程序的入口起点认为是根上下文(contextual root) 或 app 第一个启动文件。

出口:将所有的资源(assets)归拢在一起后，还需要告诉 webpack 在哪里打包应用程序。webpack 的 output 属性描述了如何处理归拢在一起的代码(bundled code)。

Loader:webpack 把每个文件(.css, .html, .scss, .jpg, etc.) 都作为模块处理。然而 webpack 自身只理解 JavaScript。webpack loader 在文件被添加到依赖图中时，其转换为模块。(loader的作用：1.识别需要转换的文件，2.转换这些文件，从而使这些文件添加到依赖图)。

插件:webpack 的插件系统极其强大和可定制化。要使用一个插件，需要先require它，再把它添加到plugin数组中。

下面是webpack.config.js:

	const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
	const webpack = require('webpack'); //to access built-in plugins
	const path = require('path');

	const config = {
	  entry: './path/to/my/entry/file.js',
	  output: {
	    path: path.resolve(__dirname, 'dist'),
	    filename: 'my-first-webpack.bundle.js'
	  },
	  module: {
	    rules: [
	      { test: /\.txt$/, use: 'raw-loader' }
	    ]
	  },
	  plugins: [
	    new webpack.optimize.UglifyJsPlugin(),
	    new HtmlWebpackPlugin({template: './src/index.html'})
	  ]
	};
	module.exports = config;


#### 入口

单个入口：

	const config = {
	  entry: {
	    main: './path/to/my/entry/file.js'
	  }
	};

多个入口（对象形式）：

	const config = {
	  entry: {
	    app: './src/app.js',
	    vendors: './src/vendors.js'
	  }
	};

分离 应用程序(app) 和 第三方库(vendor) 入口。（或者多页面应用程序）


#### 输出output

即使可以存在多个入口起点，但只指定一个输出配置。

在 webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包括以下两点：

1. filename 用于输出文件的文件名。
2. 目标输出目录 path 的绝对路径。

	const config = {
		output: {
			path: '/home/proj/public/assets',
			filename: 'bundle.js'
		}
	}

如果有多个入口，应该使用占位符(substitutions)来确保每个文件具有唯一的名称：

	{
		entry: {
			app: './src/app.js',
	    	vendors: './src/vendors.js'
		}
		output: {
			filename: '[name].js',
			path: __dirname + bundle.js
		}
	}
	// 写入到硬盘：./dist/app.js, ./dist/search.js


#### loader

对模块的源代码进行转换。

loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL。loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！	
	module.exports = {
		entry: {
			...
		},
		output: {
			...
		},
		module: {
			rules:[
				{test: /\.css$/, use: 'css-loader'},
				{test:/\.ts$/, use: 'ts-loader'}
			]
		}
	}

loader的使用方法有三种：

1.推荐配置webpack.config.js

		module: {
	    rules: [
	      {
	        test: /\.css$/,
	        use: [
	          { loader: 'style-loader' },
	          {
	            loader: 'css-loader',
	            options: {
	              modules: true
	            }
	          }
	        ]
	      }
	    ]
	  }

2.内联

	import Styles from 'style-loader!css-loader?modules!./styles.css';

3.CLI(webpack命令行借口)

	webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'

#### 插件

插件是webpack的支柱功能，目的是解决loader无法实现的事情。

用法：

webpack 配置中，向 plugins 属性传入 new 实例。

	plugins: [
	    new webpack.optimize.UglifyJsPlugin(),
	    new HtmlWebpackPlugin({template: './src/index.html'})
	  ]


#### 配置

1.通过require(...)导入其他文件
2.通过require(...)使用npm的工具函数
3.使用JS的控制流表达式
4.对常用值使用常量或者变量
5.编写并执行函数来生成部分配置


#### 模块

在模块化编程中，开发者将程序分解成离散功能块，称为模块。

