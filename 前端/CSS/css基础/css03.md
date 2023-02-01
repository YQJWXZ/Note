# CSS03

## 1. 盒子模型

CSS盒子模型本质上就是一个盒子，封装周围的HTML元素，它包括：边框(border)、外边距(margin)、内边距(padding)和实际内容(content)

1. 边框(border)

* border-width 定义边框粗细
* border-style 边框的样式
    * none
    * hidden
    * solid 实线边框
    * dashed 虚线边框
    * dotted 点线边框
* border-color 边框颜色
  
> border: 1px solid red;  没有顺序
> 
> border-top: 1px solid red;  只设定上边框，其余同理

* border-collapse 控制浏览器绘制表格边框的方式，控制相邻单元格的边框
  
> border-collapse: collapse;   相邻的边框合并

#### 边框会额外增加盒子的实际大小，因此有两种方案解决：

1. 测量盒子大小的时候，不量边框
2. 测量的时候包含了边框，则需要width/height减去边框宽度


2. 内边距(padding)
   
* padding-left 左内边距
* padding-right 右内边距
* padding-top 上内边距
* padding-bottom 下内边距

**顺时针**  先从上开始，值一个一个，不够循环

```
padding: 5px   上下左右都有5像素内边距
padding: 5px 10px  上下内边距是5像素，左右内边距是10像素  
padding: 5px 10px 20px  上内边距是5像素  左右内边距是10像素  下内边距是20像素
padding: 5px 10px 20px 30px 上是5像素 右是10像素 下是20像素 左是30像素  
```

> padding: 0 28px; 盒子指定高度，这样就不会撑坏盒子（没有宽度属性）

1. 外边距（margin）

* margin-left 左外边距
* margin-right 右外边距
* margin-top 上外边距
* margin-bottom 下外边距

> margin: 0 auto;  块级盒子水平居中对齐，盒子必须指定宽度，这样就不会撑坏盒子（没有高度）
> 
> 行内元素或者行内块元素水平居中需给其父元素添加 text-align: center 即可

#### 嵌套块元素垂直外边距的塌陷问题：

解决方案：

1. 可以为父元素定义上边框
2. 可以为父元素定义上内边距
3. 可以为父元素添加overflow:hidden 

#### 清除网页元素的内外边距

```css
* {
  padding: 0;
  margin: 0;
}
```

> 行内元素尽量只设置左右 内外边框，不设置上下内外边框，但转换为块级和行内块元素就可以

## 2. 圆角边框

* border-radius:length;  用于设置元素的外边框圆角
    * border-radius: 10px, 20px, 30px, 40px;
    * border-top-left-radius
    * border-top-right-radius
    * border-bottom-right-radius
    * border-bottom-left-radius
> border-radius:100px;  圆角矩形设置为高度一半
> 
> border-radius:50%;

## 3. 盒子阴影

* border-shadow: h-shadow v-shadow blur spread color inset;
    * h-shadow 必需，水平阴影的位置，允许负值
    * v-shadow 必需，垂直阴影的位置，允许负值
    * blur 模糊距离
    * spread 阴影的尺寸
    * color 阴影的颜色
    * inset 将外部阴影改为内部阴影

> box-shadow: 10px 10px 10px -4px rgba(0,0,0,0.3);
> 
> 盒子阴影不占用空间，不会影响其他盒子排列

原先盒子没有影子，当我们鼠标经过盒子就添加阴影效果
```css
div {
  box-shadow: 10px 10px 10px -4px rgba(0,0,0,0.3);
}
```

## 4. 文字阴影

* text-shadow: h-shadow v-shadow blur color;