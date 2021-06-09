#### reduce 用法

arr.reduce(function(prev,cur,index,arr){}, init);
其中，
arr 表示原数组；
prev 表示上一次调用回调时的返回值，或者初始值 init;
cur 表示当前正在处理的数组元素；
index 表示当前正在处理的数组元素的索引，若提供 init 值，则索引为 0，否则索引为 1；
init 表示初始值。
常用的参数只有两个：prev 和 cur

##### 1. 求数组项之和

```
var arr1 = [4,7,23,17,25,8,53,9,66]
var sum1 = arr1.reduce((prev, cur) => {
  // console.log('初始值为0', prev, cur)
  return prev + cur;
}, 0);
// var sum1 = arr1.reduce((prev, cur) =>  prev + cur, 0);

var sum2 = arr1.reduce((prev, cur) => {
  // console.log('初始值为arr1数组首项的值', prev, cur)
  return prev + cur;
});
console.log('数组之和', sum1, sum2) // 212 212

```

```
const arr = [12, 34, 23];
const sum1 = arr.reduce((total, num) => total + num);
// 设定初始值求和
const sum2 = arr.reduce((total, num) => total + num, 10);  // 以10为初始值求和
console.log('求和', sum1, sum2) // 69 79

// 对象数组求和
var result = [
  { subject: 'math', score: 88 },
  { subject: 'chinese', score: 95 },
  { subject: 'english', score: 80 }
];
const sum11 = result.reduce((accumulator, cur) => accumulator + cur.score, 0);
const sum12 = result.reduce((accumulator, cur) => accumulator + cur.score, -10);  // 总分扣除10分
console.log('对象数组求和', sum11, sum12) // 263 253

```

##### 2. 求数组项最大/小值

```
var arr2 = [24,7,23,117,25,8,53,9,66]
var max = arr2.reduce((prev, cur) => {
  // return prev > cur ? prev : cur;
  return Math.max(prev,cur);
});
var min = arr2.reduce((prev, cur) => {
  return Math.min(prev,cur);
});
console.log('数组最大值', max, '数组最小值', min) // 117 7

```

Math.max.apply(null, arr)求最大值

```
let a = [5,9,3,25,16]
let b = [1,2,3,[5,6],[1,4,8]]
let c = b.join(",").split(","); //转化为一维数组
const maxA = Math.max.apply(null, a)
const minA = Math.min.apply(null, a)
const maxA2 = Math.max.apply(null, c)
const minA2 = Math.min.apply(null, c)
console.log('最大值', maxA, '最小值', minA); // 25 3
console.log('最值', maxA2, minA2); // 8 1

```

对象数组中的 count 的最大值

```
var lists = [
  {id: 1, count: 22},
  {id: 2, count: 43},
  {id: 3, count: 14}
]
var maxResult = Math.max(...lists.map(x => x.count));
var objMax = lists.find(function (obj) {
  return obj.count === maxResult;
})
console.log('lists最大值', maxResult, objMax) // 43 {id: 2, count: 43}
// console.log('lists最大值', maxResult, objMax, Math.floor(maxResult), Math.ceil(maxResult))

```

##### 3. 数组去重

```
var arr3 = [4,7,23,17,23,8,53,8,66]
var newArr = arr3.reduce((prev, cur) => {
  prev.indexOf(cur) === -1 && prev.push(cur);
  return prev;
}, []);
console.log('数组去重', newArr) // [4, 7, 23, 17, 8, 53, 66]

```

对象数组去重

```
const hash = {};
var chatlists = [
  {topic: 'a', stream_id: 1, index: 11},
  {topic: 'b', stream_id: 2, index: 22},
  {topic: 'c', stream_id: 3, index: 33},
  {topic: 'd', stream_id: 1, index: 44}
]
const chat = chatlists.reduce((obj, next) => {
  const hashId = `${next.stream_id}`; // 定义唯一hashId
  if (!hash[hashId]) {
    hash[`${next.stream_id}`] = true;
    obj.push(next);
  }

 	// const hashId = `${next.topic}_${next.stream_id}`;
 	// if (!hash[hashId]) {
  //   hash[`${next.topic}_${next.stream_id}`] = true;
  //   obj.push(next);
 	// }
  return obj;
}, []);
console.log('对象数组去重', chat)
// [
//   {topic: 'a', stream_id: 1, index: 11},
//   {topic: 'b', stream_id: 2, index: 22},
//   {topic: 'c', stream_id: 3, index: 33}
// ]

```

##### 4. 其他

###### 数组对象中的用法,比如生成“老大、老二、老三和老四”

```
const objArr = [{name: '老大'}, {name: '老二'}, {name: '老三'}, {name: '老四'}];
const res1 = objArr.reduce((pre, cur, index, arr) => {
  if (index === 0) {
    return cur.name;
  } else if (index === (arr.length - 1)) {
    return pre + '和' + cur.name;
  } else {
    return pre + '、' + cur.name;
  }
}, '');
console.log('数组对象中的用法', res1) // 老大、老二、老三和老四

```

把 name 值生成数组

```
const objArr = [{name: '老大'}, {name: '老二'}, {name: '老三'}, {name: '老四'}];
const resArr = objArr.reduce((pre, cur) => {
  pre.push(cur.name)
  return pre;
}, []);
console.log('数组对象中的用法2', resArr) // ["老大", "老二", "老三", "老四"]

```

###### 求字符串中字母出现的次数

```
const str2 = 'sfjsfjfs';
const res2 = str2.split('').reduce((accumulator, cur) => {
  accumulator[cur] ? accumulator[cur]++ : accumulator[cur] = 1;
  return accumulator;
}, {});
console.log('字符串中字母出现的次数', res2) // {s: 3, f: 3, j: 2}

```

###### 数组转数组 按照一定的规则转成数组

```
var arrpf = [2, 3, 4, 5, 6]; // 求每个值的平方
var newarrpf = arrpf.reduce((accumulator, cur) => {
  accumulator.push(cur * cur);
  return accumulator;
}, []);
console.log('平方', newarrpf) // [4, 9, 16, 25, 36]

```

###### 数组转对象 按照 id 取出 stream

```
var streams = [{name: '技术', id: 'a'}, {name: '设计', id: 'c'}, {name: '贸易', id: 'b'}];
var obj = streams.reduce((accumulator, cur) => {
  accumulator[cur.id] = cur;
  return accumulator;
}, {});
console.log('数组转对象', obj, obj.a)
// {
//   a: {name: "技术", id: "a"},
//   b: {name: "贸易", id: "b"},
//   c: {name: "设计", id: "c"}
// }
// {name: "技术", id: "a"}

```

###### 多维的叠加执行操作 各科成绩占比重不一样，求结果

```
var resultA = [
  { subject: 'math', score: 100 },
  { subject: 'chinese', score: 90 },
  { subject: 'english', score: 80 }
];
var dis = {
  math: 0.5,
  chinese: 0.3,
  english: 0.2
};
var resA = resultA.reduce((accumulator, cur) => dis[cur.subject] * cur.score + accumulator, 0);
console.log('多维的叠加', resA) // 93

```

###### 加大难度，商品对应不同国家汇率不同，求总价格

```
var prices = [{price: 23}, {price: 45}, {price: 56}];
var rates = {
  us: '6.5',
  eu: '7.5',
};
var initialState = {usTotal:0, euTotal: 0};
var resP = prices.reduce((accumulator, cur1) => Object.keys(rates).reduce((prev2, cur2) => {
  // console.log(accumulator, cur1, prev2, cur2);
  accumulator[`${cur2}Total`] += cur1.price * rates[cur2];
  return accumulator;
}, {}), initialState);
console.log('汇率', resP) // {usTotal: 806, euTotal: 930}

var prices2 = [{price: 23}, {price: 45}, {price: 56}];
var rates2 = {
  us: '6.5',
  eu: '7.5',
};
var initialState2 = {usTotal:0, euTotal: 0};
var manageReducers = function() {
  return function(state, item) {
    return Object.keys(rates2).reduce((nextState, key) => {
      state[`${key}Total`] += item.price * rates2[key];
      return state;
    }, {});
  }
};
var resP2= prices2.reduce(manageReducers(), initialState2);
console.log('汇率2', resP2) // {usTotal: 806, euTotal: 930}

```

###### 扁平一个二维数组

```
var arrbp = [[1, 2, 8], [3, 4, 9], [5, 6, 10]];
var resbp = arrbp.reduce((x, y) => x.concat(y), []);
console.log('扁平一个二维数组', resbp) // [1, 2, 8, 3, 4, 9, 5, 6, 10]

// 多维数组
var arr1 = [1, 2, [3], [1, 2, 3, [4, [2, 3, 4]]]];
const flatten = arr => {
  return arr.reduce((pre, cur) => {
    return pre.concat(Array.isArray(cur) ? flatten(cur) : cur);
  }, []);
}
console.log(flatten(arr1)); //[1, 2, 3, 1, 2, 3, 4, 2, 3, 4]

```

###### map 映射

```
let resM = [
  {name: '名字1', sex: 1, age: 19, img: 'aaa'},
  {name: '名字2', sex: 0, age: 23, img: 'bbb'},
  {name: '名字3', sex: '', age: 25, img: 'ccc'}
]
let r1 = resM.map(item => {
  return {
    title: item.name,
    sex: item.sex === 1 ? '男' : item.sex === 0 ? '女' : '保密',
    age: item.age,
    avatar: item.img
  }
})
console.log('map1', r1)
// [
//   {title: '名字1', sex: '男', age: 19, avatar: 'aaa'},
//   {title: '名字2', sex: '女', age: 23, avatar: 'bbb'},
//   {title: '名字3', sex: '保密', age: 25, avatar: 'ccc'}
// ]


let resN = [
  {login: '名字1', avatar_url: 19, html_url: 'aaa'},
  {login: '名字2', avatar_url: 23, html_url: 'bbb'}
]

const users = resN.map(item => ({
  url: item.html_url,
  img: item.avatar_url,
  name: item.login,
}));
console.log('map2', users)
// [
//   {name: '名字1', img: 19, url: 'aaa'},
//   {name: '名字2', img: 23, url: 'bbb'}
// ]

```
