# Web APIs

Web API 是浏览器提供的一种操作浏览器功能和页面元素的API(BOM和DOM)

## 1. DOM

处理可拓展标记语言(HTML或者XML)的标准编程接口

### 1.1 获取元素

1. 根据ID获取
* getElementById(id)  ``返回的是一个元素对象``

> console.dir(document.getElementById('time'));  打印我们返回的元素对象，更好的查看里面的属性和方法

2. 根据标签名获取
* getElementsByTagName('标签名')   返回带有指定标签的对象的集合，以伪数组的形式存储

3. HTML5新增的方法获取

* getElementsByClassName('类名')  根据类名返回元素对象集合
* querySelector('选择器')  根据指定选择器返回第一个元素对象  ``里面的选择器需要加符号 .box #nav``
* querySelectorAll('选择器')  根据指定选择器返回

4. 获取特殊元素

* 获取body元素  ``document.body``
* 获取html元素  ``document.documentElement``

### 1.2 事件

可以被Js侦测到的行为

触发 --- 响应机制

1. 事件三要素
* 事件源
* 事件类型
* 事件处理程序

2. 执行事件的步骤
* 获取事件源
* 注册事件(绑定事件)
* 添加事件处理程序(采取函数赋值形式)

3. 常见的鼠标事件
* onclick  鼠标点击左键触发
* onmouseover  鼠标经过触发
* onmouseout  鼠标离开触发
* onfocus  获得鼠标焦点触发
* onblur  失去鼠标焦点触发
* onmousemove  鼠标移动触发
* onmouseup  鼠标弹起触发
* onmousedown  鼠标按下触发

### 1.3 操作元素

1. 改变元素内容
* element.innerText  从起始位置到终止位置的全部内容，**但它去除html标签，同时空格和换行也会去掉**
* element.innerHTML  从起始位置到终止位置的全部内容，**包括html标签，同时保留空格和换行**
```js
    // 1. 获取元素
    var btn = document.querySelector('button');
    var div = document.querySelector('div');
    
    // 2. 注册事件
    btn.onclick = function () {
        div.innerText = '2022-12-10';
    }
    
    // 元素可以不用添加事件
    var p = document.querySelector('p');
    p.innerText = getDate();
```

2. 修改元素属性
* innerText、innerHTML改变元素内容
* src、href
* id、alt、title
```js
    // 1.获取元素
    var ldh = document.getElementById('ldh');
    var zxy = document.getElementById('zxy');
    var ing = document.querySelector('img');
    
    // 2. 注册事件 处理程序
    zxy.onclick  = function() {
        img.src = 'images/zxy.jpg';
        img.title = '张学友';
    }
    
    ldh.onclick = function () {
        img.src = 'images/ldh.jpg';
        img.title = '刘德华';
    }
```

3. 修改表单属性

> type、value、checked、selected、disabled

``btn.disabled = true; 表单被禁用,btn也可换成this,this指向的是事件函数的调用者``

4. 修改样式属性

* element.style  行内样式操作
> JS里面的样式采用驼峰命名法, fontSize、backgroundColor
> 
> JS修改style样式操作，产生的是行内样式，css权重比较高

```js
    var div = document.querySelector('div');
    
    div.onclick = function () {
        this.style.backgroundColor = 'purple';
        this.style.width = '250px';
    }
```

* element.className  类名样式操作
> className会直接更改元素的类名，会覆盖原先的类名
```css
    .first {
    background-color: pink;
}
    .change {
    background-color: #00a4ff;
    font-size: 25px;
    margin-top: 100px;
}
```
```js
    var div = document.querySelector('div');

    div.onclick = function () {
        // 可以修改元素的className更改元素的样式  适合于样式较多或者功能复杂的情况
        this.className = 'change';
        
        // 如果想要保留原先的类名，可以这么做
        this.className = 'first change';
    }
```
5. 获取自定义属性

H5自定义属性规范：``H5规定自定义属性data-开头作为属性名并且赋值,比如<div data-index = "1"></div> 或者使用JS设置setAttribute``

* 获取属性值
    * element.属性  获取属性值  ``console.log(div.id)``
    * element.getAttribute('属性');  ``console.log(div.getAttribute('id'));``
    * H5新增的获取自定义属性的方法：``console.log(div.dataset.index)或者console.log(div.dataset['index'])``

区别：
> element.属性 获取内置属性值（元素本身自带的属性）
> 
> element.getAttribute('属性');  主要获取自定义的属性

* 设置属性值
    * element.属性 = '值'
    * element.setAttribute('属性','值');

* 移除属性
    * element.removeAttribute('属性');

6. 排他思想(算法)

首先排除其他人，然后才设置自己的样式

```js
    // 1.获取所有按钮元素
    var btns = document.getElementsByTagName('button');

    // btns得到的是伪数组 里面的每一个元素 btns[i]
    for (var i = 0; i < btn.length; i++) {
        btns[i].onclick = function () {
            // 首先需要把所有的按钮背景元素去掉
            for (var j = 0; j < btn.length; j++) {
                btns[j].style.backgroundColor = '';
            }
            
            // 其次设置当前元素的背景颜色
            this.style.backgroundColor = 'pink';
        }
}
```

### 1.4 节点操作

元素节点 nodeType：1
属性节点 nodeType: 2
文本节点 nodeType: 3(文本节点包括文字、空格、换行等)

#### 1.4.1 父节点

> node.parentNode(得到的是离元素最近的父节点，找不到返回null)

#### 1.4.2 子节点

> parentNode.childNode

`` 返回值里面包含了所有的子节点，包括元素节点、文本节点``

如果想要获得里面的元素节点，则需要专门处理，一般不提倡使用


> parentNode.children是一个只读属性，返回所有的子元素节点。它只返回子元素节点，其余节点不返回

1. 第一个子节点

> parentNode.firstChild
> 
> parentNode.firstElementChild  返回第一个元素节点(兼容性问题)
> 
> parentNode.children[0]

2. 最后一个子节点

> parentNode.lastChild
> 
> parentNode.lastElementChild  返回最后一个元素节点(兼容性问题)
> 
> parent.children[parent.children.length-1]

#### 1.4.3 兄弟节点

> node.nextSibling 下一个兄弟节点 包含元素节点或者文本节点
> node.nextElementSibling  下一个兄弟元素节点
> 
> 
> node.previousSibling  上一个兄弟节点 包含元素节点或者文本节点
> node.previousElementSibling  上一个兄弟元素节点

#### 1.4.4 创建和添加节点

> document.createElement('tagName')  创建节点
> 
> node.appendChild(child)  添加节点
> 
> node.insertBefore(child,指定元素)

#### 1.4.5 删除节点

> node.removeChild(child)

**阻止链接跳转需要添加javascript:void(0);或者javascript:;**

#### 1.4.6 复制节点

> node.cloneNode()

**如果括号参数为空或者为false，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点**

**如果括号参数为true，则是深拷贝，会复制接地那本身以及里面所有的子节点**

* 三种动态创建元素
  * document.write()  是直接将内容写入页面的内容流，**但是文档执行完毕，它会导致页面全部重绘**
  * element.innerHTML  是将内容写入某个DOM节点，不会导致页面全部重绘，创建多个元素效率更高（不要拼接字符串，采取数组形式拼接）
  * document.createElement()  创建多个元素效率会低一点，但是结构更清晰 


### 1.5 事件高级

1. 注册事件

* 传统注册   ``注册事件的唯一性``
* 事件监听注册  ``同一个元素同一个事件可以注册多个监听器``
  * eventTarget.addEventListener(type, listener[, useCapture])
    * type: 事件类型字符串，click、mouseover
    * listener: 事件处理函数，事件发生时，会调用该监听函数
    * useCapture: 可选参数，是一个布尔值，默认是false（表示在事件冒泡阶段调用事件处理程序；如果是true，表示在事件捕获阶段调用事件处理程序）

2. 删除事件

* 传统方式
* 方法监听方式
  * eventTarget.removeEventListener(type, listener[, useCapture]);

```js
var btns=document.querySelectorAll('button');

btns[1].addEventListener('click', fn);  // 注册事件

function fn() {
  alter('事件监听');
  btns[1].removeEventListener('click',fn);  // 删除事件
}
```
3. DOM事件流

DOM事件流分为3个阶段：
* 捕获阶段  ``Document -> html -> body -> father -> son``
* 当前目标阶段
* 冒泡阶段  ``son -> father -> body -> html -> Document``

4. 事件对象

```js
var  div=document.querySelector('div');
div.onclick=function (event) {
  console.log(event);
  
  // 1. event就是一个事件对象 写到侦听函数里面 当形参来看
  // 2. 事件对象只有有了事件才会存在，是系统自动创建的 不需要传递参数
  // 3. 事件对象是我们事件的一系列相关数据的集合 跟事件相关的 有很多属性和方法
  // 4. 这个事件对象可以自己命名
}

div.onclick=function (e) {
  console.log(e);
}
```

* 事件对象的常见属性和方法：
  1. e.target  返回**触发**事件的对象
  2. e.type 返回事件的类型 click、mouseover
  3. e.preventDefault() 阻止默认行为 也可以直接return false(return后面的代码就不执行了)
  4. e.stopPropagation()  阻止冒泡 也可以用 e.cancelBubble = true

* e.target 与 this 的区别：

> e.target 点击了哪个元素就返回那个元素
> 
> this 哪个元素绑定了这个点击事件，那么就返回谁（返回绑定事件的对象）

5. 事件委托（代理、委派）

原理：不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每个子节点上

6. 常用的鼠标事件(MouseEvent)
* e.clientX  返回鼠标相对于**浏览器窗口可视区**的X坐标
* e.clientY  返回鼠标相对于**浏览器窗口可视区**的Y坐标
* e.pageX  返回鼠标相对于**文档页面**的X坐标
* e.pageY  返回鼠标相对于**文档页面**的Y坐标
* e.screenX  返回鼠标相对于**电脑屏幕**的X坐标
* e.screenY  返回鼠标相对于**电脑屏幕**的Y坐标

7. 常见的键盘事件(keyEvent)
* onkeyup 某个键盘按键被松开时触发
* onkeydown 某个键盘按键被按下时触发
* onkeypress 某个键盘按键被按下时触发  **但是它不识别功能键 比如 ctrl、shift**

> 如果使用addEventListener不需要加on
> 
> 三个事件的执行顺序： keydown -> keypress -> keyup

键盘事件对象：``e.keyCode``
> keyup 和 keydown 事件不区分字母大小写  a和A得到都是65
> 
> keypress事件区分字母大小写 


## 2. BOM

浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是window

### 2.1 BOM的构成 

windows对象是浏览器的顶级对象，具有双重角色
1. 它是js访问浏览器窗口的一个接口
2. 它是一个全局对象，定义在全局作用域中的变量、函数都会变成windows对象的属性和方法

### 2.2 windows对象的常见事件

1. 窗口加载事件

> windows.onload = function(){}  当文档内容完全加载完成后会触发该事件（包括图像、脚本文件、CSS文件等）
> 
> windows.addEventListener('load',  function(){});

``windows.onload传统注册事件方式智能写一次 如果是多个，会议最后一个windows.onload为准``

``如果使用addEventLisstener则没有限制``

> windows.addEventListener('DOMContentLoaded', function(){})  仅当DOM加载完成，不包括样式表，图片，flash等等

2. 调整窗口大小事件

> windows.onresize = function(){} 
> 
> windows.addEventListener('resize', function(){});

### 2.3 定时器

1. window.setTimeout(调用函数, [延迟的毫秒数]);  多个定时器时加标识符
2. window.setInterval(回调函数, [间隔的毫秒数]);  重复调用一个函数，每隔这个时间，就去调用一次回调函数 
3. window.clearInterval(intervalID);  取消/停止计时器，里面的参数就是定时器的标识符（**标识符声明为全局变量**）

### 2.4 js执行机制

1. 同步任务
同步任务都在主线程上执行，形成一个**执行栈**

2. 异步任务
js的异步是通过回调函数实现的

一般而言，异步任务有以下三种类型：
* 普通事件 click、resize等
* 资源加载 load、error等
* 定时器 setInterval、setTimeout等

> 异步任务相关**回调函数**添加到**任务队列**中(也叫消息队列) 

3. js执行机制
* 先执行**执行栈中的同步任务**
* 异步任务放到任务队列中(异步进程)
* 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取**任务队列**中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

> 由于主线程不断地重复获得任务、执行任务、再获取任务、再执行，这种机制被称为**事件循环**

```js
        console.log(1);
        document.onclick = function() {
            console.log('click');
        }
        console.log(2);
        setTimeout(function() {
            console.log(3)
        }, 3000)

// 执行结果： 1,2,3,(只有点击了就会显示click)
```

### 2.5 location对象

1. location属性用于**获取或设置窗体的URL**，并且可以用于**解析URL**

> URL一般格式：protocol://host[:port]/path/[?query]#fragment

2. location对象的属性
* location.href  获取或者设置整个URL
* location.host
* location.port
* location.pathname
* location.search  返回参数
* location.hash  返回片段

3. location对象的属性
* location.assign()  跟href一样，可以跳转页面（重定向页面），记录历史，可以后退
* location.replace()  替换当前页面 不记录历史 不能后退
* location.reload()  重新加载页面 参数为true则强制刷新

### 2.6 navigator对象

### 2.7 history对象

history对象方法：
* back() 后退
* forward() 前进
* go(参数) 前进后退 参数是1 前进一个页面；参数是-1 后退一个页面


## 3. PC端网页特效 

### 3.1 元素偏移量offset系列

1. offset系列常用属性
* element.offsetParent 返回**作为该元素带有定位的父级元素**，如果父级都没有定位则返回body
* element.offsetTop  返回**元素相对带有定位父元素**上方的偏移
* element.offsetLeft 返回**元素相对带有定位父元素**左边框的偏移
* element.offsetWidth 返回自身**包括padding、边框、内容区的宽度**，返回数值不带单位
* element.offsetHeight  返回自身**包括padding、边框、内容区的高度**，返回数值不带单位

2. offset 与 style 的区别

```
offset:
1. offset可以得到任意样式表中的样式值
2. offset系列获得的数值是没有单位的
3. offsetWidth包含padding+border+width
4. 只读属性

获取元素大小位置 --> offset
```
```
style:
1. style只能得到行内样式表中的样式值
2. style.width获得的是带有单位的字符串
3. style.width获得不包含padding+border的值
4. 可读写属性

给元素更改值  --> style
```
### 3.2 元素可视区client系列

1. client系列常用属性
* element.clientTop  返回元素上边框的大小
* element.clientLeft 返回元素左边框的大小
* element.clientWidth 返回自身**包括padding、内容区的宽度，不含边框**，返回数值不带单位
* element.offsetHeight  返回自身**包括padding、内容区的高度，不含边框**，返回数值不带单位

2. 立即执行函数
主要作用：创建一个独立的作用域，避免了命名冲突
> (function(){})()  // 最后一个小括号可以看做是调用函数, 可以传参
>
> (function(){}());

淘宝flexible.js
```js
(function flexible(window, document) {
    // 获取的html 的根元素
    var docEl = document.documentElement
        // dpr 物理像素比
    var dpr = window.devicePixelRatio || 1

    // adjust body font size  设置我们body 的字体大小
    function setBodyFontSize() {
        // 如果页面中有body 这个元素 就设置body的字体大小
        if (document.body) {
            document.body.style.fontSize = (12 * dpr) + 'px'
        } else {
            // 如果页面中没有body 这个元素，则等着 我们页面主要的DOM元素加载完毕再去设置body的字体大小
            document.addEventListener('DOMContentLoaded', setBodyFontSize)
        }
    }
    setBodyFontSize();

    // set 1rem = viewWidth / 10    设置我们html 元素的文字大小
    function setRemUnit() {
        var rem = docEl.clientWidth / 10
        docEl.style.fontSize = rem + 'px'
    }

    setRemUnit()

    // reset rem unit on page resize  当我们页面尺寸大小发生变化的时候，要重新设置下rem 的大小
    window.addEventListener('resize', setRemUnit)
        // pageshow 是我们重新加载页面触发的事件
    window.addEventListener('pageshow', function(e) {
        // e.persisted 返回的是true 就是说如果这个页面是从缓存取过来的页面，也需要从新计算一下rem 的大小
        if (e.persisted) {
            setRemUnit()
        }
    })

    // detect 0.5px supports  有些移动端的浏览器不支持0.5像素的写法
    if (dpr >= 2) {
        var fakeBody = document.createElement('body')
        var testElement = document.createElement('div')
        testElement.style.border = '.5px solid transparent'
        fakeBody.appendChild(testElement)
        docEl.appendChild(fakeBody)
        if (testElement.offsetHeight === 1) {
            docEl.classList.add('hairlines')
        }
        docEl.removeChild(fakeBody)
    }
}(window, document))
```
### 3.3 元素滚动scroll系列

1. scroll系列常用属性
* element.scrollTop  返回被卷去的上侧距离，返回数值不带单位
* element.scrollLeft 返回被卷去的左侧距离，返回数值不带单位
* element.scrollWidth 返回自身实际的宽度，不含边框，返回数值不带单位
* element.scrollHeight  返回自身实际的高度，不含边框，返回数值不带单位

> 元素被卷去的头部是element.scrollTop, 如果是页面被卷去的头部则是window.pageYOffset

```
他们主要用法：
1. offset系列经常用于获得元素位置 offsetLeft offsetTop
2. client经常用于获取元素大小 clientWidth clientHeight
3. scroll经常用于获取滚动距离 scrollTop scrollLeft
4. 注意页面滚动的距离通过window.pageXOffset获得
```

### 3.4 mouseover 和 mouseenter 鼠标事件

> mouseover鼠标经过自身盒子会触发，经过子盒子还会触发
> 
> mouseenter鼠标 智慧经过自身盒子触发，因为mouseenter不会冒泡，鼠标离开mouseleave同样不会冒泡

## 4. 动画效果

动画实现原理： 通过定时器setInterval()不断移动盒子位置
1. 获得盒子当前位置
2. 让盒子在当前位置加上1个移动距离
3. 利用定时器不断重复这个操作
4. 加一个结束定时器的条件
5. 注意此元素需要添加定位，才能使用element.style.left

```html
<style>
  position: absolute;
  left: 0;
  width: 100px;
  height: 100px;
  background-color: pink;
</style>

<script>
  var div = document.querySelector('div');
  var timer = setInterval(function () {
    if(div.offsetLeft >= 400) {
        // 停止动画 本质就是停止计时器
      clearInterval(timer);
    }
    div.style.left = div.offsetLeft + 1 + 'px';
  }, 30)
</script>
```
### 4.1 动画函数封装 

1. 简单动画函数封装obj目标对象 target目标位置
2. 给不同的对象添加不同的定时器
```html
<script>
  function animate(obj, target) {
      clearInterval(obj.timer);
    /*var timer*/ obj.timer = setInterval(function () {  // var每次调用占太大内存空间，换成obj.timer，只需在函数外部给obj.name赋值
      if(obj.offsetLeft >= target) {
        // 停止动画 本质就是停止计时器
        clearInterval(obj.timer);
      }
      obj.style.left = obj.offsetLeft + 1 + 'px';
    }, 30)
  }

  var div = document.querySelector('div');
  // 调用函数
  animate(div,400);
</script>
```

### 4.2 缓动动画

1. 效果原理

> 核心算法：（目标值 - 现在的位置）/ 10 作为每次移动的距离  ``步长``

```html
<script>
  function animate(obj, target,callback) {  // callback 回调函数
      clearInterval(obj.timer);
      obj.timer = setInterval(function () {
          var step = (target - obj.offsetLeft) / 10;
          step = step > 0 ? Math.ceil(step) : Math.floor(step);
          if(obj.offsetLeft >= target) {
              // 停止动画 本质就是停止计时器
              clearInterval(obj.timer);
              // 回调函数写到定时器结束里面
             if(callback){
                 callback();
             }
      }
      obj.style.left = obj.offsetLeft + step + 'px'; 
    }, 30)
  }
  
  var span = document.querySelector('.span');
  var btn500 =  document.querySelector('.btn500');
  var btn800 =  document.querySelector('.btn800');
  
  btn500.addEventListener('click', function () {
      animate(span,500);
  })
  
  btn800.addEventListener('click', function () {
    // 调用函数
    animate(span, 800, function () {
      span.style.backgroundColor = 'red';
    })
  })
</script>
```

### 4.3 缓动动画添加回调函数

回调函数：函数可以作为一个参数。将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，再执行传进去的这个函数，这个过程就叫回调

### 4.4 动画函数封装

以后经常使用这个动画函数，可以单独封装到一个js文件里面，使用的时候引用这个js文件即可

### 4.5 节流阀

1. 防止轮播图按钮连续点击造成播放过快

2. 目的：当上一个函数动画内容执行完毕，再去执行下一个函数动画，让事件无法连续触发

3. 核心实现思路：利用回调函数，添加一个变量来控制，锁住函数和解锁函数
``
1. 开始设置一个变量 var flag = true;
2. if(flag){flag = false; do something}  关闭水龙头
3. 利用回调函数 动画执行完毕,flag = true; 打开水龙头
``