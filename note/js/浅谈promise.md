##### 说一说 promise，有几个状态，通过 catch 捕获到 reject 之后，在 catch 后面还能继续执行 then 方法吗？如果能执行，执行的是第几个回调函数？

###### 一、什么是 Promise

Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用(proxy)，充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数。

###### 二、Promise 对象状态

Promise 对象通过自身的状态，在控制异步操作。

Promise 实例具有三种状态
异步操作未完成(pending)
异步操作成功(fulfilled)
异步操作失败(rejected)
上面三种状态里面，fulfilled 和 rejected 合在一起称为 resolved(已定型)。

这三种状态的变化途径只有两种：
从“未完成”到“成功”
从“未完成”到“失败”

一旦状态发生变化，就凝固了，不会再有新的状态变化。这也是 Promise 这个名字的由来，它的英语意思是“承诺”，一旦承诺成效，就不得再改变了。这也意味着，Promise 实例的状态变化只可能发生一次。

因此，Promise 的最终结果只有两种：
异步操作成功，Promise 实例传回一个值(value)，状态变为 fulfilled
异步操作失败，Promise 实例抛出一个错误(error)，状态变为 rejected

###### 三、错误捕获

通过 catch 捕获到 reject 之后，在 catch 后面还可以继续顺序执行 then 方法，但是只执行 then 的第一个回调(resolve 回调)

```
Promise.reject(2)
  .catch(r => {
    // 捕获到错误，执行
    console.log('catch1')
  })
  // 错误已经被捕获，后边的'then'都顺序执行，且只执行'then'的第一个回调(resolve的回调)
  .then(v => {
    console.log('then1')
  }, r => {
    console.log('catch2')
  })
  .catch(r => {
    // 前边没有未捕获的错误，不执行
    console.log('catch3')
  })
  .then(v => {
    console.log('then2')
  }, r => {
    console.log('catch4')
  })

```

结果会打印：catch1 then1 then2
