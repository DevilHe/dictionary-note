---
title: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.
date: 2021-05-25 14:36:10
tags: vue
---

在升级脚手架到 vue-cli3.0 版本的时候出现了这个报错：

**[Vue warn]: You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.**

我在这里大概说一下出现这个报错的原因在哪里和解决办法

#### 原因

vue 有两种形式的代码 compiler（模板）模式和 runtime 模式（运行时），vue 模块的 package.json 的 main 字段默认为 runtime 模式， 指向了"dist/vue.runtime.common.js"位置。

这是 vue 升级到 2.0 之后就有的特点。

而我的 app.vue 文件中，初始化 vue 却是这么写的，这种形式为 compiler 模式的，所以就会出现上面的错误信息

```
// compiler
new Vue({
  el: '#app',
  router: router,
  store: store,
  template: '<App/>',
  components: { App }
})

```

#### 解决办法

将 app.vue 中的代码修改如下就可以

```
//runtime

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount("#app")
```

到这里我们的问题还没完，那为什么之前是没问题的，之前 vue 版本也是 2.x 的呀？

这也是我要说的第二种解决办法

因为之前我们的 webpack 配置文件里有个别名配置，具体如下

```
resolve: {
    alias: {
        'vue$': 'vue/dist/vue.esm.js' //内部为正则表达式  vue结尾的
    }
}
```

也就是说，import Vue from 'vue' 这行代码被解析为 import Vue from 'vue/dist/vue.esm.js'，直接指定了文件的位置，没有使用 main 字段默认的文件位置

所以第二种解决方法就是，在 vue.config.js 文件里加上 webpack 的如下配置即可，

```
configureWebpack: {
    resolve: {
      alias: {
        'vue$': 'vue/dist/vue.esm.js'
      }
    }
```

既然到了这里我想很多人也会想到第三中解决方法，那就是在引用 vue 时，直接写成如下即可

```
import Vue from 'vue/dist/vue.esm.js'
```

#### 总结

遇到问题之后不是说解决了问题就完事了，更重要的是要思考 -- 为什么？，原因是啥？要理解而不是死记，只有这样才能得到提升！

如果您有疑惑或质疑的地方，请留言，或邮件给我，共同学习进步。

代码搬运工
邮箱：wlh929673054@163.com
