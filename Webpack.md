# 插件篇

### 用法

webpack配置文件中plugins属性数组
```
plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(src, 'src/index.html')
    }),
    new webpack.ProvidePlugin({
      $q: path.resolve(app, 'unit/global.js')
    })
  ],
```

### 分类（webpack内置插件和其他插件）

上述代码引入HtmlWebpackPlugin和ProvidePlugin的方式不同，官方解释，通过webpack属性配置的插件就包含再webpack模块中，即是否是内置模块的区别。

### 常用插件

1. ProvidePlugin自动加载模块

用途：模块中出现$q，就相当于执行了import('jquery')或require('jquery')
```
new webpack.ProvidePlugin({
    $q: 'jquery'
})
```

2. DefinePlugin定义全局常量

```
new webpack.DefinePlugin({
    PRODUCTION: JSON.stringify(PRODUCTION)
})
```

3. ExtractTextPlugin分离css文件

用途：默认css文件打包进了js文件中，将css从js中抽离

` new ExtractTextPlugin(PRODUCTION ? '[name].css' : 'style.css') `

4. HtmlWebpackPlugin创建html文件

用途：重构入口html

```
new HtmlWebpackPlugin({
    title: config.APP_NAME,
    template: './entry/index.html',// 源文件
    filename: './dist/index.html',// 输出文件
    chunks: ['index'],// js入口，存在于entry属性数组中
})
```

5. UglifyJsPlugin

用途：js压缩

` new webpack.optimize.UglifyJsPlugin() `

# loaders模块加载器

webpack将所有静态资源都认为时模块，比如Javascript,Css,Less,jsx,图片等等，除了纯Javascript之外，每一种资源都需要通过对应的加载器处理成模块

### 用法
```
module: {
    rules: [
        {
            test: /\.jsx?$/,
            loader: 'babel-loader'
        }, {
            test: /\.tsx?$/,
            use: ['babel-loader', 'ts-loader']
        },
    ]
}
```

### 常用loader

1. babel

用途：将下一代的JavaScript标准（ES6，ES7），或者基于JavaScript进行了拓展的语言，如React的JSX，转换成浏览器能识别的js。

```
rules: [
    {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
            presets: ['es2015','react']
        }
    }
]
```

2. css-loader, style-loader, less-loader, postcss

用途：css-loader模块通过@import(js文件中引入css用import，css文件中引入css用@import)或者require可以使用样式，CSS modules对css提供了模块化，当前css文件不会全局污染；style-loader将样式表潜入js文件中；less-loader将less文件编译成css；postcss-loader为css添加适应不同浏览器的前缀

```
rules: [
    {
        test: /\.css$/,
        user: [
            { loader: 'style-loader' },
            {
                loader: 'css-loader',
                options: {
                    modules: true
                }
            },
            { loader: 'postcss-loader' },
            { loader: 'less-loader' },
        ]
        
    }
]
```

3. file-loader加载图片或者文件等