# 管理输出

## 修改输出文件目录

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
+   assetModuleFilename: 'images/[hash][ext][query]'
  },
  module: {
    rules: [
      {
        test: /\.png/,
        type: 'asset/resource'
-     }
+     },
+     {
+       test: /\.html/,
+       type: 'asset/resource',
+       generator: {
+         filename: 'static/[hash][ext][query]'
+       }
+     }
    ]
  },
};
```

## 设置 HtmlWebpackPlugin

首先安装插件，并且调整 `webpack.config.js` 文件：

```powershell
npm install --save-dev html-webpack-plugin
```

**webpack.config.js**

```js
 const path = require('path');
+const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   entry: {
     index: './src/index.js',
     print: './src/print.js',
   },
+  plugins: [
+    new HtmlWebpackPlugin({
+      title: '自动引入打包的资源-示例',
+      // 模板,以 public/index.html 为模板,创建新的 html 文件
+      // 新的 html 文件和原来的一样,自动引入了打包输出的资源
+      template: path.resolve(__dirname, "public/index.html") 
+    }),
+  ],
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
 };
```

HtmlWebpackPlugin 可以依据 html 模板自动生成新的 html 文件，新的 html 文件和模板结构一致，并自动引入打包后的文件。
