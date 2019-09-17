---
layout: post
title: "搭建前端应用（webpack+babel+ES6+react环境搭建）"
description: "一个简单的入门"
categories: [前端]
tags: [react, webpack, babel]
redirect_from:
  - /2019/09/17/
---

* toc
{:toc}

#### 1.新建项目目录 react-demo
~~~
mkdir  app    //创建app目录 用来存放项目源文件
mkdir  dist   // 创建dist目录  用来存放打包好的文件
touch  .gitignore  //创建.gitignore  用来添加git 忽略的文件
touch   webpack.config.js   //创建webpack的配置文件
cd  app    //进入到app目录  
touch  index.js  //在app目录中创建 index文件  入口文件
~~~
#### 2.初始化项目
~~~ javascript
npm init -y
~~~
#### 3.安装相关依赖
~~~ javascript
//安装babel
npm install -D babel-loader @babel/core @babel/preset-env
//安装babel扩展插件jsx，从而支持jsx语法
npm install --save-dev @babel/plugin-transform-react-jsx 
//安装react支持
npm install react react-dom babel-preset-react --save
//安装样式支持
npm install css-loader style-loader --save
//安装webpack
npm install --save-dev webpack
//安装webpack-cli
npm install --save-dev webpack-cli
//安装webpack-dev-server
npm install --save-dev webpack-dev-server
~~~

#### 4.修改package.json
~~~ javascript
// 修改package.json 的文件中的scripts
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --open"
}
~~~
#### 5.项目的根目录中新增.babelrc文件
~~~ javascript
// 文件内容如下：
{ "plugins": ["@babel/plugin-transform-react-jsx"] }
~~~
#### 6.编写html、js、css文件
  1. 在dist目录下，新增index.html

   {% highlight html %}
    <!DOCTYPE html>
    <htmllang="en">
    <head>
        <metacharset="UTF-8">
    <title>首页</title>
    </head>
    <body>
        <div id="box"></div>
    </body>
    <script src="app.bundle.js"></script>
    </html>
    {% endhighlight %}


  2. 修改app目录下的index.js文件

   {% highlight javascript %}
    import React from 'react';
    import ReactDOM from 'react-dom';
    class IndexComponent extends React.Component{
        render(){
            return <h1>hello world!!!</h1>
        }
    }
    var oBox = document.getElementById("box");
    ReactDOM.render(<IndexComponent/>,oBox)
    {% endhighlight %}


  3. 在app目录下新增index.css文件
    ~~~ css
    h1{
        color: green;
    }
    ~~~


#### 7.修改webpack.config.js，具体内容如下
~~~ javascript
module.exports = {
    mode: 'development',//仅用于开发环境
    context: __dirname + "/app", //源文件目录
    entry: {
        app: "./index.js" //在源文件目录下去找index.js 文件作为打包的入口文件
    },
    devtool: 'inline-source-map',//显示源码，方便排查
    devServer: {
        contentBase: __dirname + "/dist" //将 dist 目录下的文件 serve 到 localhost:8080 下
    },
    output: {
        path: __dirname + "/dist", //生成的文件存放目录
        filename: "[name].bundle.js" //生成的文件 name 表示entry下面的app
    },
    module: {
        rules: [
                    {
                        test: /\.m?js$/,
                        exclude: /(node_modules|bower_components)/,
                        use: {
                                loader: 'babel-loader',
                                options: {
                                            presets: ['@babel/preset-env']
                                         }
                              }
                     },
                     {
                         test: /\.css$/,
                         use: ["style-loader", "css-loader"],
                     },

                ]
            }
}
~~~

#### 8.运行npm start

![helloWold](/_posts/image/ReactHelloWolrd.png)

#### 9.我的理解
> * babel 就是用来把一些高级的语言转化成普通的语言，它有很多插件，比如 plugin-transform-react-jsx  用来将 jsx 转 js；babel的扩展插件可以在.babelrc文件下加入规则
> * webpack 就是将指定目录下的文件进行打包，并将打包后的内容输出到指定目录
> * 启动webpack-dev-server后，你在目标文件夹中是看不到编译后的文件的,实时编译后的文件都保存到了内存当中。因此很多同学使用webpack-dev-server进行开发的时候都看不到编译后的文件
