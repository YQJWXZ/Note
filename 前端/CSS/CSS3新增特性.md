# CSS3新增特性

## 1. 新增选择器

**类选择器、属性选择器、伪类选择器。权重为10**

**伪元素选择器和标签选择器一样，权重为1**

1. 属性选择器

* E[att]  选择具有att属性的E元素
* E[att="val"]  选择具有att属性且**属性值等于val**的E元素
* E[att^="val"]  选择具有att属性且**值以val开头**的E元素
* E[att$="val"]  选择具有att属性且**值以val结尾**的E元素
* E[att*="val"]  选择具有att属性且**值中含有val**的E元素

```html
<style>
    input[type=text] {
        color: #00a4ff;
    }
</style>

<input type="text" name="" id="">
<input type="password" name="" id="">
```

2. 结构伪类选择器

* E:first-child  匹配父元素中的第一个子元素E
* E:last-child  匹配父元素中的最后一个E元素
* E:nth-child(n)  匹配父元素中的第n个子元素E  ``n可以是数字，关键字和公式; nth-child会把所有的盒子都排列序号``
* E:first-of-type  指定类型E的第一个
* E:last-of-type  指定类型E的最后一个
* E:nth-of-type(n)  指定类型E的第n个  ``nth-of-type 会把指定元素的盒子排列序号``

> n是数字，就是选择第n个子元素
```css
ul li:nth-child(2) {
    background-color: skyblue;
}
```

> n是关键字: **even 偶数, odd 奇数**
```css
ul li:nth-child(even) {
    background-color: #cccccc;
}
```

> n是公式
```css
/*nth-child(n) 从0开始 每次加1 往后面计算 这里面必须是n 不能是其他的字母*/
ul li:nth-child(n) {
    background-color: pink;
}

ol li:nth-child(2n+1) {
    background-color: #00a4ff;
}
```

nth-child与nth-of-type的区别：

* nth-child 对父元素里面所有孩子排序选择（序号是固定的）先找到第n个孩子，然后看看是否和E匹配
* nth-of-type 对父元素里面指定子元素进行排序选择，先去匹配E，然后再根据E找第n个孩子

3. 伪元素选择器

利用CSS创建新标签元素，而不需要html标签

* ::before  在元素内部的前面插入内容
* ::after  在元素内部的后面插入内容

> before和after必须有content属性

#### 伪元素字体图标

```css
div {
    position: relative;
    width: 200px;
    height: 35px;
    border: 1px solid red;
}

div::after {
    position: absolute;
    top: 10px;
    right: 10px;
    font-family: 'icomoon';
    content: '\e91e';
    color: red;
    font-size: 18px;
}
```

#### 伪元素清除浮动

## 2. CSS3盒子模型

1. box-sizing:content-box  盒子大小为width+padding+border (以前默认的)
2. box-sizing:border-box  盒子大小为width  (padding+bordedr就不会撑大盒子，前提padding+border不会超过width宽度)

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

## 3. CSS3过渡(transition)

> transition: 要过渡的属性 花费时间 运动曲线（默认是ease） 何时开始

谁做过渡给谁加

``transition: width .5s, height .5s;``

``transition: all .5s;``

## 4. CSS3新增其他特性

1. 图片边模糊（CSS3滤镜filter）

* filter: 函数();   ``filter:blur(5px);  blur模糊处理 数值越大越模糊``

2. 计算盒子宽度width:calc函数

    ``width:calc(100%-80px);``