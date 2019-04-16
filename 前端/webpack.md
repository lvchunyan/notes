## webpack(静态模块打包工具)

当 webpack 处理应用程序时，它会在内部构建一个 依赖图(dependency graph)，此依赖图会映射项目所需的每个模块，并生成一个或多个 bundle。

<font color=red>Node.js 是一个 JavaScript 运行时，可以在浏览器环境之外的计算机和服务器中使用。webpack 运行在 Node.js 中。</font>
- **入口（entry）**

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部 依赖图(dependency graph) 的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。默认值是 ./src/index.js，但你可以通过配置 entry 属性，来指定一个（或多个）不同的入口起点。

     
     module.exports = {
       entry: './src/index.js'
     };
     
- **输出（output）**

output 属性告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件。主要输出文件的默认值是 ./dist/main.js，其他生成文件默认放置在 ./dist 文件夹中。

     //path模块一个 Node.js 核心模块，用于操作文件路径
     const path = require('path');
     
     module.exports = {
       entry: './path/to/my/entry/file.js',
       output: {
         path: path.resolve(__dirname, 'dist'),
         filename: 'main.bundle.js'
       }
     };
     
- **loader**
 
webpack 只能理解 JavaScript 和 JSON 文件。loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 模块，以供应用程序使用，以及被添加到依赖图中。在更高层面，在 webpack 的配置中 loader 有两个属性：
 1. test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
 2. use 属性，表示进行转换时，应该使用哪个 loader。  
 
 
      const path = require('path');
      
      module.exports = {
        output: {
          filename: 'my-first-webpack.bundle.js'
        },
        module: {
          rules: [
            { test: /\.txt$/, use: 'raw-loader' }
          ]
        }
      }; 
      // “嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先 使用 raw-loader 转换一下。”
注：

  _<font color='red'>  1. 在 webpack 配置中定义 rules 时，要定义在 module.rules_ 而不是 rules 中。 </font>
  
  _<font color='red'>  2. 使用正则表达式匹配文件时，你不要为它添加引号 </font>_    
  
- **插件（plugin）**

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。


      const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
      const webpack = require('webpack'); // 用于访问内置插件
      
      module.exports = {
        module: {
          rules: [
            { test: /\.txt$/, use: 'raw-loader' }
          ]
        },
        plugins: [
          new HtmlWebpackPlugin({template: './src/index.html'})
        ]
      };
      
      //html-webpack-plugin 为应用程序生成 HTML 一个文件，并自动注入所有生成的 bundle
      
  - 模式（mode）
  
通过选择 development, production 或 none 之中的一个，来设置 mode 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 production。


      module.exports = {
        mode: 'production'
      };
      
## 入口起点（entry points）

- **单个入口语法**

<font color='red'> 用法：entry: string|Array<string> </font>


      module.exports = {
        entry: './path/to/my/entry/file.js'
      };
      或
      module.exports = {
        entry: {
          main: './path/to/my/entry/file.js'
        }
      };
      
- **对象语法**

<font color='red'> 用法：entry: {[entryChunkName: string]: string|Array<string>} </font>


      module.exports = {
        entry: {
          app: './src/app.js',
          adminApp: './src/adminApp.js'
        }
      };
      
- **多个页面应用程序**


      module.exports = {
        entry: {
          pageOne: './src/pageOne/index.js',
          pageTwo: './src/pageTwo/index.js',
          pageThree: './src/pageThree/index.js'
        }
      };
      
## 输出（output）

配置 output 选项可以控制 webpack 如何向硬盘写入编译文件。**注意，即使可以存在多个 entry 起点，但只指定一个 output 配置。**

- **用法**

在 webpack 中配置 output 属性的最低要求是，将它的值设置为**一个对象**，包括以下属性：

 - filename 用于输出文件的文件名。
 
 
      module.exports = {
        output: {
          filename: 'bundle.js',
        }
      };
      //配置将一个单独的 bundle.js 文件输出到 dist 目录中
      
- **多个入口起点**

如果配置创建了多个单独的 "chunk"（例如，使用多个入口起点或使用像 CommonsChunkPlugin 这样的插件），则应该使用 **占位符(substitutions)** 来确保每个文件具有唯一的名称。


      module.exports = {
        entry: {
          app: './src/app.js',
          search: './src/search.js'
        },
        output: {
          filename: '[name].js',
          path: __dirname + '/dist'
        }
      };
      
      // 写入到硬盘：./dist/app.js, ./dist/search.js
      
## 模式（mode）

提供 mode 配置选项，告知 webpack 使用相应环境的内置优化。可能的值有：none, development 或 production（默认）。

- **用法**

      只需在配置对象中提供 mode 选项：
      module.exports = {
        mode: 'production'
      };
      

选项|描述
            ---|:--:
    development|会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。
    production |会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。
    none       |退出任何默认优化选项
    
      // webpack.development.config.js
      module.exports = {
      + mode: 'development'
      - plugins: [
      -   new webpack.NamedModulesPlugin(),
      -   new webpack.NamedChunksPlugin(),
      -   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
      - ]
      }
      
      // webpack.production.config.js
      module.exports = {
      +  mode: 'production',
      - plugins: [
      -   new TerserPlugin(/* ... */),
      -   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
      -   new webpack.optimize.ModuleConcatenationPlugin(),
      -   new webpack.NoEmitOnErrorsPlugin()
      - ]
      }
      
      // webpack.custom.config.js
      module.exports = {
      + mode: 'none',
      - plugins: []
      }
      
## loader

loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件。loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript 或将内联图像转换为 data URL。loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！

- **示例**

使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JavaScript。为此，首先安装相对应的 loader：

      npm install --save-dev css-loader
      npm install --save-dev ts-loader
    
      module.exports = {
        module: {
          rules: [
            { test: /\.css$/, use: 'css-loader' },
            { test: /\.ts$/, use: 'ts-loader' }
          ]
        }
      };
      // 指示 webpack 对每个 .css 使用 css-loader，以及对所有 .ts 文件使用 ts-loader
      
- **使用loader**

  有三种使用 loader 的方式： 
   * 配置（推荐）：在 webpack.config.js 文件中指定 loader。
   * 内联：在每个 import 语句中显式指定 loader。
   * CLI：在 shell 命令中指定它们
   
   注：<font color=red>loader 遵循 模块解析 标准。多数情况下，loader 将从 模块路径 加载（通常是从 npm install, node_modules 进行加载）。</font>
   
> 配置

module.rules 允许你在 webpack 配置中指定多个 loader。

      loader 从右到左地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，
      然后继续执行 css-loader，最后以 style-loader 为结束。
      
      module.exports = {
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
                },
                { loader: 'sass-loader' }
              ]
            }
          ]
        }
      };
      
  > 内联
  
  在 import 语句或任何 等同于 "import" 的方法 中指定 loader。使用 ! 将资源中的 loader 分开。每个部分都会相对于当前目录解析。
  
      import Styles from 'style-loader!css-loader?modules!./styles.css';
      //选项可以传递查询参数，例如 ?key=value&foo=bar，或者一个 JSON 对象，例如 ?{"key":"value","foo":"bar"}。
      
注：<font color=red> 尽可能使用 module.rules，因为这样可以减少源码中样板文件的代码量，并且可以在出错时，更快地调试和定位 loader 中的问题。 </font>

> CLI

通过 CLI 使用 loader：

      webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
      //对 .jade 文件使用 jade-loader，以及对 .css 文件使用 style-loader 和 css-loader
      
  ## 插件（plugin）
  
  插件目的在于解决 loader 无法实现的其他事
  
  - **用法**
  
       
        const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
        const webpack = require('webpack'); //访问内置的插件
        const path = require('path');
        
        module.exports = {
          entry: './src/index.js',
          output: {
            filename: 'main.bundle.js',
            path: path.resolve(__dirname, 'dist')
          },
          module: {
            rules: [
              {
                test: /\.(js|jsx)$/,
                use: 'babel-loader'
              }
            ]
          },
          plugins: [
            new webpack.ProgressPlugin(),
            new HtmlWebpackPlugin({template: './src/index.html'})
          ]
        };
    
## 模块（module）
 
在 模块化编程 中，开发者将程序分解为功能离散的 chunk(discrete chunks of functionality)，并称之为_模块_。

webpack _模块_能够以各种方式表达它们的依赖关系。下面是一些示例：

 - ES2015 import 语句
 - CommonJS require() 语句
 - AMD define 和 require 语句
 - css/sass/less 文件中的 @import 语句。
 - 样式(url(...))或 HTML 文件(<img src=...>)中的图片链接
     
## 配置（configuration）
  
  