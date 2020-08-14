title: 搭建vue-cli
author: coolsong
tags:
  - JavaScript
  - Vue
categories:
  - JS
date: 2019-05-14 23:07:00
---
&nbsp;&nbsp;&nbsp;&nbsp;最近看到网上的一篇博客，写的如何自己搭建一个vue-cli。自己思考了一下，好像之前面试也被问到过这个问题，但是要自己一下子来回答还是有点不确定。所以自己就决定，也像那篇博客一样，一步一步的搭一下。我们都知道vue-cli生成的项目中可以配置各种各样的功能。比如转码、文件转换、资源加上hash、代码热更新、资源预加载等。那具体是怎么实现的呢？
<!--more-->
## 1 搭建webpack环境
### 1.1 初始化
新建一个文件夹，在文件夹中输入下面的命令
```
npm init //生成package.json

cnpm install webpack webpack-cli -D //安装webpack
```
### 1.2 测试
在文件夹中新建一个src文件夹，再在里面建立一个main.js
```javascript
//main.js
console.log('ok')
```
可以再package.json中新增一条命令
```
"serve": "webpack ./src/main.js --mode development"
```
测试的目录结构如图所示：
![My Pic](/images/cli1.png)
运行结果：打包成功 生成了一个dist文件夹

## 2.配置功能

* 在根目录新建一个build文件夹，来存放webpack的配置信息
* 在build文件夹中建立一个webpack.config.js,基本配置文件
* 像里面添加内容
```javascript
const path = require('path')
module.exports = {
mode: 'development',//指定打包的模式
entry: {//打包的入口
main: path.resolve(__dirname,'../src/main.js')//将文件路径解析为绝对路径
},
output: {
path: path.resolve(__dirname, '../dist'),
filename: 'js/[name].[hash:8].js',//生成文件路径及名字
publicPath: './'//资源引用路径
}
}
```
再修改package.json中命令
```

"serve": "webpack ./src/main.js --config ./build/webpack.config.js"
```
### 2.1配置ES6/7/8转ES5

* 安装做需要的依赖
```
npm install babel-loader @babel/core @babel/preset-env
```

*修改webpack.config.js配置
```javascript
const path = require('path')
module.exports = {
  mode: 'development',//指定打包的模式
  entry: {//打包的入口
    main: path.resolve(__dirname,'../src/main.js')//将文件路径解析为绝对路径
  },
  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: 'js/[name].[hash:8].js',//生成文件路径及名字
    publicPath: './'//资源引用路径
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: [
          {
            loader: 'babel-loader'
          }
        ]
      }
     ]
   }
}
```

* 在根目录中提那家一个babel.config.js
```javascript
module.exports = {
    presets: [
        "@babel/preset-env"
    ]
}
```
这样就实现了转码的功能
#### 2.1.1 ES6/7/8 API转es5
由于babe-loader只会将ES6/7/8语法转换为ES5，但是对api并没有转化
所以还需要安装新的依赖
```
cnpm install @babel/polyfill
```
同时修改webpack.config.js
```javascript
entry: {//打包的入口
main: ["@babel/polyfill",path.resolve(__dirname,'../src/main.js')]//将文件路径解析为绝对路径
},

```
#### 2.1.2按需引入polyfill
与上一个配置其一便可

* 安装相关依赖
```
cnpm install core-js@2 @babel/runtime-corejs2 -S
```
修改babel-config.js
```js
module.exports = {
  presets: [
    ["@babel/preset-env",{
       "useBuiltIns":"usage"
     }]
  ]
}
```
设置按需引入后能大大减少打包编译后的体积
### 2.2 配置scss转css
如果没配置css相关的loader时，引入sass、less等相关文件打包时会报错

* 安装相关依赖
```
cnpm install sass-loader dart-sass css-loader style-loader -D
```
sass-loader, dart-sass主要是将 scss/sass 语法转为csscss-loader主要是解析 css 文件style-loader 主要是将 css 解析到 html页面 的 style 上

* 修改webpack.config.js配置
```js
module: {
  rules: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      use: [
        {
          loader: 'babel-loader'
        }
      ]
    },
    {
      test:/\.(sass|scss)$/,
      use: [
        {
          loader: 'style-loader'
        },
        {
          loader: 'css-loader'
        },
        {
          loader: 'sass-loader',
          options: {
            implementation: require('dart-sass')
          }
        }
      ]
    }
  ]
}
```
### 2.3配置postcss自动添加css3前缀

* 安装相关依赖
```
cnpm install postcss-loader autoprefixer -D
```

* 修改webpack.config.js配置
```js
{
  test:/\.(sass|scss)$/,
  use: [
    {
      loader: 'style-loader'
    },
    {
      loader: 'css-loader'
    },
    {
      loader: 'sass-loader',
      options: {
        implementation: require('dart-sass')
      }
    },
    {
      loader: 'postcss-loader'
    }
  ]
}
```

* 在根目录新建一个postcss.config.js
```js
module.exports = {
    plugins: {
      autoprefixer: {}
    }
}
```
### 2.4 使用html-webpack-plugin来创建html模板
使用html-webpack-plugin来创建html页面，并自动引入打包生成的js文件

* 安装依赖
```
cnpm install html-webpack-plugin -D
```

* 新建一个public文件夹，并在里面新建index.html
```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>index</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```
* 修改webpack.config.js配置
![My Pic](/images/cli2.png)
### 2.5 配置devServer热更新功能
热更新功能可以实现不刷新页面的情况下，更新页面
* 安装依赖
```
cnpm install webpack-dev-server -D
```
* 修改webpack.config.js配置
* 通过配置devServer和HotModuleReplacementPlugin插件来实现
```js
devServer:{
hot: true,
port: 3000,
contentBase: './dist'
},
```
### 2.6 配置webpack打包图片、媒体、字体等
* 安装依赖
```
cnpm install file-loader url-loader -D
```
file-loader 解析文件url，并将文件复制到输出的目录中url-loader 功能与 file-loader 类似，如果文件小于限制的大小。则会返回 base64 编码，否则使用 file-loader 将文件复制到输出的目录中
* 修改webpack-config.js配置在rules中添加相应的规则
```js
{
  test: /\.(jpe?g|png|gif)$/i,
  use: [
    {
      loader: 'url-loader',
      options: {
         limit: 4096,
         fallback: {
           loader: 'file-loader',
             options: {
                 name: 'img/[name].[hash:8].[ext]'
              }
          }
       }
      }
    ]
  },
  {
    test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
    use: [
    {
      loader: 'url-loader',
      options: {
         limit: 4096,
         fallback: {
           loader: 'file-loader',
             options: {
               name: 'media/[name].[hash:8].[ext]'
             }
         }
      }
    }
    ]
  },
  {
    test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
    use: [
    {
      loader: 'url-loader',
      options: {
        limit: 4096,
        fallback: {
        loader: 'file-loader',
        options: {
           name: 'fonts/[name].[hash:8].[ext]'
         }
       }
      }
    }
  ]
}
```
## 3 让webpack识别.vue文件
* 安装所需的依赖
```
cnpm 
npm install vue-loader vue-template-compiler cache-loader thread-loader -D
cnpm install vue -S
```
vue-loader 用于解析.vue文件vue-template-compiler 用于编译模板cache-loader 用于缓存loader编译的结果thread-loader 使用 worker 池来运行loader，每个 worker 都是一个 node.js 进程。
* 修改webpack.config.js配置
```js

const path = require('path') 
const webpack = require('webpack') 
const HtmlWebpackPlugin = require('html-webpack-plugin') 
const VueLoaderPlugin = require('vue-loader/lib/plugin') module.exports = { 
  mode: 'development', 
  entry: { // ... }, 
  output: { // ... },
  devServer: { // ... },
  resolve: { 
    alias: {
      vue$: 'vue/dist/vue.runtime.esm.js'
    }, 
   }, 
   module: {
     rules: [
       { 
         test: /\.vue$/, 
         use: [
           { 
             loader: 'cache-loader' 
           },
           {
             loader: 'thread-loader' 
           },
           {
             loader: 'vue-loader', 
             options: {
               compilerOptions: { 
                 preserveWhitespace: false
                }, 
              } 
            }
          ] 
        },
        { 
          test: /\.jsx?$/,
          use: [ 
            { 
              loader: 'cache-loader'
            },
            { 
              loader: 'thread-loader' 
            },
            { 
              loader: 'babel-loader'
            } 
          ]
         },
         // ... 
         ]
       },
       plugins: [ // ... new VueLoaderPlugin() ] 
     }
```
* 测试一下结果
1.在src下新建App，vue
```Vue
<template>
  <div class="app">
    Hello World
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {};
  }
};
</script>
<style lang="scss" scoped>
.app {
  color: red;
}
</style>
```
2.修改main.js
```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
  render: h => h(App)
}).$mount('#app')
```
3.运行一下 npm run serve
能够正常打包
## 4 定义环境变量
```js
plugins: [
    new webpack.DefinePlugin({
        'process.env': {
            VUE_APP_BASE_URL: JSON.stringfy('http://localhost:3000')
        }
    })
]
```
## 5.区分生产环境和开发环境
新建两个文件
* webpack.config.js 公用的配置
* webpack.dev.js  开发环境使用
* webpack.prod.js 生产环境
比较一下开发环境与生产环境的不停
### 5.1开发环境

1. 不需要代码压缩
2. 需要热更新
3. css不需要提取到css文件
4. sourseMap
5. 不需要转码

### 5.2生产环境

1. 压缩代码
2. 不需要热更新
3. 提取css并压缩
4. sourceMap
5. 构建时覆盖之前的内容

* 安装所需的依赖
```
npm i @intervolga/optimize-cssnano-plugin mini-css-extract-plugin clean-webpack-plugin webpack-merge copy-webpack-plugin -D
```
1. @intervolga/optimize-cssnano-plugin 用于压缩css代码
2. mini-css-extract-plugin 用于提取css到文件中
3. clean-webpack-plugin 用于删除上次构建的文件
4. webpack-merge 合并 webpack配置
5. copy-webpack-plugin 用户拷贝静态资源

### 5.3开发环境配置
* build/webpack.dev.js
```js
// build/webpack.dev.js
const merge = require('webpack-merge')
const webpackConfig = require('./webpack.config')
const webpack = require('webpack')
module.exports = merge(webpackConfig, {
  mode: 'development',
  devtool: 'cheap-module-eval-source-map',
  module: {
    rules: [
      {
        test: /\.(scss|sass)$/,
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2
            }
          },
          {
            loader: 'sass-loader',
            options: {
              implementation: require('dart-sass')
            }
          },
          {
            loader: 'postcss-loader'
          }
        ]
      },
    ]
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('development')
      }
    }),
  ]
})
```
* build/webpack.config.js
```js
// build/webpack.config.js
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
  entry: {
    // 配置入口文件
    main: path.resolve(__dirname, '../src/main.js')
  },
  output: {
    // 配置打包文件输出的目录
    path: path.resolve(__dirname, '../dist'),
    // 生成的 js 文件名称
    filename: 'js/[name].[hash:8].js',
    // 生成的 chunk 名称
    chunkFilename: 'js/[name].[hash:8].js',
    // 资源引用的路径
    publicPath: '/'
  },
  devServer: {
    hot: true,
    port: 3000,
    contentBase: './dist'
  },
  resolve: {
    alias: {
      vue$: 'vue/dist/vue.runtime.esm.js'
    },
    extensions: [
      '.js',
      '.vue'
    ]
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: [
          {
            loader: 'cache-loader'
          },
          {
            loader: 'vue-loader',
            options: {
              compilerOptions: {
                preserveWhitespace: false
              },
            }
          }
        ]
      },
      {
        test: /\.jsx?$/,
        loader: 'babel-loader'
      },

      {
        test: /\.(jpe?g|png|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 4096,
              fallback: {
                loader: 'file-loader',
                options: {
                  name: 'img/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 4096,
              fallback: {
                loader: 'file-loader',
                options: {
                  name: 'media/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 4096,
              fallback: {
                loader: 'file-loader',
                options: {
                  name: 'fonts/[name].[hash:8].[ext]'
                }
              }
            }
          }
        ]
      },
    ]
  },
  plugins: [
    new VueLoaderPlugin(),

    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '../public/index.html')
    }),
    new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin(),
  ]
}
```
### 5.4 生产环境配置
```js
const path = require('path')
const merge = require('webpack-merge')
const webpack = require('webpack')
const webpackConfig = require('./webpack.config')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const OptimizeCssnanoPlugin = require('@intervolga/optimize-cssnano-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin')
const CopyWebpackPlugin = require('copy-webpack-plugin')
module.exports = merge(webpackConfig, {
  mode: 'production',
  devtool: '#source-map',
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendors: {
          name: 'chunk-vendors',
          test: /[\\\/]node_modules[\\\/]/,
          priority: -10,
          chunks: 'initial'
        },
        common: {
          name: 'chunk-common',
          minChunks: 2,
          priority: -20,
          chunks: 'initial',
          reuseExistingChunk: true
        }
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.(scss|sass)$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader
          },
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2
            }
          },
          {
            loader: 'sass-loader',
            options: {
              implementation: require('dart-sass')
            }
          },
          {
            loader: 'postcss-loader'
          }
        ]
      },
    ]
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: 'production'
      }
    }),
    new MiniCssExtractPlugin({
      filename: 'css/[name].[contenthash:8].css',
      chunkFilename: 'css/[name].[contenthash:8].css'
    }),
    new OptimizeCssnanoPlugin({
      sourceMap: true,
      cssnanoOptions: {
        preset: [
          'default',
          {
            mergeLonghand: false,
            cssDeclarationSorter: false
          }
        ]
      }
    }),
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../public'),
        to: path.resolve(__dirname, '../dist')
      }
    ]),
    new CleanWebpackPlugin()
  ]
})
```
### 5.5 修改package.json
```js

"scripts": { 
  "serve": "webpack-dev-server --config ./build/webpack.dev.js",         "build": "webpack --config ./build/webpack.prod.js"
}
```
运行npm run serve成功启动
![My Pic](/images/cli3.png)
运行npm run build 打包成功
![My Pic](/images/cli4.png)
参考博客：[面试官：自己搭建过vue开发环境吗？](https://juejin.im/post/5cc55c336fb9a032086dd701)
