#### Vue 为什么要用 Vue.set(vm.$set)解决对象新增属性不能相应的问题？说下以下代码的实现原理

```
Vue.set(object, propertyName, value);
vm.$set(object, propertyName, value);

```

##### 题解

###### 1)Vue 为什么要用 Vue.set(vm.$set)解决对象新增属性不能相应的问题

1.Vue 使用了 Object.defineProperty 实现双向数据绑定

2.在初始化实例时，对属性执行 getter/setter 转化

3.属性必须在 data 对象上存在才能让 Vue 将它转换为响应式的(这也就造成了 Vue 无法检测到对象属性的添加或删除)

###### 2)框架本身是如何实现的

Vue 源码位置：vue/src/core/instance/index.js(版本"vue": "^2.5.2",在 vue/src/core/observer/index.js)

```
export function set (target: Array<any> | Object, key: any, val: any): any {
  if (process.env.NODE_ENV !== 'production' &&
    (isUndef(target) || isPrimitive(target))
  ) {
    warn(`Cannot set reactive property on undefined, null, or primitive value: ${(target: any)}`)
  }
  // target 为数组
  if (Array.isArray(target) && isValidArrayIndex(key)) {
    // 修改数组的长度，避免索引大于数组长度导致splice()执行有误
    target.length = Math.max(target.length, key)
    // 利用数组的splice变异方法触发响应式
    target.splice(key, 1, val)
    return val
  }
  // key 已经存在，直接修改属性值
  if (key in target && !(key in Object.prototype)) {
    target[key] = val
    return val
  }
  const ob = (target: any).__ob__
  if (target._isVue || (ob && ob.vmCount)) {
    process.env.NODE_ENV !== 'production' && warn(
      'Avoid adding reactive properties to a Vue instance or its root $data ' +
      'at runtime - declare it upfront in the data option.'
    )
    return val
  }
  // target 本身就不是响应式数据，直接赋值
  if (!ob) {
    target[key] = val
    return val
  }
  defineReactive(ob.value, key, val)
  ob.dep.notify()
  return val
}

```

阅读以上源码，vm.$set 的实现原理是：

1.如果目标是数组，直接使用数组的 splice 方法触发响应式

2.如果目标是对象，会先判断属性是否存在，对象是否是响应式

3.最终如果要对属性进行响应式处理，则是通过调用 defineReactive 方法进行响应式处理

defineReactive 方法就是 Vue 在初始化对象时，给对象属性采用 Object.defineProperty 动态添加 getter/setter 的功能所调用的方法。
