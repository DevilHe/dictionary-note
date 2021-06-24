##### 如何获取 html 元素实际的样式值

```
<div id="myDiv" style="background-color: red;border: 1px solid black;"></div>

<style>
#myDiv {
  background-color: blue;
  width: 100px;
  height: 200px;
}
</style>

```

###### 题解

实际的样式值，可以理解为浏览器的计算样式
style 对象中包含支持 style 属性的元素为这个属性设置的样式信息，但不包含从其他样式表层叠继承的同样影响该元素的样式信息。

DOM2 Style 在 document.defaultView 上增加了 getComputedStyle()方法。这个方法接收两个参数：要取得计算样式的元素和伪元素字符串（如:':after'）。如果不需要查询伪元素，则第二个参数可以传 null。getComputedStyle()方法返回一个 CSSStyleDeclaration 对象（与 style 属性的类型一样），包含元素的计算属性。

```
let myDiv = document.getElementById('myDiv');
let computedStyle = document.defaultView.getComputedStyle(myDiv, null);
console.log('computedStyle', computedStyle);
console.log(computedStyle.backgroundColor); // rgb(255, 0, 0)
console.log(computedStyle.width); // 100px
console.log(computedStyle.height); // 200px
console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

// 兼容写法
function getStyleByAttr(obj, name) {
  return window.getComputedStyle ? window.getComputedStyle(obj, null)[name] : obj.currentStyle[name];
}
let node = document.getElementById('myDiv');
console.log(getStyleByAttr(node, 'backgroundColor'));
console.log(getStyleByAttr(node, 'width'));
console.log(getStyleByAttr(node, 'height'));
console.log(getStyleByAttr(node, 'border'));

```
