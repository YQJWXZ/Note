# CSS02

## 1. Emmet语法
快速生成html语法：
* **标签名按Tab** 生成双标签  ``div+Tab``
* **标签名*9**  生成9个标签  ``div*3`` 
* **>**  生成父子级关系的标签  ``ul>li``
* **+**  生成兄弟级的标签   ``div+p``
* **.demo**  生成带有类名  ``div.nav``
* **#demo+Tab** 生成带有id的  ``div#nav+Tab``
* **$**  生成有顺序的div类名  ``.demo$*5``

快速生成CSS语法：
* **w200+Tab** => ``width: 200px``
* **lh26+Tab** => ``line-height: 26px``

## 2. CSS的复合选择器
1. 后代选择器（儿子孙子都选出来）
```
元素1 元素2 {
    样式声明
}
```

2. 子选择器(只选亲儿子)
```
元素1>元素2 {
    样式声明
}
```

3. 并集选择器
```
元素1,元素2 {
    样式声明
}
```

4. 伪类选择器
   1. 链接伪类选择器
        * a:link  选择所有未被访问的链接
        * a:visited  选择所有已被访问的链接
        * a:hover  选择鼠标指针位于其上的链接
        * a:active  选择活动链接（鼠标按下未弹起的链接）
   2. :focus伪类选择器（用于选取获得焦点的表单元素）
   ```css
        input:focus {
            background-color: aqua;
   }
   ```
> 按照LVHA的循顺序声明 :link -> :visited -> :hover -> :active

## 3. CSS的元素显示模式
1. 元素显示模式

* 块元素

块级元素的特点：
* 自己独占一行
* 高度、宽度、外边距以及内边距都可以控制
* 宽度默认是容器的100%
* 是一个容器及盒子，里面可以放行内或者块级元素

> 文字类的元素内不能使用块级元素

* 行内元素

行内元素的特点：
* 相邻行内元素在一行上，一行可以显示多个
* 高、宽直接设置是无效的
* 默认宽度就是它本身内容的宽度
* 行内元素只能容纳文本或其他行内元素

* 行内块元素

行内块元素的特点：
* 和相邻行内元素在一行上，但是他们之间会有空白缝隙。一行可以显示多个
* 默认宽度就是它本身内容的宽度
* 高度，行高，外边距以及内边距都可以控制

2. 元素显示模式转换
* `` display:block; ``  转换为块元素
```css
   a {
   width: 150px;
   height: 50px;
   background-color: pink;
   display: block;
}
```

* `` diplay:inline; `` 转换为行内元素
```css
div {
   width: 300px;
   height: 100px;
   background-color: purple;
   display: inline;
}
```
* `` display:inline-block; ``  转换为行内块
```css
span {
   width: 300px;
   height: 30px;
   background-color: skyblue;
   display: inline-block;
}
```

## 4. CSS的背景
1. background-color
   * transparent 背景色透明

> background: rgba(0,0,0,0.3);

2. background-image: url()
3. background-repeat 背景平铺
   * repeat
   * no-repeat
   * repeat-x 横向平铺
   * repeat-y 纵向平铺
4. background-position: x y
   * top
   * center
   * bottom
   * left
   * right
5. background-attachment 背景附着(背景图像固定或者滚动)
   * fixed 背景图像固定
   * scroll 背景图像是随对象内容滚动

> background: 背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置;
> 
> background: transparent url(image.jpg) repeat-y fixed top;

## 5，CSS三大特性
1. 层叠性（解决样式冲突）
* 样式冲突，**就近原则**，哪个样式 离结构近，就执行哪个样式
* 样式不冲突，不会层叠

2. 继承性
字标签会继承父标签的某些样式，如文本颜色和字号
```css
body {
   color: pink;
   font: 12px/1.5 'Microsoft YaHei UI';
}

div {
   /*子元素继承了父元素body的行高 1.5*/
   /*这个1.5就是当前元素文字大小 font-size 的1.5倍  16*1.5=21*/
    font-size: 16px;
}

p {
   /*1.5*16=24*/
    font-size: 16px;
}
```

3. 优先级
同一个元素指定多个选择器，就会有优先级的产生
* 选择器相同，则执行层叠性
* 选择器不同，则根据**选择器权重**执行

> 通配符和继承权重为0，标签选择器为1，类选择器为10，id选择器为100，行内样式表为1000，!important无穷大 
> 
> **继承的权重为0**
> 
> **复合选择器的权重叠加叠加问题**
