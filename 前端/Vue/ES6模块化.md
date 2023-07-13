# ES6模块化与异步编程

1. ES6模块化规范中定义：
* 每个js文件都是一个独立的模块
* 导入其他模块成员使用import关键字
* 向外共享模块成员使用export关键字

2. nodejs中配置ES6模块化
* 确保版本在v14.15.1更高版本
* 在package.json的根节点中添加"type":"module"节点

## 1. ES6语法

### 1.1 默认导出和默认导入

### 1.2 按需导出和按需导入

1. export 按需导出的成员

2. import { s1 } from '模块标识符'

### 1.3 直接导入并执行模块中的代码

> import 'xxx.js'

## 2. Promise

1. 回调地域：多层回调函数的相互嵌套

2. Promise
* 是一个构造函数
    * 可以创建Promise的实例 ``const p = new Promise()``
    * new出来的Promise实例对象，代表一个异步操作
* Promise.prototype上包含一个.then()方法
* .then()方法用来预先指定成功和失败的回调函数
    * p.then(result => {}, error => {})
    * 调用.then()方法时，成功的回调是必选的，失败的回调是可选的

3. 基于then-fs读取文件内容
> npm install then-fs
```js
import thenFs from 'then-fs'

thenFs.readfile('./files/1.txt','utf-8').then((r1) => {console.log(r1)})  // 此代码无法保证文件的读取顺序

// 基于Promise按顺序读取文件的内容, Promise支持链式调用，从而来解决回调地狱的问题
thenFs.readfile('./files/1.txt', 'utf-8')
    .then((r1) => {
      console.log(r1)
      return thenFs.readfile('./files/2.txt', 'utf-8')
    })
    .then((r2) => {
      console.log(r2)
      return thenFs.readfile('./files/3.txt', 'utf-8')
})
    .then((r3) => {
      console.log(r3)
})
    // Promise的链式操作中如果发生错误，可以使用Promise.prototype.catch方法进行捕获和处理 
    .catch(err => {
      console.log(err.message)
})
```

4. Promise.all()

Promise.all()方法会发起并行的Promise异步操作，等所有的异步操作全部结束后才会执行下一步的.then操作
```js
const promiseArr = [
  thenFs.readfile('./files/1.txt', 'utf-8'),
  thenFs.readfile('./files/2.txt', 'utf-8'),
  thenFs.readfile('./files/3.txt', 'utf-8')
]

Promise.all(promiseArr)
    .then(([r1, r2, r3]) => {
      console.log(r1, r2, r3)
    })
    .catch(err => {
      console.log(err.message)
    })
```

5. Promise.race()

Promise.race()方法会发起并行的Promise异步操作，只要任何一个异步操作完成，就立即执行下一步的.then操作

6. 基于Promise封装读文件的方法

方法的封装要求：
* 方法的名称要定义为getFile
* 方法接收一个形参fpath，表示要读取的文件的路径
* 方法的返回值为Promise实例对象
```js
function getfile(fpath) {
    return new Promise(function () {
      fs.readFile(fpath, 'utf-8', (err, dataStr) => {  })
    })
}
```

7. 通过.then()指定的成功和失败的回调函数，可以在function的形参中进行接收
```js
function getfile(fpath) {
    return new Promise(function (resolve, reject) {
      fs.readFile(fpath, 'utf-8', (err, dataStr) => {  
          if(err) return reject(err)  // 如果读取失败，则调用"失败的回调函数"
          resolve(dataStr)  // 如果读取成功，则调用"成功的回调函数"
      })
    })
}

// getFile 方法的调用过程
getfile('./files/1.txt').then(成功的回调函数，失败的回调函数)
```

## 3. async/await

注意事项：
* 如果在function中使用了await，则function必须被async修饰
* 在async方法中，第一个await之前的代码会同步执行，await之后的代码会异步执行
```js
console.log('A')
async function getAllFile() {
  console.log('B')
    const r1 = await thenFs.readfile('./files/1.txt', 'utf-8')
    console.log(r1)
  console.log('D')
}

getAllFile()
console.log('C')

// 输出顺序
A
B
C
111
D
```

## 4. EventLoop

1. 同步任务
* 非耗时任务，指的是在主线程上排队执行的那些任务
* 只有前一个任务执行完毕，才能执行后一个任务
2. 异步任务
* 耗时任务，异步任务由JavaScript委托给宿主环境进行执行
* 当异步任务执行完成后，会通知JavaScript主线程执行异步任务的回调函数

## 5. 宏任务和微任务
JavaScript把异步任务又做了进一步的划分，异步任务又分为两类：
1. 宏任务
* 异步Ajax请求
* setTimeout、setInterval
* 文件操作
* 其他宏任务
2. 微任务
* Promise.then、.catch、.finally
* process.nextTick
* 其他微任务
> 每一个宏任务执行完之后，都会检查是否存在待执行的微任务
