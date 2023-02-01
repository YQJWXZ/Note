# CSS05

## 1. 精灵图(sprites)

有效地减少服务器接收和发送请求的次数，提高页面的加载速度

1. 精灵图主要针对于小的背景图片 使用
2. 主要借助于背景位置来实现---**background-position**

> background: url(./images/sprites.png) no-repeat -182px -108px;

> background: url(./images/sprites.png) no repeat;
> background-position: -182px -108px;

3. 一般情况下精灵图都是负值（网页中的坐标）

## 2. 字体图标(iconfont)

字体图标展示的是图标，**本质属于字体**

1. 字体图标的引入：

    1. 把下载包里面的fonts文件夹放入页面根目录下
    2. 在CSS样式中全局声明字体，字体文件路径问题
    3. 复制小方框到span中去，并指定字体

2. 字体图标的追加

## 3. CSS三角
```css
.div {
    width: 0;
    height: 0;
    line-height: 0;
    font-size: 0;
    border: 50px solid transparent;
    border-left-color: pink;
}
```

## 4. CSS用户界面样式

1. 更改用户的鼠标样式(cursor)
    * {cursor:default;}
    * {cursor:pointer;}  小手
    * {cursor:move;}  移动
    * {cursor:text;}  文本
    * {cursor:not-allowed;}  禁止

2. 表单轮廓(outline)
    * input{outline: 0;}  去掉默认的表单轮廓线
    * input{outline: none;}

3. 防止表单域拖拽(resize)
    * textarea{ resize:none;}

## 5. vertical-align属性应用

用于设置一个元素的**垂直对其方式**，但是它只针对行内元素或者行内块元素有效

1. vertical-align:
    * baseline 默认，元素放置在父元素的基线上
    * top  元素的顶端与行中最高元素的顶端对齐
    * middle  元素放置在父元素的中部
    * bottom  元素的顶端与行中最低的元素的顶端对齐

2. 图片底侧空白缝隙解决
    * 给图片添加vertical-align属性
    * 把图片转换为块级元素  ``dispaly:block;``

## 6. 溢出的文字省略号显示

1. 单行文本溢出显示省略号

```css
{
/*1.先强制一行内显示文本*/
    white-space: nowrap;
/*2. 超出的部分隐藏*/
    overflow: hidden;
/*3. 文字用省略号替代超出的部分*/
    text-overflow: ellipsis;
}
```

2. 多行文本溢出显示省略号

兼容性问题(适用于webkit内核)

```css
{
    overflow: hidden;
    text-overflow: ellipsis;
    /*弹性伸缩盒子模型显示*/
    display: -webkit-box;
    /*限制在一个块级元素显示的文本的行数*/
    -webkit-line-clamp: 2;
    /*设置或检索伸缩盒对象的子元素的排列方式*/
    -webkit-box-orient: vertical;
}
```

## 7. 常见布局技巧

1. margin负值的运用
    * 细线边框  ``margin-left: -1px;``
    * 细线边框鼠标经过变化
   ```css
      ul li:hover {
      /*如果盒子没有定位，则鼠标经过添加相对定位即可*/
        position:relative;
        border: 1px solid blue;  
   }
   
      ul li:hover {
        /*如果li都有定位，则利用z-index提高层级*/
        z-index: 1;
        border: 1px solid blue;
   }
   ```

2. 文字围绕浮动元素

3. 行内块的巧妙运用
    * 页码布局
4. css三角强化
    * 直角三角形
   ```css
   {
        position: absolute;
        right: 0;
        top:0;
        width:0;
        height:0;
        border-color: transparent red transparent transparent;
        border-style: solid;
        border-width: 22px 8px 0 0;
   }
   ```

## 8. CSS初始化

指重设浏览器的样式