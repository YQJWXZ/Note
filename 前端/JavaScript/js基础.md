# js基础

## 1. 变量

### 1.1 声明变量

> var age;

同时声明多个变量：

同时声明多个变量时，只需要写一个var，多个变量名之间使用英文逗号隔开

```js
var age=10,
    name='zs',
    sex=2;
```

### 1.2 变量的命名规范

## 2. 数据类型

### 2.1 简单数据类型

* Number
    * 程序里面数字前面加0，表示八进制  ``var num1 = 010;``
    * 程序里面数字前面加0x，表示十六进制  ``var num2 = 0xa;``
    * 数值的最大值  ``Number.MAX_VALUE``
    * 数值的最小值  ``Number.MIN_VALUE``
* String
    * 单引号
    * 单引号嵌套双引号  双引号嵌套单引号
    * 字符串转义符
    * 字符串长度(length属性)以及拼接  ``只要字符串和其他类型相拼接，最终的结果是字符串类型``
* Boolean
    * true 参与加法运算当1看 ``console.log(true+1)``
    * false 参与加法运算当0看 ``console.log(flase+1)``
* Undefined
* Null
    * 一个声明变量给null值，里面存的值为空

1. 数字型三个特殊值
    * Infinity  无穷大  ``alter(Infinity)``
    * -Infinity  无穷小  ``alter(-Infinity)``
    * NaN  Not a number,代表一个非数值  ``alter(NaN)``
        * isNaN()  方法用来判断非数字，返回一个布尔值，是数字返回false，不是数字返回true

2. 获取检测变量的数据类型
    * typeof  ``console.log(typeof num);``
    * prompt取过来的值是字符型的，注意运算
    * 字面量 ``数字字面量、字符串字面量、布尔字面量``
3. 数据类型转换
    * 转换为字符串  ``toString()、String()强制转换``
    * 转换为数字类型
        * parseInt(string)  ``console.log(parseInt('120px'));  会去掉px这个单位，结果为120``
        * parseFloat(string)
        * Number()  强制转换函数
        * 算术运算 隐式转换  ``console.log('12'-0);``
    * 转换为布尔型
        * 代表空、否定的值会被转换为false  ``console.log(Boolean(''));  //false``
```js
    var num1=prompt('输入第一个值：');
    var num2=prompt('输入第二个值：');
    var result=parseFloat(num1)+parseFloat(num2);
    alter('结果是：'+ result);
```
### 2.2 复杂数据类型

* Object

## 3. 运算符(operator)

### 3.1 算数运算符

### 3.2 递增递减运算符

### 3.3 比较运算符

1. 等于(==)
   默认转换数据类型  会把字符串型的数据转换为数字型 只要求值相等就可以  ``console.log(18=='18');  // true``
2. 全等于(===)
   一模一样 要求两侧的值 还有数据类型完全一致才可以  ``console.log(18==='18'); // false``

### 3.4 逻辑运算符

##### 短路运算(逻辑中断)
短路运算的原理：当有多个表达式时，左边的表达式值可以确定结果时，姐不再继续运算右边的表达式的值

1. 逻辑中断逻辑与

> 表达式1 && 表达式2
* **如果第一个表达式的值为真，则返回表达式2**
* **如果第一个表达式的值为假，则返回表达式1**
```js
    console.log(123 && 456);  // 456
    console.log(0 && 456);  // 0
    console.log('' & 1 + 2 && 456 * 56789);  // 0
```

2. 逻辑中断逻辑或

> 表达式1 || 表达式2
* **如果第一个表达式的值为真，则返回表达式1**
* **如果第一个表达式的值为假，则返回表达式2**
```js
    console.log(123 || 456);  // 123
    console.log(123 || 456 || 456 + 123);  // 123
    console.log(0 || 456 || 456 + 123);  // 456

    var num = 0;
    console.log(123 || num++);  // 123
    console.log(num);   // 0
```

### 3.5 赋值运算符
1. =
2. +=、-=
3. *=、/=、%=

### 3.6 运算符优先级

## 4. 流程控制分支结构

1. 顺序结构

2. 分支结构
* if
* if-else
* if-else-if
* 三元表达式
* switch

3. 循环结构
* for
* 双重for
* while
* do while
* break与continue

## 5. 数组

### 5.1 数组

数组中可以存放**任意类型**的数据

> var arrStus = ['小白',12,true,28.9];

### 5.2 获取数组元素

### 5.3 遍历数组

### 5.4 数组长度

> "数组名".length

### 5.5 数组新增元素

1. 通过修改length长度新增数组元素
```js
    var arr = ['red', 'green', 'blue', 'pink'];
    arr.length = 7;
    console.log(arr);   // ['red', 'green', 'blue', 'pink',empty x 3]
    console.log(arr[4]);  // undefined
    console.log(arr[5]);  // undefined
    console.log(arr[6]);  // undefined
```
2. 通过修改数组索引新增数组元素
```js
    var arr = ['red', 'green', 'blue'];
    arr[3] = 'pink';
    console.log(arr);  // ['red', 'green', 'blue', 'pink']

    arr[0] = 'yellow';  // 这里会替换原来的元素
    console.log(arr);  // ['yellow', 'green', 'blue', 'pink']
```
> 不要直接给数组名赋值，否则会覆盖掉以前的数据

## 6. 函数

### 6.1 函数

1. 声明函数

* 利用函数关键字自定义函数
```js
    function sayhi() {   // 函数是做某件事情，函数名一般是动词
    console.log('hi~~');
}
```
* 函数表达式声明函数
```js
    var 变量名 = function (){};
```
> return终止函数并且只能返回一个值
> 
> 函数没有return返回undefined
2. 调用函数

3. 函数封装


### 6.2 形参和实参的传递过程

### 6.3 arguments获取函数的参数

只有函数才有arguments对象，arguments实际上是当前函数的一个**内置对象**

arguments展示形式是一个伪数组，因此可以遍历，具有以下特点：
* 具有length属性
* 按索引方式储存数据
* 不具有数组的push,pop等方法
```js
    function fn() {
    console.log(arguments);
    console.log(arguments.length);
    console.log(arguments[2]);
}

    fn(1,2,3);
```
> 函数可以调用另外一个函数

## 7. 作用域

### 7.1 作用域

1. 全局作用域  整个script标签或者是一个单独的js文件

2. 局部作用域  函数内部

3. 块级作用域(es6新增)

### 7.2 变量的作用域

1. 全局变量
2. 局部变量

### 7.3 作用域链

内部函数访问外部函数的变量，采取的是链式查找的方式来决定取哪个值，这种结构称之为作用域链

## 8. 预解析

js引擎运行js分两步： 1.预解析  2. 代码执行

1. 预解析

js引擎会把js里面所有的 war 还有 function 提升到当前作用域的最前面

2. 变量预解析（变量提升）

就是把所有的变量声明提升到当前的作用域最前面  **不提升赋值操作**

3. 函数预解析（函数提升）

就是把所有的函数声明提升到当前作用域的最前面  **不调用函数**

## 9. 对象

### 9.1 对象

对象是由属性和方法组成的
* 属性：事物的特征，在对象中用属性来表示（常用名词）
* 方法：事物的行为，在对象中用方法表示（常用动词）

### 9.2 创建对象

1. 利用字面量创建对象

使用对象：
* 对象名.属性名
* 对象名['属性名']
```js
    var obj = {
        username:"zhangsan",
        age:18,
        sex:'男',
        sayHi: function () {
            console.log('hi~~');
        }
}

    console.log(obj.username);
    console.log(obj['age']);
```

> 变量、属性、函数、方法

2. 利用new Object创建对象
```js
    var obj = new Object();
    obj.username = 'zhangsan';
    obj.age = 18;
    obj.sex = '男';
    obj.sayHi = function () {
        console.log('hi~');
    }
```
3. 利用构造函数创建对象
```js
// 构造函数首字母大写
    function Star(username,age,sex) {  // 构造函数不需要return就可以返回结果
        this.username = username;
        this.age = age;
        this.sex = sex;
        this.sing = function (sang) {
            console.log(sang);
        }
}

    var ldh = new Star('刘德华',18,'男');  // 调用函数返回的是一个对象
    
    ldh.sing('冰雨');
```
### 9.3 new关键字

### 9.4 遍历对象属性

> for...in语句

```js
    var obj = {
        username:"zhangsan",
        age:18,
        sex:'男'
}

    for(var k in obj) {
        console.log(k);  // k 变量 输出得到的是属性名
        console.log(obj[k]);  // obj[k] 得到的是属性值
    }
```

## 10. js内置对象
### 10.1 内置对象

1. js对象
    * 自定义对象
    * 内置对象
    * 浏览器对象
### 10.2 Math对象

1. Math最大值
> 如果没有参数，则结果为 -Infinity
>
> 如果有任一参数不能被转换为数值，则结果为NaN

2. Math绝对值

> Math.abs();

3. Math取整

> Math.floor()  向下取整
> 
> Math.ceil()  向上取整
> 
> Math.round()  四舍五入

4. 随机数方法random()

Math.random() 函数返回一个浮点，伪随机数在范围[0,1)

```js
    // 得到两个数之间的随机整数
    function getRandom(min,max) {
        return Math.floor(Math.random()*(max-min+1))+min;
    }
    
    console.log(getRandom(1,10));

    // 随机点名
    var arr = ['张三', '张三丰', '李四','王五','赵六'];
    console.log(arr[getRandom(0,arr.length-1)]);
```
### 10.3 Date对象

Date() 日期对象 是一个构造函数，必须使用new来调用创建日期对象

1. 格式化日期

> console.log(date.getMonth()+1);  //返回的月份小一个月  记得加1

2. Date总的毫秒数(时间戳)
```js
    // 方法1
    console.log(date.valueOf());
    // 方法2
    console.log(date.getTime());
    // 方法3
    var date1 = +new Date();
    
    // H5新增的 获得总的毫秒数
    console.log(Date.now());
    
```
### 10.4 Array对象

1. 检测是否为数组
* instanceof运算符  ``console.log(arr instanceof Array)``
* Array.isArray(参数)  ``console.log(Array.isArray(arr))``

2. 添加删除数组元素
* push(参数1...)  末尾添加一个或多个元素
* pop()  删除数组最后一个元素，数组长度减1
* unshift(参数1...)  数组开头添加一个或多个元素
* shift()  输出数组第一个元素，数组长度减1

3. 数组排序
* reverse()  翻转数组
* sort()   数组排序

```js
    // 数组排序(冒泡排序)
    var arr1 = [13, 4, 77, 1, 7];
    arr1.sort(function (a, b) { 
        return a-b;  // 升序的顺序排列
        // return b-a;  // 降序的顺序排列
    });
    console.log(arr1);
```

4. 获取数组元素索引
* indexOf(数组元素)  数组中查找给定元素的第一个索引
* lastIndexOf()  在数组中的最后一个索引

> 索引不存在返回-1

5. 数组转换为字符串
* toString()
* join('分隔符')  把数组中的所有元素转换为一个字符串  ``console.log(arr.join('&'))``

### 10.5 String对象

1. 基本包装类型

就是把简单数据类型包装成复杂数据类型，使基本数据类型有了属性和方法

* String
* Number
* Boolean

2. 根据字符返回位置
* indexOf('要查找的字符',开始的位置)
* lastIndexOf()  从后往前找，只找第一个匹配的

```js
    // 求某个字符出现的位置及次数
    var str = "abcoefoxyozzopp";
    var index = str.indexOf('o');
    var count = 0;
    while (index != -1){
        console.log(index);
        count++;
        index = str.indexOf('o',index + 1);
    }
```

3. 根据位置返回字符
* chatAt(index)  返回指定位置的字符
* charCodeAt(index)  返回指定位置处字符的ASCII码
* str[index]  获取指定位置处字符

4. 拼接以及截取字符串
* concat(str1,str2,str3...)  等效于+
* substr(start,length)  从start位置开始，length取的个数
* slice(start,end)  从start位置开始，截取到end位置 ，end取不到
* substring(start,end)  从start位置开始，截取到end位置 ，end取不到，不接受负值

5. 替换字符串以及转换为数组
* replace('被替换的字符', '替换为的字符')  ``只会替换第一个字符``
* split('分隔符')

## 11. 简单数据类型与复杂数据类型

### 11.1 简单类型(值类型)与复杂类型(引用类型)

### 11.2 堆和栈

### 11.3 数据类型分配

### 11.4 数据类型传参