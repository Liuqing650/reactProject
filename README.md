# 使用node创建的一个简单项目

1. 更新摘要 (2017-11-29)
  - 新增了自动补上浏览器兼容
    - npm install autoprefixer-loader --save-dev
  - 重新配置 webpack.config.js
    - 支持css和less
      - { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
      - 这里将会把所有的样式文件放到index.html的head里面
  - 重新划分结构
    - 原Test文件以及后续增加的文件将放到routes中的app.js进行统一管理，后续再做调整
2. 更新摘要 (2017-11-30)
  - 重新配置 webpack的样式文件
    - 将样式文件独立出来, 同目录下生成一个main.css的样式文件
      - 引入了 extract-text-webpack-plugin
      - plugins: [ new ExtractTextPlugin("[name].css", {allChunks: true}),... ]
      -  css-loader等配置，webpack解析loader是从右到左解析
        ```
        {
            test:/\.css$/,
            loader: ExtractTextPlugin.extract('css-loader')
          }, {
              test: /\.less$/,
            loader: ExtractTextPlugin.extract(['css-loader','less-loader'])
          }
        ```
    - 将打包后的js文件也以文件名打包
      - filename: "[name].js" 生成一个main.js文件, 比较大
  3. 更新摘要 （2017-12-12）
    - 修改pacage.json中的启动项
      - "start": "webpack-dev-server --open"
      - 新增 --open 即可在启动完成之后自动在默认浏览器打开项目
    - 新增图像支持
      - 新增文件 webpack-isomorphic-tools.js
      - 安装 webpack-isomorphic-tools
      - 在webpack.config.js中使用
        ```
          var WebpackIsomorphicToolsPlugin = require('webpack-isomorphic-tools/plugin');
          var webpackIsomorphicToolsPlugin = new WebpackIsomorphicToolsPlugin(require('./webpack-isomorphic-tools'));
            ...
          module.exports = {
            ...
              module: {
                loaders: [
                  { test: webpackIsomorphicToolsPlugin.regular_expression('images'), loader: 'url-loader?limit=10240' },
                  ...
                ]
              }
            ...
          }
        ```
    - 引入CSS Modules
      - 具体（https://segmentfault.com/a/1190000004530909）
      - 将css打包到css文件夹下
      ```
        new ExtractTextPlugin({ 
            filename: (getPath) => {
                return getPath('css/[name].css').replace('css/js', 'css');
            },
            allChunks: true
        })
      ```
    - 引入可视化资源分析工具webpack-visualizer
      - npm install webpack-visualizer-plugin --save-dev
      - 在webpack.config.js中使用
        ```
          var Visualizer = require('webpack-visualizer-plugin');
            ...
            module.exports = {
              ...
              plugins: [
                new Visualizer(),
                ...
              ]
            }
        ```
      - 编译 npm run build 将多出一个stats.html文件
