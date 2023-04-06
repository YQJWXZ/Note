# JS高级

* 面向过程(POP)
* 面向对象(OOP)

## 1. ES6中的类和对象

类抽象了对象的公共部分，它泛指某一大类(class)
对象特指某一个，通过类实例化一个具体的对象

> 在ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象
>
> 类里面的共有的属性和方法一定要加this使用

#### 类里面this指向问题

> constructor里面的this指向实例对象，方法里面的this指向这个方法的调用者

1. 创建类
2. 类添加方法

```js
    class Star {
    // constructor()方法是类的构造函数(默认方法),用于传递参数，返回实例对象
    // 通过new命令生成对象实例时，自动调用该方法，没有定义的话类内部会自动创建
    constructor(uname) {
        this.uname = uname;
    }

    // 类添加方法
    say(value) {

    }
}

var xx = new Star('刘德华');
```

## 2. 类的继承

> super必须放到this之前

```js
    class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    sum() {
        console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x, y) {
        super(x, y);
        this.x = x;
        this.y = y;
    }
}

var son = new Son(1, 2);
son.sum();
```

## 3. 构造函数和原型

### 3.1 构造函数

1. 创建对象的方式

* 对象字面量
* new Object()
* 自定义构造函数

> 构造函数用于创建某一类对象，其首字母要大写

2. 静态成员和实例成员
   实例成员：就是构造函数内部通过this添加的成员
   静态成员：在构造函数本身上添加的成员

### 3.2 原型

1. 构造函数原型(prototype)--> 就是一个对象

* 构造函数通过原型分配的函数是所有对象所共享的
* *我们可以把那些不变的方法，直接定义在prototype对象上，这样所有对象的实例就可以共享这些方法*

2. 对象原型(__ptoto__)

* 对象都有一个属性__proto__指向构造函数的prototype原型对象，之所以我们对象可以使用构造函数prototype原型对象的属性和方法，就是因为对象有__ptoto__原型的存在
* __proto__对象原型和原型对象prototype是等价的 ``显式原型与隐式原型``

#### 3.2.1 constructor构造函数

1. constructor主要用于纪录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数
2. 很多情况下，我们需要手动的利用constructor 这个属性指回 原来的构造函数

```js
    Star.prototype = {
    // 如果我们修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动的利用constructor指回原来的构造函数
    constructor: Star,
    sing: function () {
        // ......  
    },
    movie: function () {
        //  ......   
    }
}
```

#### 3.2.2 构造函数、实例、原型对象三者之间的关系

### 3.3 原型链

1. 对象成员查找规则
   就近原则

2. 原型对象this指向

* 构造函数中，里面this指向的是对象实例
* 原型对象函数里面的this指向的是实例对象

3. 拓展内置对象
   可以通过原型对象，对原来的内置对象进行扩展自定义的方法

## 4. 继承

> ES6之前，没有extends，通过构造函数+原型对象模拟实现继承(组合继承)

1. call()

* 调用这个函数，并且修改函数运行时的this走向
    * fun.call(thisArg, arg1, arg2, ...)
        * thisArg: 当前调用函数this的指向对象

2. 利用父构造函数继承属性
3. 借用原型对象继承方法

```js
    function Father(uname, age) {
    // this 指向父构造函数的实例
    this.uname = uname;
    this.age = age;
}

Father.prototype.money = function () {
    console.log(100000);
}

function Son(uname, age, score) {
    // this 指向子构造函数的对象实例
    Father.call(this, uname, age);
    this.score = score;
}

// 子构造函数专门的方法
Son.prototype.exam = function () {
    console.log("孩子考试");
}
// Son.prototype = Father.prototype;  直接赋值会出现问题，如果修改了子原型对象，父原型对象也会跟着一起变化
Son.prototype = new Father();
// 如果利用对象的形式修改了原型对象，别忘了利用constructor 指回原来的原型对象
Son.prototype.constructor = Son;
var son = new Son('刘德华', 18, 100);
console.log(son);
console.log(Father.prototype);
console.log(Son.prototype.constructor);
```

## 5. 类的本质

1. class的本质还是function
2. ES6的类其实就是*语法糖*  ``语法糖：有两个方法可以实现同样的功能，但是一种写法更加清晰、方便，那么这个方法就是语法糖``

## 6. ES5新增方法

### 6.1 数组方法

#### 6.1.2 迭代(遍历)方法: forEach()、map()、filter()、some()、every()

1. forEach()  ``forEach()中return不会终止迭代``

> array.forEach(function(currentValue, index, arr))

* currentValue: 数组当前项的值
* index：数组当前项的索引
* arr: 数组对象本身

2. filter()  ``filter()中return不会终止迭代``

> array.filter(function(currentValue, index, arr))

* filter()方法创建一个新的数组，主要用于筛选数组
* 它直接返回一个新数组

3. some()

> array.some(function(currentValue, index, arr))

* some()方法用于查找数组中是否有满足条件的元素
* *返回值是布尔值*
* 如果找到第一个满足条件的元素，则终止循环，不再继续查找

### 6.2 字符串方法

1. trim()  从一个字符串的两端删除空白字符

> str.trim()

### 6.3 对象方法

1. Object.defineProperty() 定义对象中新属性或修改原有的属性

> Object.defineProperty(obj, prop, descriptor)

* obj: 必需，目标对象
* prop: 必需，需定义或修改的属性的名字
* descriptor: 必需，目标属性所拥有的特性 ``以对象形式{}书写``
    * value 设置属性的值
    * writable 值是否可以重写
    * enumerable 目标属性是否可以被枚举
    * configurable 目标属性是否可以被删除或是否可以再次修改特性

```js
    var obj = {
    id: 1,
    pname: '小米',
    price: 1999
};

Object.defineProperty(obj, 'num', {
    value: 1000
})

console.log(obj);
```

2. Object.keys()  获取对象自身所有的属性

> Object.keys(obj)

* 效果类似for..in
* 返回一个由属性名组成的数组

## 7. 函数进阶

### 7.1 函数的定义和调用

1. 函数的定义方式

* 命名函数function
* 匿名函数
* new Function()

2. 函数的调用方式

* 普通函数
    * fn.call()
* 对象的方法
    * 对象.方法()
* 构造函数
    * new Star()
* 绑定事件函数
    * btn.onclick = function(){} 点击了按钮就可以调用函数
* 定时器函数
    * setInterval(function(){}, 1000)  定时器自动1秒钟调用一次
* 立即执行函数
    * (function(){})()  自动调用

### 7.2 this

* 普通函数  ``this指向window``
* 对象的方法  ``this指向调用的对象``
* 构造函数  ``this指向new出来的实例对象``
* 绑定事件函数  ``this指向函数的调用者``
* 定时器函数  ``this指向window``
* 立即执行函数  ``this指向window``

1. 改变函数内部this指向

* call()  ``经常用作继承``
    * fun.call(thisArg, arg1, arg2, ...)
* bind()  ``不会调用函数,返回的是原函数改变this之后产生的新函数``
    * fun.bind(thisArg, arg1, arg2, ...)
    * 应用：如果有的函数我们不需要立即执行调用，但是又想改变这个函数内部的this指向此时用bind

```js
    var btn = document.querySelector('button');
btn.onclick = function () {
    this.disabled = true;  // 这个this指向的是 btn 这个按钮
    setTimeout(function () {
        // this.disabled = false;  // 定时器函数里面的this指向的是window
        this.disabled = false;
    }.bind(this), 3000)  // 绑定了bind 
}
```

* apply() ``经常跟数组有关系``
    * fun.apply(thisArg, [argsArray])  参数必须是数组

### 7.3 严格模式

严格模式可以应用到整个脚本或个别函数中

1. 为脚本开启严格模式

> 在所有语句之前做一个特定语句"use strict";

2. 为函数开启严格模式

> 把"use strict";声明放在函数体所有语句之前

3. 严格模式下this指向问题

> 严格模式下全局作用域函数中的this是undefined
>
> 严格模式下，如果构造函数不加new调用，this会报错
>
> new实例化的构造函数指向创建的对象实例
>
> 定时器this还是指向window
>
> 事件、对象还是指向调用者

4. 严格模式下函数变化

> 函数不能有重名的参数
>
> 函数必须声明在顶层 新版本的Js会引入"块级作用域",为了与新版本接轨，不允许在非函数的代码块内声明函数

### 7.4 高阶函数

高阶函数是对其它函数进行操作的函数，它接收函数作为参数或将函数作为返回值输出，典型的就是回调函数

```js
  function fn(a, b, callback) {
    console.log(a + b);
    callback && callback();
}

fn(1, 2, function () {
    console.log('我是最后调用的');
});
```

### 7.5 闭包

闭包指有权访问另一个函数作用域中变量的函数  ``一个作用域可以访问另外一个函数内部的局部变量``

1. 闭包的作用  ``延伸了变量的作用范围``

### 7.6 递归

由于递归很容易发生"栈溢出"错误，所以*必须要加退出条件return*

1. 递归函数求阶乘

```js
  function fn(n) {
    if (n == 1) {
        return 1;
    }
    return n * fn(n - 1);
}
```

2. 递归函数求斐波那契序列

```js
  function fb(n) {
    if (n == 1 || n == 2) {
        return 1;
    }
    return fb(n - 1) + fb(n - 2);
}
```

### 7.7 浅拷贝

浅拷贝只是拷贝一层，更深层次对象级别的只拷贝引用(地址)
> ES6新增浅拷贝语法糖 Object.assign(target,...sources)

```js
  var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    }
};
var o = {};
for (var k in obj) {
    // k是属性名 obj[k]是属性值
    o[k] = obj[k];
}

console.log(o);
o.msg.age = 20;
console.log(obj);  // 这里的age会被修改为20
console.log('---------------');

Object.assign(o, obj);
console.log(o);
```

### 7.8 深拷贝

深拷贝拷贝多层，每一级别的数据都会拷贝
>

```js
  var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    },
    color: ['pink', 'red']
};
var o = {};

// 封装函数
function deepCopy(newobj, oldobj) {
    for (var k in oldobj) {
        // 判断我们的属性值属于哪种数据类型
        // 1. 获取属性值
        var item = oldobj[k];
        // 2. 判断这个值是否是数组
        if (item instanceof Array) {  // 数组也是个Object
            newobj[k] = [];
            deepCopy(newobj[k], item)
        } else if (item instanceof Object) {
            // 3. 判断这个值是否是对象
            newobj[k] = {};
            deepCopy(newobj[k], item)
        } else {
            // 4. 属于简单数据类型
            newobj[k] = item;
        }
    }
}

deepCopy(o, obj);
```

## 8. 正则表达式

### 8.1 概述

在JS中，正则表达式也是对象

* 验证表单
* 过滤敏感词
* 关键词提取

### 8.2 使用

1. 创建正则表达式

* 调用RegExp对象的构造函数创建
    * var regexp = new RegExp(/123/);
* 利用字面量创建
    * var regexp = /123/;

2. 测试正则表达式 test()

> regexObj.test(str);

### 8.3 特殊字符

1. 边界符

> ^ 以谁开始
>
> $ 以谁结束

2. 字符类

> []  有一系列字符可供选择，只要匹配其中一个就可以
>
> /^[abc]$/ 三选一，只能是a或b或c
>
> [-]  -表示范围
>
> [^....]  取反，不能包括括号里面的

3. 量词符

```
 * 重复零次或多次
 + 重复一次或多次
 ? 重复零次或一次
 {n} 重复n次
 {n,} 重复n次或者更多次
 {n,m} 重复n到m次  中间不要有空格
```

4. 预定义类

> \d  [0-9]
>
> \D  [^0-9]
>
> \w  [A-Za-z0-9]
>
> \W  [^A-Za-z0-9]
>
> \s  [\t\r\n\v\f]  匹配空格
>
> \S  [^\t\r\n\v\f]  匹配非空格的字符

### 8.4 替换

1. replace替换

> stringObject.replace(regexp/substr, replacement)

* 第一个参数：被替换的字符串或者正则表达式
* 第二个参数：替换为的字符串
* 返回值是一个替换完毕的新字符串

2. 正则表达式参数

> /表达式/[switch]

switch(也称为修饰符)按照什么样的模式来匹配:

* g: 全局匹配
* i: 忽略大小写
* gi: 全局匹配 + 忽略大小写

## 9. ES6新语法

### 9.1 let关键字

> let声明的变量只在所处于的块级有效 块级作用域 使用var声明的变量不具备块级作用域特性
>
> let防止循环变量变成全局变量
>
> 使用let不存在变量提升
>
> 使用let具有暂时性死区特性 不受外部影响

```js
    var arr = [];
for (var i = 0; i < 2; i++) {
    arr[i] = function () {
        console.log(i);
    }
}

arr[0]();
arr[1]();

// 使用var关键字输出结果为 2 2
// 将var换为let输出结果为  0 1
```

### 9.2 const关键字

> 声明常量，就是值(内存地址)不能变化的量
>
> 具有块级作用域
>
> 声明常量时必须赋值
>
> 常量赋值后，值不能修改 可以修改数组里面元素的值，但不能修改数组

### 9.3 数组解构赋值

ES6中允许从数组中提取值，按照对应位置，对变量赋值。对象也可以实现解构

如果解构不成功，变量的值为undefined

### 9.4 对象解构


```js
let person = {name: 'zhangsan', age: 20};
// 第一种
let {name, age} = person;  // 对象解构实际上就是属性匹配
console.log(name);
console.log(age);

// 第二种
let{name: myName, age: myAge} = person;  // myName myAge属于别名
console.log(myName);
console.log(myAge);
```

### 9.5 箭头函数

箭头函数用来简化函数定义语法
> () => {}
> 
> const fn = () => {}
> 
> 若函数体中只有一句代码，且代码的执行结果就是返回值，可以省略大括号
> 
> const sum = (num1, num2) => num1 + num2;
> 
> 如果形参只有一个，可以省略小括号
> 
> const fn = v => {alter(v);}

1. 箭头函数中的this关键字
> 箭头函数不绑定this关键字，箭头函数中的this，指向的是函数定义位置的上下文this
```js
    function fn() {
    console.log(this);
    return () => {
        console.log(this)
    }
    const obj = {name: 'zhangsan'};
    const resFn = fn.call(obj);   // fn.call(obj)使得fn的this指向obj  this-> obj
    resFn();  // this-> obj
}
```

### 9.6 剩余参数
剩余参数语法允许我们将一个不定数量的参数表示为一个*数组*
> function fn(...args){}
```js
    const sum = (...args) =>{
    let total = 0;
    args.forEach(item => total += item)
    return total;
};
console.log(sum(10,20));
console.log(sum(10,20,30));
```

### 9.7 扩展运算符

扩展运算符可以将数组或者对象转为用逗号分隔的参数序列
```js
    let arr = [1,2,3];
    console.log(...arr); // 1 2 3
    console.log(1,2,3)
```

### 9.8 Array
1. Array的扩展方法
> 扩展运算符可以应用于合并数组
> 
> arr1.push(...ary2)
> 
> 扩展运算符将类数组或可遍历对象转换为真正的数组
```js
    let oDivs = document.getElementsByTagName('div');
    oDivs = [...oDivs];
```

2. Array.from()方法  ``将类数组或可遍历对象转换为真正的数组``

3. find()方法  ``用于找出第一个符合条件的数组成员，如果没有找到返回undefined``

4. findIndex()方法  ``用于找出第一个符合条件的数组成员的位置，如果没有找到返回-1``

5. includes()方法  ``表示某个数组是否包含给定的值，返回布尔值``

### 9.9 模板字符串

ES6新增的创建字符串的方式，使用反引号定义
> let name = `zhangsan`;

1. 模板字符串中可以解析变量
```js
    let name = '张三';
    let sayHello = `hello, my name is ${name}`;
```
2. 模板字符串可以换行

3. 模板字符串中可以调用函数

### 9.10 String的扩展方法

1. startsWith()与endsWith()
> startsWith(): 表示参数字符串是否在原字符串的头部，返回布尔值
> 
> endsWith(): 表示参数字符串是否在原字符串的尾部，返回布尔值

2. repeat(n)  ``将原字符串重复n次，返回一个新字符串``

### 9.11 Set数据结构

ES6提供了新的数据结构Set。类似于数组，但是成员的值都是唯一的，没有重复的值

1. Set本身是一个构造函数，用来生成Set数据结构
> const s = new Set();
> 
> const set = new Set([1,2,3,4,4]);

2. 利用set数据结构做数组去重

3. set实例方法
* add(value)
* delete(value)
* has(value)  返回一个布尔值，表示该值是否为Set的成员
* clear()  清除所有成员，没有返回值

4. set遍历
> s.forEach(value => console.log(value));