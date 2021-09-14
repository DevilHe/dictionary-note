#### 1、加载 js || css || style

```
const loadRes = function(name, type, fn) { // 加载js || css || style
  let ref
  if (type === 'js') { // 外部js
    ref = document.createElement('script')
    ref.setAttribute('type', 'text/JavaScript')
    ref.setAttribute('src', name)
  } else if (type === 'css') { // 外部css
    ref = document.createElement('link')
    ref.setAttribute('rel', 'stylesheet')
    ref.setAttribute('type', 'text/css')
    ref.setAttribute('href', name)
  } else if (type === 'style') { // style
    ref = document.createElement('style')
    ref.innerhtml = name
  }
  if (typeof ref !== 'undefined') {
    document.getElementsByTagName('head')[0].appendChild(ref)
    ref.onload = function() { // 加载完成执行
      typeof fn === 'function' && fn()
    }
  }
}

// name:路径 (style ?)
loadRes('name', 'js') // <script type="text/JavaScript" src="name"></script>

```

#### 2、获取 url 参数

```
const getUrlParam = function(name) { // 获取url参数
  let reg = new RegExp('(^|&?)' + name + '=([^&]*)(&|$)', 'i')
  // window.location.href 本地url地址
  let r = window.location.href.substr(1).match(reg)
  if (r != null) {
    return decodeURI(r[2])
  }
  return undefined
}

const getUrlParam = function(name) { // 获取url参数
  let reg = new RegExp('(^|&?)' + name + '=([^&]*)(&|$)', 'i')
  // window.location.href 本地url地址
  // let r = window.location.href.substr(1).match(reg)
  let r = 'http://localhost:8080/server_list?a=111'.substr(1).match(reg)
  if (r != null) {
    return decodeURI(r[2])
  }
  return undefined
}
console.log('getUrlParam', getUrlParam('a')) // 111

```

#### 3、本地存储

```
const store = { // 本地存储
  set: function(name, value, day) { // 设置
    let d = new Date()
    let time = 0
    day = (typeof(day) === 'undefined' || !day) ? 1 : day // 时间,默认存储1天
    time = d.setHours(d.getHours() + (24 * day)) // 毫秒
    localStorage.setItem(name, JSON.stringify({
      data: value,
      time: time
    }))
  },
  get: function(name) { // 获取
    let data = localStorage.getItem(name)
    if (!data) {
      return null
    }
    let obj = JSON.parse(data)
    if (new Date().getTime() > obj.time) { // 过期
      localStorage.removeItem(name)
      return null
    } else {
      return obj.data
    }
  },
  clear: function(name) { // 清空
    if (name) { // 删除键为name的缓存
      localStorage.removeItem(name)
    } else { // 清空全部
      localStorage.clear()
    }
  }
}

// store.set('name', 'Jim', 2)
// console.log(store.get('name'))
// store.clear()

```

#### 4、cookie 操作 [set, get, del]

```
const cookie = { // cookie操作【set，get，del】
  set: function(name, value, day) {
    let oDate = new Date()
    oDate.setDate(oDate.getDate() + (day || 30))
    document.cookie = name + '=' + value + ';expires=' + oDate + "; path=/;"
  },
  get: function(name) {
    let str = document.cookie
    let arr = str.split('; ')
    for (let i = 0; i < arr.length; i++) {
      let newArr = arr[i].split('=')
      if (newArr[0] === name) {
        return newArr[1]
      }
    }
  },
  del: function(name) {
    this.set(name, '', -1)
  }
}

cookie.set('name', 'mingcheng', 3)

```

**注意！！！**

本地文件直接访问 html，如 file:///E:/index.html 访问时，设置的 document.cookie,读取时一直会显示空字符串！而开 Apache 时：localhost:8080/index.html 或者 127.0.0.1:8080/index.html 可以设置 [参考](https://www.cnblogs.com/why-not-try/p/8028794.html)
It did not set a cookie when opened as a file but worked every time when opened from the server.

#### 5、JS 获取元素样式[支持内联]

```

```

#### 6、时间格式化

```
const formatDate = function(fmt, date) { // 时间格式化 【'yyyy-MM-dd hh:mm:ss',时间】
  if (typeof date !== 'object') {
    date = !date ? new Date() : new Date(date)
  }
  var o = {
    'M+': date.getMonth() + 1, // 月份
    'd+': date.getDate(), // 日
    'h+': date.getHours(), // 小时
    'm+': date.getMinutes(), // 分
    's+': date.getSeconds(), // 秒
    'q+': Math.floor((date.getMonth() + 3) / 3), // 季度
    'S': date.getMilliseconds() // 毫秒
  }
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  for (var k in o) {
    if (new RegExp('(' + k + ')').test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
    }
  }
  return fmt
}

console.log('formatDate', formatDate('yyyy-MM-dd hh:mm:ss')) // 2021-09-13 10:03:39
console.log('formatDate', formatDate('yyyy-MM-dd hh:mm:ss', 1630044530000)) // 2021-08-27 14:08:50

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```

#### 、

```

```
