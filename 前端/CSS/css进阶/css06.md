# CSS06（CSS3新增特性）

##  1. CSS3 2D转换

1. 移动translate

语法：
```css
transform: translate(x,y);

transform: translateX(n);
transform: translateY(n);
```
* translate最大的优点：**不会影响到其他元素的位置**
* translate中的百分比单位是相对于**自身元素**的  ``translate:(50%,50%);``
* 对行内标签没有效果

2. 旋转rotate

语法：
```css
transform: rotate(度数)

eg:
transform: rotate(45deg)
```

#### 转换中心点transform-origin

语法：
```css
transform-origin: x y;
```

3. 缩放scale

语法：
```css
transform: scale(x,y);
```

* x,y不跟单位，就是倍数的关系
* scale缩放最大的优势：可以设置转换中心点缩放，默认以中心点缩放的，而且不影响其他盒子

4. 2D转换综合写法

```css
transform: translate() rorate() scale();
```
* 其顺序会影响转换的效果
* **同时有位移和其他属性的时候，要讲位移放在最前**

5. 一个盒子水平垂直居中

```css
.city div[class^="pluse"] {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

## 2. CSS3 动画(animation)

1. 用keyframes定义动画（类似定义类选择器）
```css
@keyframes 动画名称 {
    0%{
        width: 100px;
    }
    
    100%{
        width: 200px;
    }
}
```

1. 元素使用动画
```css
div {
    ·····
    /*调用动画*/
    animation-name: 动画名称;
    /*持续时间*/
    animation-duration: 持续时间;
}
```

> from 和 to 等价于 0% 和 100%

CSS动画常用属性：
* @keyframes  规定动画
* **animation-name  动画名称**
* **animation-duration  动画一个周期所需时间**
* animation-timing-function  规定动画的速度曲线，默认"ease"
* animation-delay  规定动画何时开始，默认是0
* animation-iteration-count  规定动画被播放的次数，次数是1，还有infinite
* animation-direction  规定动画是否在下一周期逆向播放，默认是"normal"，alternate逆播放
* animation-play-state  规定动画是否正在运行或暂停。默认是"running"，还有"paused"
* animation-fill-mode  规定动画结束后状态，保持forwards，回到起始backwards

> animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束的状态

速度曲线之steps步长：
animation-timing-function
* linear  匀速
* ease  低速->加速->低速
* ease-in  低速开始
* ease-out  低速结束
* ease-in-out  低速开始，低速结束
* steps()  指定了时间函数中的间隔数量（步长）  ``进度条、打字机效果``

## 3. CSS3 3D转换

1. 3D位移  ``translate3d(x,y,z)``

```css
transform: translate3d(x,y,z);

transform: translateX(100px);
transform: translateY(100px);
transform: transformZ(100px);

transform: translateX(100px) translateY(100px) transformZ(100px);
```

2. 透视  ``perspective``

**透视写在被观察元素的父盒子上面**

3. 3D旋转  ``rotate3d(x,y,z)``

```css
transform: rotate3d(x,y,z,deg);     /* 矢量 */

transform: rotateX(45deg);
transform: rotateY(45deg);
transform: rotateZ(45deg);
```

4. 3D呈现  ``transform-style``

控制子元素是否开启三维立体环境
* transform-style:flat;  默认，子元素不开启3d立体空间
* **transform-style:preserve-3d;**  子元素开启立体空间

> **代码写给父级，但是影响的是子盒子**

## 4. 浏览器私有前缀

1. 私有前缀
* -moz-  firefox浏览器私有属性
* -ms-   IE浏览器
* -webkit- safari、chrome浏览器
* -o-  Opera