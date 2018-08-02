css模块化修改步骤


npm run eject，随着package.script的命令改变，这个时候override不起效果了


安装依赖babel-plugin-import，创建.babelrc文件
```
{
  "presets": ["react-app"],
  "plugins": [
    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
  ]
}
```


webpack.dev && webpack.prod增加less|css的模块化配置
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

index.js入口文件添加ant的全局样式
import 'antd/dist/antd.less';


页面
import styles from './**.less';
div className={styles.container}/>