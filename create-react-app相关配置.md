# create-react-app修改配置

## css模块化修改步骤

css模块化的意义在于一个样式文件对应一个组件，不进行全局污染的解决方案

create-react-app引入antd跟less配置参考：https://ant.design/docs/react/use-with-create-react-app-cn

1. npm run eject，暴露配置，添加一些自定义配置，随着暴露package.script的命令发生改变，这个时候override不起效果了


2. 按需加载的babel，安装依赖babel-plugin-import，创建.babelrc文件
```
{
  "presets": ["react-app"],
  "plugins": [
    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
  ]
}
```


3. webpack.dev && webpack.prod增加less|css的模块化配置

css-loader添加modules:true,之所以写两个test是因为作用的文件加不同，对第三方组件如antd就不能进行模块化
```
{
    test: /\.less$/,
    exclude: [/node_modules/],
    use: [
        require.resolve('style-loader'),
        {
        loader: require.resolve('css-loader'),
        options: {
            importLoaders: 1,
            modules: true,
        },
        },
        {
        loader: require.resolve('postcss-loader'),
        options: {
            // Necessary for external CSS imports to work
            // https://github.com/facebookincubator/create-react-app/issues/2677
            ident: 'postcss',
            plugins: () => [
            require('postcss-flexbugs-fixes'),
            autoprefixer({
                browsers: [
                '>1%',
                'last 4 versions',
                'Firefox ESR',
                'not ie < 9', // React doesn't support IE8 anyway
                ],
                flexbox: 'no-2009',
            }),
            ],
        },
        },
        {
        loader: require.resolve('less-loader'), // compiles Less to CSS
        options: { javascriptEnabled: true }
        }
    ],
}, {
    test: /\.less$/,
    exclude: [/src/],
    use: [
        require.resolve('style-loader'),
        {
        loader: require.resolve('css-loader'),
        options: {
            importLoaders: 1,
        },
        },
        {
        loader: require.resolve('postcss-loader'),
        options: {
            // Necessary for external CSS imports to work
            // https://github.com/facebookincubator/create-react-app/issues/2677
            ident: 'postcss',
            plugins: () => [
            require('postcss-flexbugs-fixes'),
            autoprefixer({
                browsers: [
                '>1%',
                'last 4 versions',
                'Firefox ESR',
                'not ie < 9', // React doesn't support IE8 anyway
                ],
                flexbox: 'no-2009',
            }),
            ],
        },
        },
        {
        loader: require.resolve('less-loader'), // compiles Less to CSS
        options: { javascriptEnabled: true }
        }
    ],
}
  ```

4. index.js入口文件添加ant的全局样式

`import 'antd/dist/antd.less';`


5. 页面书写
```
import styles from './**.less';

div className={styles.container}/>
```


## 多入口 && 多出口

```
entry: {
    app: './src/app.js',
    index: './src/index.js',
    map: './src/map.js',
},
output: {
    filename: '[name].js',
    path: __dirname + '/dist'
}

// ./dist/app.js, ./dist/index.js, ./dist/map.js
```


## 多页面应用配置

疑问来源：一般单页面应用通过react-router路由进行跳转就绰绰有余了，之前也不了解多页面应用要干嘛用，做了一个智慧城市的项目，里面嵌套的地图（一堆jquery封装的地图方法，大佬不想改里面的方法）是通过iframe的src属性加载，如果是网络路径也没啥事，但是发现src是相对路径也会走路由匹配，然而路由里面根本没有这条路径，所以就通过多页面配置增加一个map的html模板解决了问题

1. webpack.dev && webpack.prod增加HtmlWebpackPlugin插件配置

```
new HtmlWebpackPlugin({
    filename: 'index.html',
    // template路径可以直接字符串'public/index.html'，paths是写在path.js文件里暴露出来的路径
    template: paths.appHtml,
    chunks: ['app']
}),
new HtmlWebpackPlugin({
    filename: 'map.html',
    template: paths.mapHtml,
    chunks: ['map']
}),
```

2. 页面调用

`<iframe src="map.html" style={{ height: 500, width: 500 }} />`

### 
