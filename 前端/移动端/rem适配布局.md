# rem布局

## 1. rem

em是相对于**父元素字体大小**
rem的基准是相对于**html元素的字体大小**

## 2. 媒体查询

1. 使用@media查询，可以针对不同的媒体类型定义不同的样式
2. **@media可以针对不同的 屏幕尺寸设置不同的样式

```css
@media mediatype and|not|only (media feature) {
    CSS-Code;
}
```
eg:
```css
/*超过800px后背景颜色失效*/
@media screen and (max-width: 800px) {
  body {
    background-color: #00a4ff;
  }
}
```

* mediatype 媒体类型
    * all 用于所有设备
    * print 用于打印机和打印预览
    * **screen  用于电脑屏幕，平板，智能手机等**
* 关键字 and|not|only
    * and 可以将多个多媒体特性连接到一起
    * not 排除某个媒体类型
    * only 指定某个媒体类型
* 媒体特性 (media feature)  ``必须有小括号包含``
    * width 定义输出设备中页面可见区域的宽度
    * min-width 定义输出设备中页面最小可见区域宽度
    * max-width 定义输出设备中页面最大可见区域宽度


3. 媒体查询引入资源：

样式比较繁多时，可以针对不同的媒体使用不同的stylesheets  ``直接在link中判断设备的尺寸，然后引用不同的css文件``

媒体查询最好的方法是从小到大
```html
<link rel="stylesheet" href="mystylesheet.css" media="mediatype and|not|only (media feature)">
```

## 3. Less
一门CSS预处理语言，拓展了CSS的动态属性（.less）

1. less变量

> @变量名：值;
命名规范：
* @为前缀
* 不能包含特殊字符
* 不能以数字开头
* 大小写敏感

2. less编译(Easy Less)

3. less嵌套

> 子元素的单样式直接写到父元素里面
```less
header {
  .logo {
    height: 200px;
  }
}
```

如果遇见（交集|伪类|伪元素选择器）
* 内层选择器的前面没有&符号，则它被解析为父选择器的后代
* **如果有&符号，它就被解析为父元素自身或父元素的伪类**
```less
a {
  color: red;
  &:hover {
    color: blue;
  }
}
```

4. less运算

* 两个数参与运算，如果只有一个数有单位，则最后的结果就以这个单位为准
* 两个数参与运算，如果两个数都有单位，而且是不一样的单位，最后的结果以第一个为准

> 运算符中间左右有个空格隔开  ``1px + 5``

## 4. rem适配方案

1. 技术方案1
  * less
  * 媒体查询
  * rem

> 元素大小取值方法： 页面元素的rem值 = 页面元素值（px）/ html font-size字体大小  ``屏幕宽度/划分的份数就是html font-size的大小``

2. 技术方案2
  * flexible.js
  * rem