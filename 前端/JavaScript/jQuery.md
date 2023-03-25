# jQuery

jQuery是一个快速、简洁的JavaScript库

优点：
1. 轻量级
2. 跨浏览器兼容
3. 链式编程、隐式迭代
4. 对事件、样式、动画支持，大大简化了DOM操作
5. 支持插件拓展开发

## 1. jQuery的入口函数

等着DOM结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完毕
```js
    // $: jQuery的顶级对象
    $(function() {
    ... // 此处是页面DOM加载完成的入口
});
```
```js
    $(document).ready(function(){
    ... // 此处是页面DOM加载完成的入口
})
```

> 相当于原生js中的DOMContentLoaded

## 2. jQuery对象和 DOM对象

1. 用原生JS获取来的对象就是DOM对象
2. jQuery方法获取的单对象就是jQuery对象
3. jQuery对象本质是：利用 $ 对DOM对象包装后产生的对象(伪数组形式存储)

```
    DOM对象与jQuery对象之间是可以相互转换的
    
    1. DOM对象转换为jQuery对象： $(DOM对象)
    
    2. jQuery对象转换为DOM对象
       * $('div')[index]  index是索引号
       * $('div').get[index]  index是索引号
```

## 3. jQuery常用API

### 3.1 jQuery选择器

> $("选择器")  里面选择器直接写CSS选择器即可，但是要加引号

1. 层级选择器
* 子代选择器 ``$("ul>li")`` 使用 > 号，获取亲儿子层级的元素
* 后代选择器 ``$("ul li")`` 使用空格，代表后代选择器

2. 隐式迭代
遍历内部DOM元素(伪数组形式存储)的过程就叫做隐式迭代

3. 筛选选择器
* $('li:first')
* $('li:last')
* $("li:eq(2)")  获取到的li元素中，选择索引号为2的元素
* $("li:odd")  获取到的li元素中，选择索引号为奇数的元素
* $("li:even")  获取到的li元素中，选择索引号为偶数的元素

4. jQuery筛选方法
* $("li").parent(); 
* $("ul").children("li");  相当于``$("ul>li")``
* $("ul").find("li");  相当于``$("ul li")``
* $(".first").siblings("li");  查找兄弟节点，不包括自己
* $(".first").nextAll()  查找当前元素之后所有的同辈元素
* $(".last").prevAll()  查找当前元素之前所有的同辈元素
* $("div").hasClass("protected")  检查当前的元素是否含有某个特定的类，如果有，返回true
* $("li").eq(2);  相当于``$("li:eq(2)")``,index从0开始

5. jQuery排他思想
```js
    $(function () {
    // 1. 隐式迭代 给所有的按钮都绑定了点击事件
    $("button").click(function () {
        // 2. 当前的元素变化背景颜色
        $(this).css("background","pink");
        // 3. 其余的兄弟去掉背景颜色 隐式迭代
        $(this).siblings("button").css("background","");
    });
})
```
6. jQuery链式编程

> $(this).css('color','red').siblings().css('color','');

### 3.2 jQuery样式操作

> $(this).css('属性','值')
> 
> $(this).css({"color":"white", "font-size":"20px"})  复合属性必须采用驼峰命名

1. 修改样式操作类(提前在css中定义这个类)
* 添加类  ``$("div").addClass("current");``
* 删除类  ``$("div").removeClass("current")``
* 切换类  ``$("div").toggleClass("current")``

2. 类操作与className区别
* 原生JS中className会覆盖元素原先里面的类名
* jQuery里面类操作只是对指定类进行操作，不影响原先的类名

### 3.3 jQuery效果
 
1. 显示隐藏
* show([speed,[easing],[fn]]) 
* hide([speed,[easing],[fn]])

2. 切换
* toggle([speed.[easing],[fn]])

3. 自定义动画
* animate(params,[speed],[easing],[fn])  ``params: 想要更改的样式属性``

### 3.4 jQuery属性操作
1. 获取属性值
> prop("属性")

2. 设置属性
> prop("属性","属性值")

3. 设置或获取自定义属性
> attr("属性")
> 
> attr("属性","属性值")

4. 数据缓存
> data()  方法可以在指定的元素上存取数据，并不会修改DOM元素结构。一旦页面刷新，之前存放的数据都将被移除

### 3.5 jQuery文本属性值
1. 普通元素内容html()(相当于原生innerHTML)
> html()  // 获取元素的内容
> 
> html("内容")  // 设置元素的内容

2. 普通元素文本内容text()(相当于原生innerText)
> text()  // 获取元素的文本内容
> 
> text("文本内容")  // 设置元素的文本内容

3. 表单的值val()(相当于原生value)
> value("表单值")

### 3.6 jQuery元素操作
1. 遍历元素(each方法)
> $("div").each(function(index, domEle){   })
* each方法遍历匹配的每一个元素，主要用DOM处理
* 里面的回调函数有两个参数：index是每个元素的索引号；domEle是每个*DOM元素对象，不是jquery对象*
* *所以要想使用jquery方法，需要给这个dom元素转换为jquery对象$(domEle)*

> $.each(object, function(index, element){  })
* $.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象
* 里面的函数有2个参数：index是美格尔元素的索引号；element遍历内容

2. 创建元素

``$("<li></li>");  动态创建一个li`` 

3. 添加元素
* 内部添加
    * 放到内容的最后面 ``$("ul").append(li);``
    * 放到内容的最前面 ``$("ul").prepend(li);``
* 外部添加
    * 把内容放到目标元素后面 ``element.after("内容");``
    * 把内容放到目标元素前面 ``element.before("内容");``

4. 删除元素
> element.remove()  删除匹配的元素(本身)
>
> element.empty()  删除匹配的单元素集合中所有的子节点
> 
> element.html("")  可以删除匹配的元素里面的子节点
### 3.7 jQuery尺寸、位置操作
1. 尺寸
* width()/height()  只算width/height
* innerWidth()/innerHeight()  包含padding
* outerWidth()/outerHeight()  包含padding、border
* outerWidth(true)/outerHeight(true)  包含padding、border、margin

2. 位置
* offset()设置或获取元素偏移  ``相对于*文档*的偏移坐标，跟父级没有关系``
* position()获取元素偏移  ``相对于*带有定位的父级*偏移坐标``
* scrollTop()/scrollLeft()设置或获取元素被卷去的头部和左侧

## 4. jQuery事件

### 4.1 jQuery事件注册
1. 单个事件注册
> element.事件(function(){ })
>
> $("div").click(function(){事件处理程序 })


### 4.2 jQuery事件处理
1. 事件处理on()绑定一个或多个事件
> element.on(events,[selector],fn)
* events: 一个或多个用空格分隔的事件类型
* selector: 元素的子元素选择器
* fn: 回调函数 即绑定在元素身上的侦听函数
```js
    $("div").on({
        mouseenter: function() {
            $(this).css("background","skyblue");
        },
        click: function(){
            $(this).css("background","purple");
        },
        mouseover: function(){
            $(this).css("background","blue");
        }
    })

    $("div").on("mouseenter mouseleave", function () {
        $(this).toggleClass("current");
    })

    // on()方法优势2：可以事件委派操作
    // 事件委派：把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素

    // click是绑定在ul身上的，但是触发的对象是ul里面的小li
    $("ul").on("click","li",function () {
      alert("hello world!");
    })

    // on()方法优势3： 可以给未来动态生成的元素绑定事件
    $("ul").on("click","li",function () {
    alert(11);
    })
    var li=$("<li>后来创建的</li>");
    $("ol").append(li);
```

2. 事件处理off()解绑事件
> $("p").off()  解绑p元素所有事件处理程序
>
> $("p").off("click")  解绑p元素上面的点击事件 后面的foo是侦听函数名
> 
> $("p").off("click","li")  解绑事件委托

3. 自动触发事件trigger()
* element.click()
* element.trigger("type")
* $("元素").triggerHandler("事件")  就是不会触发元素的默认行为

### 4.3 jQuery事件对象

事件被触发，就会有事件对象的产生
> element.on(events,[selector],function(event) { })

* 阻止默认行为：event.preventDefault() 或者 return false
* 阻止冒泡： event.stopPropagation()

## 5. jQuery其他方法

### 5.1 jQuery拷贝对象

> $.extend([deep], target, object1, [objectN])  会覆盖target原有的数据
* deep: 如果设为true为深拷贝，默认为false浅拷贝
* target: 要拷贝的目标对象
* object1: 待拷贝到第一个对象的对象

> 浅拷贝是把被拷贝的对象*复杂数据类型中的地址*拷贝给目标对象，修改目标对象会影响被拷贝对象
> 
> 深拷贝，完全克隆（拷贝的是对象，而不是地址），修改目标对象不会影响被拷贝对象

### 5.2 多库共存
需要一个解决方案，让jQuery和其他的js库不存在冲突，可以同时存在，这就叫做多库共存

jQuery解决方案：
1. 把里面的$符号统一改为jQuery ``jQuery("div")``
2. jQuery变量规定新的名称：$.noConflict() ``var xx=$.noConflict()``

### 5.3 jQuery插件
1. jQuery插件库
2. jQuery之家
3. 图片懒加载(图片使用延迟加载在可提高网页下载速度。它也能帮助减轻服务器负载)
4. 全屏滚动
5. bootstrap JS插件