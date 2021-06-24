#### Vue 定义全局方法

实现方式

##### 一、将方式挂载到 Vue.prototype 上面

```
// global.js
const RandomString = (encode = 36, number = -8) => {
  return Math.random() // 生成随机数字
    .toString(encode) // 转化成36进制
    .slice(number);
}

export default {
  RandomString
}

```

```
// 在项目入口的main.js里配置
import Vue from "vue";
import global from "@/global";
Object.keys(global).forEach((key) => {
  Vue.prototype["$global" + key] = global[key]
});

```

```
// 挂载之后，在需要引用全局变量的模块处(App.vue)，不需要导入全局变量模块，而是直接用this就可以引用了，如下：
export default {
  mounted() {
    this.$globalRandomString();
  }
}

```

##### 二、利用全局混入 mixin

优点：因为 mixin 里面的 methods 会和创建的每个单文件组件合并，这样做的优点是调用这个方法的时候有提示

```
// mixin.js
const mixin = {
  methods: {
    minRandomString(encode = 36, number = -8) {
      return Math.random() // 生成随机数字
        .toString(encode) // 转化成36进制
        .slice(number);
    }
  }
}
export default mixin

```

```
// 在项目入口的main.js里配置
import Vue from "vue";
import mixin from "@/mixin";
Vue.mixin(mixin);

```

```
export default {
  mounted() {
    this.minRandomString();
  }
}

```

##### 三、使用 plugin 方式

Vue.use 的实现没有挂载的功能，只是触发了插件的 install 方法，本质还是使用了 Vue.prototype

```
// plugin.js
function randomString(encode = 36, number = -8) {
  return Math.random() // 生成随机数字
    .toString(encode) // 转化成36进制
    .slice(number);
}
const plugin = {
  // install是默认的方法
  // 当外界在use这个组件或函数的时候，就会调用本身的install方法，同时传一个Vue这个类的参数
  install: function(Vue) {
    Vue.prototype.$pluginRandomString = randomString
  }
}
export default plugin

```

```
// 在项目入口的main.js里配置
import Vue from "vue";
import plugin from "@/plugin";
Vue.use(plugin);

```

```
export default {
  mounted() {
    this.$pluginRandomString();
  }
}

```

##### 四、任意 vue 文件中写全局函数

```
// 创建全局方法
this.$root.$on("test", function() {
  console.log("test");
});
// 销毁全局方法
this.$root.$off("test");
// 调用全局方法
this.$root.$emit("test");

```
