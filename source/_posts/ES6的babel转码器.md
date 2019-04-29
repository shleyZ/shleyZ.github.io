---
title: ES6的babel转码器
date: 2017-10-14 09:43:06
categories: javascript
---

#### 我使用的是Mac系统，webstorm，在webstorm上创建一个新的项目ES6，安装babel。（首先要安装node）

#### 1.在ES6项目下执行

	npm init -y (根据权限不同，有的时候前面需要加sudo：sudo npm init -y，然后输入管理员密码)

这样是为了生成一个package.json的文件

#### 2.本地安装babel命令行工具，这样就可以命令行使用babel。

babel命令行工具可以全局安装也可本地安装，不过官方推荐本地安装，原因有两点：

	本地安装不同的项目可以使用不同版本的babel，你也可以单独对某个项目的babel进行升级等操作  
	每个项目单独安装的意味着对计算机环境没有依赖，便于移植

安装：

	npm install --save-dev babel-cli

这时项目里面又了node_modules文件夹

如果你已经全局安装了babel-cli,可以使用下面的命令卸载：

	npm uninstall --global babel-cli

这时的package.json文件如下：

``` json

	{
	  "name": "ES6-babel",
	  "version": "1.0.0",
	  "description": "",
	  "main": "index.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "keywords": [],
	  "author": "",
	  "license": "ISC",
	  "devDependencies": {
	    "babel-cli": "^6.26.0"
	  }
	}

```

#### 3.在命令行中调用babel

本地安装的babel是不能够在直接命令行中运行的，为了在命令行中运行babel，有两种方法，不过推荐第一种：

方法一：配置package.json文件的scripts项（在script中添加 "build": "babel src -d lib","babel": "babel"）

``` json

	{
	  "name": "ES6-babel",
	  "version": "1.0.0",
	  "description": "",
	  "main": "index.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1",
	    "build": "babel src -d lib",
	    "babel": "babel"
	  },
	  "keywords": [],
	  "author": "",
	  "license": "ISC",
	  "devDependencies": {
	    "babel-cli": "^6.26.0"
	  }
	}

```	

方法二：进入node_modules文件夹，再进入.bin文件夹，然后执行在命令行中执行babel src -d lib。

#### 4.创建 .babelrc 配置文件

	type nul>.babelrc

或者：

	mkfile -n 1m .babelrc

都能创建.babelrc文件	

#### 5.安装babel转译器

把ES6代码转换为ES5的代码必须要用转译器，装好需要的转译器还需要在babelrc进行配置这些转译器(presets)选项。

下面是一些转译器，可以选择一个或多个，根据自己需要安装：

	##ES2015转码规则
	npm install --save-dev babel-preset-es2015
	 
	##react转码规则
	npm install --save-dev babel-preset-react
	 
	##ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
	npm install --save-dev babel-preset-stage-0
	npm install --save-dev babel-preset-stage-1
	npm install --save-dev babel-preset-stage-2
	npm install --save-dev babel-preset-stage-3

	

#### 6.配置.babelrc文件

.babelrc用于配置除回调以外的所有babel api 选项。例如plugins和presets。plugins用于配置我们转译所需要的插件，presets用于配置我们所需要的转译器。

添加：

``` json

	{
	    "presets": [
	      "es2015"
	    ],
	    "plugins": []
		}
		
```		

#### 7.这样我们就可以来转码了

由于"build": "babel src -d lib",所以我们要创建src文件夹，里面是要转码的es6内容

例如在 ES6/src/es6.js里面添加如下内容：

	input.map(item => item + 1);

执行命令：

	npm run build

就会自动生成一个lib文件夹，里面是已经转码好的内容：

	"use strict";

	input.map(function (item) {
	  return item + 1;
	});		




