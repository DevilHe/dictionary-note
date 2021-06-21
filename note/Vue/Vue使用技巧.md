#### Vue 使用技巧

##### 1.使用 Object.assign 恢复 vue 组件默认值

Vue 组件可能会有这样的需求：
在某种情况下，需要重置 Vue 组件的 data 数据。此时，我们可以通过 this.$data获取当前状态下的data，通过this.$options.data()获取组件初始状态下的 data。然后只要使用 Object.assign(this.$data, this.$options.data())就可以将当前状态的 data 重置为初始状态，非常方便。

使用 Object.assign 设置默认对象 [参考链接](https://github.com/beginor/clean-code-javascript#%E7%AE%80%E4%BB%8B)
[Object.assign](https://blog.csdn.net/dwb123456123456/article/details/83316471?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

```
// 不好的
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;
}

createMenu(menuConfig);

```

```
// 好的
const menuConfig = {
  title: 'Order',
  // User did not include 'body' key
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);

```
