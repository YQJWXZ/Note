# flex布局(弹性布局)

## 1. flex布局原理

通过给父盒子添加flex属性，来控制子盒子的位置和排列方式

## 2. flex布局父项常见属性
首先给父级添加flex属性
> display: flex;

1. flex-direction: 设置主轴的方向
   * row  默认值，从左到右
   * row-reverse 从右到左
   * column 从上到下
   * colum-reverse 从下到上

2. justify-content: 设置**主轴**上的子元素排列方式  ``首先必须指定主轴方向``
   * flex-start  默认，从头部开始，若主轴是x轴，则从左到右
   * flex-end 从尾部开始排列
   * **center 在主轴居中对齐**
   * space-around 平分剩余空间
   * **space-between 先两边贴合 再平分剩余空间**

3. flex-wrap: 设置子元素是否换行
   * nowrap  默认不换行
   * wrap  换行

4. align-items: 设置侧轴上的子元素排列方式（单行）
   * flex-start 从上到下
   * flex-end 从下到上
   * **center 挤在一起居中（垂直居中）**
   * stretch 拉伸（默认值）

5. align-content: 设置侧轴上的子元素 的排列方式（多行）  ``只能用于子项出现换行的情况``
   * flex-start
   * flex-end
   * center 在侧轴中间显示
   * space-around 子项在侧轴平分剩余空间
   * space-between 子项在侧轴先分布在两头，再平分剩余空间
   * stretch 设置子项元素高度平分父元素高度

6. flex-flow: 复合属性，相当于同时设置了flex-direction和flax-wrap


## 3. flex布局子项常见属性

1. flex子项占的份数

> flex: number  默认是0, 可以是数字，也可以是百分比（百分比是相随与父级来说的）

2. align-self: 控制子项自己在侧轴上的排列方式

3. order属性定义项目的排列顺序

> 数值越小越靠前