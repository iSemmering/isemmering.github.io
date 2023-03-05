# 其他方面

## 1.webpack 常见配置项

devtool：source-map 用来定位当前错误代码具体的位置

entry：指定 Webpack 打包的入口文件，可以是单个文件或多个文件。

output：指定打包后的文件输出目录和文件名。

module：配置模块解析规则，例如使用什么加载器解析什么类型的文件。

resolve：配置 Webpack 如何解析模块，例如设置模块的搜索路径和文件后缀名。

plugins：使用插件来扩展 Webpack 功能，例如打包前清理输出目录、压缩代码、抽离 CSS 等。

devServer：配置 Webpack 开发服务器，例如设置端口号、代理、热更新等。

## 2.webpack 中 plugin 和 loader 区别

Loader 是用于在打包模块时对模块代码进行转换的工具。Webpack 通过在模块编译过程中使用不同的 Loader 实现对不同类型的模块的处理。常见的 Loader 有 babel-loader（将 ES6+代码转换为 ES5 代码）、css-loader（加载和处理 CSS 文件）等。Loader 处理的是模块级别的内容，最终被打包到编译后的代码中。

Plugin 是用于扩展 Webpack 功能的工具，Plugin 可以监听 Webpack 构建过程中的事件，在特定时机做出相应的操作。Plugin 的作用范围是在整个构建过程中，从而可以做一些全局性的操作，例如代码压缩、资源优化、文件输出等等。

因此，可以简单理解为 Loader 是针对模块级别的处理，而 Plugin 是针对构建过程的全局性处理。

## 3.常用的插件和 loader

常用插件：

HtmlWebpackPlugin：自动生成 HTML 文件，并自动引入打包后的资源文件。
MiniCssExtractPlugin：将 CSS 抽取为独立的文件。
CleanWebpackPlugin：清理打包文件夹中的无用文件。
CopyWebpackPlugin：拷贝文件或者文件夹到打包目录。
DefinePlugin：定义全局常量。
HotModuleReplacementPlugin：热更新插件。
UglifyJsPlugin：压缩 JavaScript 代码。
OptimizeCSSAssetsPlugin：压缩 CSS 代码。
CompressionWebpackPlugin：使用 gzip 压缩打包后的文件，提高加载速度。

常用 loader：

babel-loader：将 ES6/ES7/ES8 代码转换为 ES5 代码。
css-loader：解析 CSS 文件。
style-loader：将 CSS 代码注入到 HTML 中的 style 标签里。
sass-loader：将 Sass/Scss 转换成 CSS。
less-loader：将 Less 转换成 CSS。
file-loader：将文件转换成 URL。
url-loader：将小于某个大小的文件转换成 URL。
eslint-loader：使用 ESLint 检查 JavaScript 代码风格。

## 4.webpack 编译流程

Webpack 的打包流程主要分为以下几个阶段：

解析配置参数：Webpack 会读取用户配置文件中的参数，解析出入口文件、输出文件、loader、plugin 等相关信息。

编译模块：Webpack 会从入口文件开始，根据配置参数逐个解析出所有的模块，并根据各自的 Loader 对其进行编译转换，生成一个个包含各自执行结果的 JavaScript 对象。

生成 Chunk：在上一步的基础上，Webpack 会将编译好的模块按照配置生成一些列的 Chunk，每个 Chunk 包含了一个或多个模块的 JavaScript 对象，并且会对这些 Chunk 进行优化和压缩。

输出打包文件：Webpack 会将生成的 Chunk 按照配置合并并压缩，最后输出到指定的文件系统中，生成最终的打包文件。

总体来说，Webpack 的打包流程就是将多个模块经过编译、打包、优化和压缩等处理，最终输出一个或多个打包文件的过程。在这个过程中，Webpack 会根据配置参数以及 Loader 和 Plugin 的作用，进行相应的优化和处理，以达到提高打包效率、减小打包体积的目的。

## 5.babel 编译流程

解析：Babel 首先会将输入的代码字符串解析成抽象语法树（AST），以便后续进行分析和修改。Babel 使用了一个名为 @babel/parser 的包来进行解析。

转换：Babel 的转换阶段是整个编译过程的核心部分，它会对输入的代码进行修改和转换，使其符合目标环境的语法和语义要求。Babel 使用一组插件来实现转换功能，这些插件会对 AST 进行遍历，对匹配的节点进行修改，并将修改后的 AST 转换回代码字符串形式。

生成：最后一步是将转换后的 AST 转换回字符串形式的代码，并输出给使用者或者其他工具使用。Babel 使用了一个名为 @babel/generator 的包来实现代码生成。

需要注意的是，Babel 的插件是可插拔的，使用者可以根据自己的需要选择合适的插件或者编写自己的插件，以实现自定义的转换功能。同时，Babel 还提供了许多其他的工具和插件，如 @babel/polyfill、@babel/cli 等，以便更方便地使用和集成 Babel。

## 6.优化 webpack 打包速度

1. 使用 Tree Shaking
   Tree Shaking 是通过静态分析代码来识别哪些代码可以安全的删除，从而减少代码的大小，提升打包速度。可以通过以下配置开启 Tree Shaking：

```javascript
// webpack.config.js
module.exports = {
  //...
  optimization: {
    usedExports: true, // 开启 Tree Shaking
    minimize: true, // 压缩代码
    moduleIds: 'hashed',
    runtimeChunk: 'single',
    splitChunks: { 将公共依赖打包成一个 vendors 文件
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
};

```

2. 缓存构建结果，需要用到 cache-loader

```javascript
module.exports = {
  cache: true,
  module: {
    rules: [
      {
        test: /\.js$/,
        use: "cache-loader",
        exclude: /node_modules/,
      },
    ],
  },
};
```

3. 移除一些不必要的插件
4. 分离第三方库：将第三方库和业务代码分离打包可以加快构建速度。可以使用 CommonsChunkPlugin 来实现。

```javascript
module.exports = {
  entry: {
    vendor: ["react", "react-dom"],
    app: "./src/index.js",
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      name: "vendor",
      minChunks: Infinity,
    }),
  ],
};
```

5. 开启多线程：可以在 Webpack 的 loader 配置中使用 thread-loader 插件，开启多线程处理。这样可以将耗时的操作（如 babel 转译）交给 worker 线程处理，从而加速打包速度。

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          {
            loader: "thread-loader",
            options: {
              workers: os.cpus().length - 1,
              workerParallelJobs: 50,
            },
          },
          "babel-loader",
        ],
        exclude: /node_modules/,
      },
    ],
  },
};
```

## 7.执行 npm 发生了什么

在命令行中输入 npm run <command>，其中 <command> 对应 package.json 文件中 scripts 字段中定义的脚本名称。

npm 根据当前目录下的 package.json 文件中的 dependencies、devDependencies 和 optionalDependencies 字段，解析出依赖树，并在本地 node_modules 目录下安装所有依赖包。

根据 package.json 文件中 scripts 字段的配置，执行对应的命令。
