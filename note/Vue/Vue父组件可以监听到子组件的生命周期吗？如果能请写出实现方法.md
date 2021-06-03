#### Vue 父组件可以监听到子组件的生命周期吗？如果能请写出实现方法

可以

##### 1)实现方式一

比如有父组件 Parent 和子组件 Child，如果父组件监听到子组件挂载 mounted 就做一些逻辑处理，可以通过以下写法实现：

```
// Parent.vue
<Child @mounted="doSomething" />

// Child.vue
mounted() {
  this.$emit("mounted)
}

```

##### 2)实现方式二

以上需要手动通过 $emit 触发父组件的事件，更简单的方式可以在父组件引用子组件时通过 @hook 来监听即可，如下所示：

```
// Parent.vue
<Child @hook:mounted="doSomething" />

doSomething() {
  console.log('父组件监听到 mounted 钩子函数...')
}

// Child.vue
mounted() {
  console.log('子组件触发 mounted 钩子函数...')
}

// 以上输出顺序为：
// 子组件触发 mounted 钩子函数...
// 父组件监听到 mounted 钩子函数...

```

当然 @hook 方法不仅仅是可以监听 mounted，其它的生命周期事件，例如：created，updated 等都可以监听。
