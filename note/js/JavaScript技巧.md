#### JavaScript 技巧

应用这些技巧，可以使你的代码更具可读性、简洁性和专业性。

##### 1.单行 if-else 语句（三目运算符）

旨在使代码更简洁，运用在简单地 if-else 语句中，不可过度使用，否则会降低代码的可读性！

```
const age = 12;
let ageGroup;

// LONG FORM
if(age > 18) {
  ageGroup = 'An adult';
} else {
  ageGroup = 'An child';
}

// SHORTHAND
ageGroup = age > 18 ? 'An adult' : 'An child';

```

##### 2.空合并

空合并运算符??，如果左侧未定义，则返回右侧；如果是，则返回左侧。

```
let maybeSomething;

// LONG FORM
if(maybeSomething) {
  console.log(maybeSomething);
} else {
  console.log('Nothing found');
}

// SHORTHAND
console.log(maybeSomething ?? 'Nothing found');

```

##### 3.可选链

如果使用运算符访问对象的属性，但未定义该属性，则会引发错误，这是使用可选链的地方。
如果使用可选链运算符?，并且属性未定义，返回 undefined 而不是抛出错误。

```
const students = {
  name: 'Jim',
  age: 18,
  address: {
    state: 'New York'
  }
}

// LONG FORM
console.log(students && students.address && students.address.ZIPCode); // Doesn't exist - Returns undefined

// SHORTHAND
console.log(students?.address?.ZIPCode); // Doesn't exist - Returns undefined

```

##### 4.将任何值转换为布尔值

使用双感叹号(!!)在 JS 中将任何内容转换为布尔值。

```
!!true  // true
!!2  // true
!![]  // true
!!'Test'  // true
!!' '  // true

!!false  // false
!!0  // false
!!''  // false

```

##### 5.扩展运算符

连接两个数组

```
const num1 = [1, 2, 3];
const num2 = [4, 5, 6];

// LONG FORM
let newArr = num1.concat(num2);

// SHORTHAND
let newArr1 = [...num1, ...num2];

```

将值推送到数组

```
let numbers = [1, 2, 3];

// LONG FORM
numbers.push(4);
numbers.push(5);

// SHORTHAND
numbers = [...numbers, 4, 5];

```

##### 6.使用扩展运算符进行解构

使用带有解构的扩展运算符将剩余元素分配给新变量

```
const student = {
  name: 'Lucy',
  age: 22,
  city: 'Helsinki',
  state: 'Finland'
};

// LONG FORM
const name = student.name;
const age = student.age;
const address = {
  city: student.city,
  state: student.state
};

// SHORTHAND
const { name, age, ...address } = student;
// city和state属性被分配给一个address常量作为剩余部分

```

##### 7.从数组中删除重复项

```
const numbers = [1, 1, 20, 3, 3, 3, 9, 9];
const uniqueNumbers = [...new Set(numbers)];
// console.log(uniqueNumbers) // [1, 20, 3, 9]

```

##### 8.使用&&进行短路评估

不必用 if 语句检查某事是否为真，可以使用&&运算符

```
var isReady = true;

function doSomething() {
  console.log('Yay!');
}

// LONG FORM
if(isReady) {
  doSomething();
}

// SHORTHAND
isReady && doSomething();

```

##### 9.类固醇的字符串

通过将字符串包装在反引号内并使用${}来嵌入值来直接将变量嵌入到字符串中

```
const age = 41;
const sentence = `I'm ${age} years old`;
console.log(sentence) // I'm 41 years old

```

##### 10.Switch-Case 更短的替代方案

使用具有与键关联的函数名称的对象来替换 switch 语句

```
const num = 3

// LONGER FORM
switch (num) {
  case 1: someFunction();
  break;
  case 2: someOtherFunction();
  break;
  case 3: yetAnotherFunction();
  break;
}

// SHORTHAND
var cases = {
  1: someFunction,
  2: someOtherFunction,
  3: yetAnotherFunction
};
cases[num]();
// 注意在cases对象中调用函数！

```

##### 11.对象属性分配

如果你希望对象键与值具有相同的名称，则可以省略对象文字

```
const name = "Luis", city = "Paris", age = 43, favoriteFood = "Spaghetti";

// LONGER FORM
const person = { name: name, city: city, age: age, favoriteFood: favoriteFood };

// SHORTHAND
const person = { name, city, age, favoriteFood };

```

##### 12.箭头函数

与使用 function 关键字声明函数不同，使用箭头函数语法会更清晰

```
// LONGER FORM
function greetLong(name) {
  console.log(`Hi, ${name}`);
}

// SHORTHAND
const greetShort = name => console.log(`Hi, ${name}`);
// 注意：在本例中，由于函数中只有一个表达式，你可以省略大括号 ( {})。但是，如果表达式需要多于一行，则需要使用大括号！

```

##### 13.不带返回关键字返回

使用箭头函数时，如果 return 函数中只有一个表达式，则可以省略关键字和函数的花括号：

```
// LONGER FORM
function toPoundsLong(kilos) {
  return kilos * 2.205;
}

// SHORTHAND
const toPoundsShort = kilos => kilos * 2.205;

```

##### 14.更短的 for 循环

有时可能希望使用内置 forEach()方法更简洁地循环，而不是在集合上使用循环

```
const numbers = [1, 2, 3, 4, 5];

// LONGER FORM
for(let i = 0; i < numbers.length; i++){
  console.log(numbers[i]);
}

// SHORTHAND
numbers.forEach(number => console.log(number));

```

for of 对 for 循环简写

```
let song = ['eenine', 'meenie', 'miney', 'mo'];
// LONGER FORM
for(let i=0;i<song.length;i++) {
  console.log(song[i]);
}
// SHORTHAND
for(const word of song) {
  console.log(word);
}

```

##### 15.函数参数的默认值

在 JavaScript 中，可以为函数参数提供默认值，以便可以带或不带参数调用函数

```
// LONG FORM
function pickUp(fruit) {
  if(fruit === undefined) {
    console.log("I picked up a Banana");
  } else {
    console.log(`I picked up a ${fruit}`);
  }
}

// SHORTHAND
function pickUp(fruit = "Banana") {
  console.log(`I picked up a ${fruit}`)
}

pickUp("Mango"); // I picked up a Mango

// 使用箭头函数使其更短
const pick = (fruit = "Banana") => `I picked up a ${fruit}`
console.log(pick()) // I picked up a Banana

```

##### 16.声明变量

像这样巧妙地将变量声明组合成一行

```
// LONGER FORM
let name;
let age;
let favoriteFood = "Pizza";

// SHORTHAND
let name, age, favoriteFood = "Pizza";

```

##### 17.将值放入数组

使用 Object.values()获取对象的值并将它们放入数组而不是循环

```
const info = { name: "Matt", country: "Finland", age: 35 };

// LONGER FORM
let data = [];
for (let key in info) {
  data.push(info[key]);
}

// SHORTHAND
const data = Object.values(info);

console.log(data) // ["Matt", "Finland", 35]

```

##### 18.在数组中查找元素

使用数组的内置 find()方法查找与特定条件匹配的元素，而不是使用冗长的循环

```
const fruits = [
  { type: "Banana", color: "Yellow" },
  { type: "Apple", color: "Green" }
];

// LONGER FORM
let yellowFruit;
for (let i = 0; i < fruits.length; ++i) {
  if (fruits[i].color === "Yellow") {
    yellowFruit = fruits[i];
  }
}

// SHORTHAND
yellowFruit = fruits.find((fruit) => fruit.color === "Yellow");

console.log('yellowFruit', yellowFruit) // {type: "Banana", color: "Yellow"}

```

##### 19.检查一个项目是否在数组中

使用 includes() 方法，而不是使用 indexOf() 方法来检查元素是否在数组中。

```
let numbers = [1, 2, 3];

// LONGER FORM
const hasNumber1 = numbers.indexOf(1) > -1 // true

// SHORTHAND/CLEANER APPROACH
const hasNumber1 = numbers.includes(1) // true

```

##### 20.检查多个条件

避免长|| 检查多个条件链时，可以使用刚刚在上一个技巧中学到的东西——即，使用 includes() 方法

```
const num = 1;

// LONGER FORM
if(num == 1 || num == 2 || num == 3){
  console.log("Yay");
}

// SHORTHAND
if([1,2,3].includes(num)){
  console.log("Yay");
}

```

##### 21.使用一行分配多个值

使用解构来一次性分配多个

```
let num1, num2;

// LONGER FORM
num1 = 10;
num2 = 100;

// SHORTHAND
[num1, num2] = [10, 100];

```

也可以在处理对象时使用

```
student = { name: "Matt", age: 29 };

// LONGER FORM
let name = student.name;
let age = student.age;

// SHORTHAND
let { name, age } = student;

```

##### 22.在没有第三个变量的情况下交换两个变量

在 JavaScript 中，可以使用解构从数组中提取值。这也可用于在没有第三个帮助程序的情况下交换两个变量

```
let x = 1;
let y = 2;

// LONGER FORM
let temp = x;
x = y;
y = temp;

// SHORTHAND
[x, y] = [y, x];

```

##### 23.指数运算符

可以使用 \*\* 运算符，而不是使用 Math.pow() 函数来将数字求幂

```
// LONGER FORM
Math.pow(4,2); // 16
Math.pow(2,3); // 8

// SHORTHAND
4**2 // 16
2**3 // 8

```

##### 24.舍入数字时 Math.floor 的简写

可以使用 ~~ 运算符，而不是使用 Math.floor() 函数来向下舍入数字

```
// LONG FORM
Math.floor(5.25) // 5.0

// SHORTHAND
~~5.25 // 5.0

```

##### 25.将字符串转换为数字

可以使用一元运算符 ( +) 将字符串转换为数字

```
// LONGER FORM
const num = parseInt("1000");
const num = Number("1000");

// SHORTHAND
const num = +"1000";

console.log(num, typeof num) // 1000 "number"

```

##### 26.区间随机数

有时需要在一个范围内生成一个随机数。该 Math.random()函数可以生成一个随机数，然后，可以将其转换为我们想要的范围。

```
const randomIntFromInterval = (min, max) => Math.floor(Math.random() * (max - min + 1) + min);
console.log('randomIntFromInterval', randomIntFromInterval(1, 9))

```

##### 27.动态属性名称

ES6 提供动态属性名称，允许对象字面量的属性键使用表达式。通过用括号包围键[]，可以使用变量作为属性键。
这个在希望动态创建秘钥的情况下很有用

```
const type = 'fruit';
const item = {
  [type]: 'kiwi'
}
console.log(item) // {fruit: "kiwi"}

// 这个在希望动态创建秘钥的情况下很有用
// 我们可以使用括号表示法访问该值
item[type]; // 'kiwi'
item['fruit']; // 'kiwi'

// 或使用点符号
item.fruit; // 'kiwi'

```

##### 28.ES6 扩展运算符和 slice

如果我们想在不改变原数组的情况下向数组添加一个新项目（通常希望避免这种情况），可以使用 ES6 扩展运算符和 slice 创建一个新数组。

```
const insert = (arr, index, newItem) => [
  ...arr.slice(0, index),
  newItem,
  ...arr.slice(index)
]

const items = ['S', 'L', 'C', 'E'];
const result = insert(items, 2, 'I');
console.log(items, result)
// ["S", "L", "C", "E"]   ["S", "L", "I", "C", "E"]

```

##### 29.十进制基数

e 后面的数字代表 1 后面 0 的个数

```
// LONGER FORM
for(let i=0;i<1000000;i++) {
  console.log('I love my cat!');
}

// SHORTHAND
for(let i=0;i<1e2;i++) {
  console.log('I love my cat!');
}

```

##### 30.反引号``模板文字${}

```
let poem_name = 'Baba Black Sheep';
console.log(`My favorite poem is ${poem_name}`);

const msg_to_cat = 'You are the best cat in the whole world.\n' + 'You are the best cat in the whole world.';
const msg_to_dog = `You are the goodest boy ever.
You are the goodest boy ever.`;

```

##### 31.默认参数

```
const sum_genetator = (a, b=10) => a + b;
console.log('sum_genetator', sum_genetator(2)); // 12
console.log('sum_genetator', sum_genetator(2, undefined)); // 12
console.log('sum_genetator', sum_genetator(2, 4)); // 6

```

##### 32.强制性参数

```
const error_handler = (property) => {
  throw new Error(`Error: ${property} is required!`);
}
const monitor_roads = (car = error_handler('Car'), speed = error_handler('Speed')) => {
  console.log(`Your ${car} was going at a speed of ${speed}mph.`);
}
monitor_roads('Ferrari', 240); // Your Ferrari was going at a speed of 240mph.
monitor_roads('Ferrari'); // Error: Speed is required!

```

##### 32.对象键、对象值

Object.keys()迭代对象获取键名，Object.values()获取对象值

```
let cat = {
  name: 'Jimmy boy',
  age: 5,
  breed: 'British Shorthair',
  favorite_word: 'Meow',
  favorite_food: 'Chimkens'
}
console.log(Object.keys(cat)); // ["name", "age", "breed", "favorite_word", "favorite_food"]
console.log(Object.values(cat)); // ["Jimmy boy", 5, "British Shorthair", "Meow", "Chimkens"]

```

##### 32.字符串比较

数组去重，localeCompare

```
let names = ['Aron', 'Elon', 'Tom', 'Tom'];
let result = [];
const remove_dupes = arr => {
  for(let i=0;i<arr.length;i++) {
    if(arr[i].localeCompare(arr[i+1]) != 0) {
      result.push(arr[i]);
    }
  }
  return result;
}
console.log('remove_dupes', remove_dupes(names))

```
