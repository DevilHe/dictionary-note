#### 动手实现一个 repeat 方法

```
function repeat(func, times, wait) {
  // TODO
}
const repeatFunc = repeat(alert, 4, 3000)
// 调用这个 repeatFunc('HelloWorld')，会输出4次 HelloWorld，每次间隔3秒

```

##### 代码实现

1.实现方式一

```
function repeat(func, times, wait) {
  if(typeof func !== 'function') return;
  if(times <= 0) return;
  return value => {
    let timesTmp = times;
    let interval = setInterval(() => {
      func(value);
      timesTmp --;
      timesTmp === 0 && clearInterval(interval)
    }, wait);
  }
}
const repeatFunc = repeat(console.log, 4, 3000);
repeatFunc('HelloWorld')

```

2.实现方式二

```
function repeat(func, times, wait) {
  return function(...args) {
    let i = 0;
    let _args = [...args];
    let handle = setInterval(() => {
      i += 1;
      if(i > times) {
        clearInterval(handle);
        return;
      }
      console.log('func', func)
      func.apply(null, _args);
    }, wait);
  }
}
const repeatFunc = repeat(console.log, 4, 3000);
repeatFunc('HelloWorld')

```
