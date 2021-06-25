#### JS 异步笔试题，请写出下面代码的运行结果

```
var date = new Date()

console.log(1, new Date() - date)

setTimeout(() => {
  console.log(2, new Date() - date)
}, 500)

Promise.resolve().then(console.log(3, new Date() - date))

while(new Date() - date < 1000) {}

console.log(4, new Date() - date)

```

##### 答案

```
1 0
3 1
4 1000
2 1001
// 其中，关于时间差结果可能因为计算机性能造成的微小差异，可忽略不计

```

##### 解析

### 由浅入深探索 Promise 异步执行

首先，看一下 event loop 的基础必备内容

event loop 执行顺序：

首先执行 script 宏任务
执行同步任务，遇见微任务进入微任务队列，遇见宏任务进入宏任务队列
当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
执行浏览器 UI 线程的渲染工作
检查是否有 Web Worker 任务，有则执行
执行下一个宏任务，回到第二步，依此循环，直到宏任务和微任务队列都为空

<strong>微任务包括：</strong>MutationObserver、Promise.then()或 catch()、Promise 为基础开发的其它技术，比如 fetch API、V8 的垃圾回收过程、Node 独有的 process.nextTick 、 Object.observe（已废弃；Proxy 对象替代）

<strong>宏任务包括：</strong>script 、setTimeout、setInterval 、setImmediate 、I/O 、UI rendering 、 postMessage 、 MessageChannel

##### 1. 同步 + Promise

##### 题目一：

```
var promise = new Promise((resolve, reject) => {
  console.log(1)
  resolve()
  console.log(2)
})
promise.then(()=>{
  console.log(3)
})
console.log(4)
// 1
// 2
// 4
// 3

```

##### 解析：

首先明确， Promise 构造函数是同步执行的， then 方法是异步执行的
开始 new Promise ，执行构造函数同步代码，输出 1
再 resolve()， 将 promise 的状态改为了 resolved ，并且将 resolve 值保存下来，此处没有传值
执行构造函数同步代码，输出 2
跳出 promise，往下执行，碰到 promise.then 这个微任务，将其加入微任务队列
执行同步代码，输出 4
此时宏任务执行完毕，开始检查微任务队列，执行 promise.then 微任务，输出 3

##### 题目二：

```
var promise = new Promise((resolve, reject) => {
  console.log(1)
})
promise.then(()=>{
  console.log(2)
})
console.log(3)
// 1
// 3

```

##### 解析：

开始 new Promise ，执行构造函数同步代码，输出 1
再 promise.then ，因为 promise 中并没有 resolve ，所以 then 方法不会执行
执行同步代码，输出 3

##### 题目三：

```
var promise = new Promise((resolve, reject) => {
  console.log(1)
})
promise.then(console.log(2))
console.log(3)
// 1
// 2
// 3

```

##### 解析：

首先明确， .then 或者 .catch 的参数期望是函数，传入非函数则会发生值透传（ value => value ）
开始 new Promise ，执行构造函数同步代码，输出 1
然后 then() 的参数是一个 console.log(2) （注意：并不是一个函数），是立即执行的，输出 2
执行同步代码，输出 3

##### 题目四：

```
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
// 1

```

##### 解析：

then(2) 、 then(Promise.resolve(3)) 发生了值穿透，直接执行最后一个 then ，输出 1

##### 题目五：

```
var promise = new Promise((resolve, reject) => {
  console.log(1)
  resolve()
  reject()
})
promise.then(()=>{
  console.log(2)
}).catch(()=>{
  console.log(3)
})
console.log(4)
// 1
// 4
// 2

```

##### 解析：

开始 new Promise ，执行构造函数同步代码，输出 1
再 resolve()， 将 promise 的状态改为了 resolved ，并且将 resolve 值保存下来，此处没有传值
再 reject() ，此时 promise 的状态已经改为了 resolved ，不能再重新翻转（状态转变只能是 pending —> resolved 或者 pending —> rejected，状态转变不可逆）
跳出 promise，往下执行，碰到 promise.then 这个微任务，将其加入微任务队列
往下执行，碰到 promise.catch 这个微任务，此时 promise 的状态为 resolved （非 rejected ），忽略 catch 方法
执行同步代码，输出 4
此时宏任务执行完毕，开始检查微任务队列，执行 promise.then 微任务，输出 2

##### 题目六：

```
Promise.resolve(1)
  .then(res => {
    console.log(res);
    return 2;
  })
  .catch(err => {
    return 3;
  })
  .then(res => {
    console.log(res);
  });
// 1
// 2

```

##### 解析：

首先 resolve(1)， 状态改为了 resolved ，并且将 resolve 值保存下来
执行 console.log(res) 输出 1
返回 return 2 实际上是包装成了 resolve(2)
状态为 resolved ， catch 方法被忽略
最后 then ，输出 2

##### 2. 同步 + Promise + setTimeout

##### 题目一：

```
setTimeout(() => {
  console.log(1)
})
Promise.resolve().then(() => {
  console.log(2)
})
console.log(3)
// 3
// 2
// 1

```

##### 解析：

首先 setTimout 被放入宏任务队列
再 Promise.resolve().then ， then 方法被放入微任务队列
执行同步代码，输出 3
此时宏任务执行完毕，开始检查微任务队列，执行 then 微任务，输出 2
微任务队列执行完毕，检查执行一个宏任务
发现 setTimeout 宏任务，执行输出 1

##### 题目二：

```
var promise = new Promise((resolve, reject) => {
  console.log(1)
  setTimeout(() => {
    console.log(2)
    resolve()
  }, 1000)
})

promise.then(() => {
  console.log(3)
})
promise.then(() => {
  console.log(4)
})
console.log(5)
// 1
// 5
// 2
// 3
// 4

```

##### 解析：

首先明确，当遇到 promise.then 时，如果当前的 Promise 还处于 pending 状态，我们并不能确定调用 resolved 还是 rejected ，只有等待 promise 的状态确定后，再做处理，所以我们需要把我们的两种情况的处理逻辑做成 callback 放入 promise 的回调数组内，当 promise 状态翻转为 resolved 时，才将之前的 promise.then 推入微任务队列
开始， Promise 构造函数同步执行，输出 1 ，执行 setTimeout
将 setTimeout 加入到宏任务队列中
然后，第一个 promise.then 放入 promise 的回调数组内
第二个 promise.then 放入 promise 的回调数组内
执行同步代码，输出 5
检查微任务队列，为空
检查宏任务队列，执行 setTimeout 宏任务，输入 2 ，执行 resolve ， promise 状态翻转为 resolved ，将之前的 promise.then 推入微任务队列
setTimeout 宏任务出队，检查微任务队列
执行第一个微任务，输出 3
执行第二个微任务，输出 4

##### 回到开头

现在看，本题就很简单了

```
var date = new Date()

console.log(1, new Date() - date)

setTimeout(() => {
  console.log(2, new Date() - date)
}, 500)

Promise.resolve().then(console.log(3, new Date() - date))

while(new Date() - date < 1000) {}

console.log(4, new Date() - date)

```

##### 解析：

首先执行同步代码，输出 1 0
遇到 setTimeout ，定时 500ms 后执行，此时，将 setTimeout 交给异步线程，主线程继续执行下一步，异步线程执行 setTimeout
主线程执行 Promise.resolve().then , .then 的参数不是函数，直接执行（ value => value ） ，输出 3 1
主线程继续执行同步任务 whlie ，等待 1000ms ，在此期间，setTimeout 定时 500ms 完成，异步线程将 setTimeout 回调事件放入宏任务队列中
继续执行下一步，输出 4 1000
检查微任务队列，为空
检查宏任务队列，执行 setTimeout 宏任务，输入 2 1000

###### Promise.resolve().then(console.log(3, new Date() - date)) 这行有个坑

```
var date = new Date()

console.log(1, new Date() - date)

setTimeout(() => {
  console.log(2, new Date() - date)
}, 500)

// Promise.resolve().then(console.log(3, new Date() - date))
Promise.resolve().then(function (){console.log(3, new Date() - date)})

while(new Date() - date < 1000) {}

console.log(4, new Date() - date)

// 1 0
// 4 1000
// 3 1000
// 2 1001

```

##### 总结

Promise 构造函数是同步执行的， then 方法是异步执行的

.then 或者 .catch 的参数期望是函数，传入非函数则会直接执行

Promise 的状态一经改变就不能再改变，构造函数中的 resolve 或 reject 只有第一次执行有效，多次调用没有任何作用

.then 方法是能接收两个参数的，第一个是处理成功的函数，第二个是处理失败的函数，再某些时候你可以认为 catch 是.then 第二个参数的简便写法

当遇到 promise.then 时， 如果当前的 Promise 还处于 pending 状态，我们并不能确定调用 resolved 还是 rejected ，只有等待 promise 的状态确定后，再做处理，所以我们需要把我们的两种情况的处理逻辑做成 callback 放入 promise 的回调数组内，当 promise 状态翻转为 resolved 时，才将之前的 promise.then 推入微任务队列

##### 写出以下执行结果，并解释原因

```
async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2() {
  console.log('async2');
}
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0)
async1();
new Promise(function(resolve) {
  console.log('promise1');
  resolve();
}).then(function() {
  console.log('promise2');
})
console.log('script end');

```

###### 结果

```
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout

```

###### 解析

知识点
考察的是 js 中的事件循环和回调队列。注意以下几点
1.Promise 优先于 setTimeout 宏任务。所以 setTimeout 回调会在最后执行。
2.Promise 一旦被定义，就会立即执行。
3.Promise 的 reject 和 resolve 是异步执行的回调。所以，resolve()会被放到回调队列中，在主函数执行完和 setTimeout 前调用。
4.await 执行完后，会让出线程。async 标记的函数会返回一个 Promise 对象。

迷惑点

```
async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2() {
  console.log('async2');
}
// 相当于
function async1() {
  return new Promise((resolve, reject) => {
    console.log('async1 start');
    resolve()
    async2().then((result) => {
      console.log('async1 end');
    })
  })

}
function async2() {
  return new Promise((resolve) => {
    console.log('async2');
    resolve()
  })

}

```

执行流程分析

1.首先，事件循环从宏任务（macrotask）队列开始，这个时候，宏任务队列中，只有一个 script（整体代码）任务；从宏任务队列中取一个任务出来执行。

1.1 首先执行 console.log('script start')，输出'script start'。
1.2 遇到 setTimeout 把 console.log('setTimeout')放到 macrotask 队列中。
1.3 执行 async1()，输出'async1 start'和'async2'，把 console.log('async1 end')放到 micro 队列中。
1.4 执行到 Promise，输出'promise1'，把 console.log('promise2')放到 micro 队列中。
1.5 执行 console.log('script end')，输出'script end'。

2.macrotask 执行完会执行 microtask，把 microtask quene 里面的 microtask 全部拿出来一次性执行完，所以会输出'async1 end'和'promise2'。

3.开始新一轮事件循环，取出一个 macrotask 执行，所以会输出'setTimeout'。
