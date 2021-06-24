**favicon.ico 图标及时更新问题**

更换了窗口小图标后，不管怎么清缓存，它就是不更新，可能过几天就好了，但我们不会去等，这时只需要在浏览器地址栏，直接输入地址，比如：localhost:8080/#/favicon.ico 去请求一次这个图标地址，浏览器就会将这个图标强制刷新过来，再次访问：localhost:8080/#/index
你就会发现：图标变了，这次真的变了

vue-cli3 [参考链接](https://blog.csdn.net/sinat_36728518/article/details/110490027?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242)

vue-cli2，本次的情况为单点登录，做了一下其他处理，开发环境地址为 xxx/#/login 打包后上传到 nginx 后地址变更为 xxx/console#/login

```
// 在webpack.prod.conf.js
function resolve(dir) {
  return path.join(__dirname, "..", dir);
}
const webpackConfig = merge(baseWebpackConfig, {
  plugins: [
    // generate dist index.html with correct asset hash for caching.
    // you can customize output by editing /index.html
    // see https://github.com/ampedandwired/html-webpack-plugin
    new HtmlWebpackPlugin({
      filename: process.env.NODE_ENV === "testing" ? "index.html" : config.build.index,
      template: "index.html",
      inject: true,
      // favicon: resolve('favicon.ico'),
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
      // necessary to consistently work with multiple chunks via CommonsChunkPlugin
      chunksSortMode: "dependency"
    }),
  ]
})

// 设置favicon: resolve('favicon.ico'),打包后index.html里<link rel="shortcut icon" href="./favicon.ico" />
// 把上面配置注释，在index.html添加<link rel="shortcut icon" href="./static/favicon.ico" />，在static目录下添加favicon.ico图标文件，打包后生成
<link rel="shortcut icon" href="./static/favicon.ico" />
// ico图标好了

```

// 开发环境地址为 xxx/#/login 打包后上传到 nginx 后地址变更为 xxx/console#/login，做了一下其他处理，中间多了个 console，href="./favicon.ico"路径取不到，观察其他打包后生成的路径，在 static 目录下可以引用，所以把 favicon 也放在 static 目录下

```
<!DOCTYPE html><html><head><meta charset=utf-8><meta http-equiv=X-UA-Compatible content="IE=edge"><meta name=viewport content="width=device-width,initial-scale=1"><meta http-equiv=Expires content=0><meta http-equiv=Pragma content=no-cache><meta http-equiv=Cache-control content=no-cache><meta http-equiv=Cache content=no-cache><title>xxx</title><link rel="shortcut icon" href=./static/favicon.ico><script src=./static/serverConfig.js></script><link href=./static/css/app.dce16f34450ebc4c4fc120bc5a82045e.css rel=stylesheet></head><body><div id=app></div><script type=text/javascript src=./static/js/manifest.f096b52fac91119b4170.js></script><script type=text/javascript src=./static/js/app.70ac234eaab20d3d5e80.js></script></body></html>

```

代码搬运工
邮箱：wlh929673054@163.com
