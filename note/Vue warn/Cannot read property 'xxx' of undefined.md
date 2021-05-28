**Error in render: "TypeError: Cannot read property 'xxx' of undefined."**
翻译：呈现错误：“ TypeError：无法读取未定义的属性'xxx'。

vue html 模板在渲染时候，读取对象中的某个对象的属性值时，这个对象不存在，说通俗点就是三层表达式 a.b.c，在对象 a 中没有对象 b，那么读取对象 a.b.c 中的值，自然会报错。如果是两层表达式 a.b 则不会报错，返回的是 undefined。

```

<div
  v-if="
    a.b[c] &&
    a.b[c] !== ''
  "
>{{ a.b[c] }}</div>

<!-- 修改：加判断条件 -->

<div
  v-if="
    a.b &&
    a.b[c] &&
    a.b[c] !== ''
  "
>{{ a.b[c] }}</div>

```

代码搬运工
邮箱：wlh929673054@163.com
