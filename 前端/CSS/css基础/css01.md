# CSS01

## 1. CSS基础选择器
1. 标签选择器（html标签作为选择器）
* 基础选择器
* 复合选择器
 
2. 类选择器
* 多类名（如：姓名）
* class属性
```
.类名 {
     ···
}
```

3. id选择器
```
#id名 {
     ···
}
```
> 样式#定义，结构id调用，只能调用一次（如：身份证号）

4. 通配符选择器
```
 * {
     ···
}
```

##  2. CSS字体属性
1. 字体
* font-family 定义文本的字体系列
* font-size
* font-weight 定义字体粗细
* font-style 文字的风格
    * normal 默认
    * italic 斜体

2. font复合属性

```
font: font-style font-weight font-size/line-height font-family   （不能更换顺序）

font: italic 700 16px 'Microsoft yahei'

font: 16px 'Microsoft yahei'  (font-size、font-famil不可缺少)
```

## 3. CSS文本属性
* color
* text-align: 设置元素内文本内容的**水平对齐方式**
* text-decoration: 文本修饰  `` none underline overline(上划线) line-through(删除线) ``
* text-indent：文本首行缩进  `` text-indent：2em ``
* line-height: 设置行间距

## 4. CSS的引入方式
1. 行内样式表（行内式）
2. 内部样式表（嵌入式）
3. 外部样式表（链接式）

