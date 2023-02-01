# Html5新增特性

## 1. 新增语义化标签

1. 头部标签  ``<header>``
2. 导航标签  ``<nav>``
3. 内容标签  ``<nav>``
4. 定义文档某个区域  ``<section>``
5. 侧边栏标签  ``<aside>``
6. 尾部标签  ``<footer>``

## 2. 新增多媒体标签

1. 音频  ``<audio>``
   * autoplay="autoplay"  自动播放(谷歌浏览器需要js来解决自动播放问题)
   * controls="controls"  向用户显示播放控件
   * loop="loop"  循环播放

``<audio src="文件地址" controls="controls"></audio>``

2. 视频  ``<video>``
    * autoplay="autoplay"  自动播放(谷歌浏览器需要添加muted来解决自动播放问题)
    * controls="controls"  向用户显示播放控件
    * width、height
    * loop="loop"  循环播放
    * preload="auto/none"  规定是否预加载视频
    * src="url"
    * poster="lmgurl"  加载等待的画面图片
    * muted="muted"  静音播放

``<video src="文件地址" controls="controls"></video>``

## 3. 新增的input标签

* type="email"
* type="url"
* type="date"
* type="time"
* type="number"
* type="tel"
* type="search"  搜索框
* type="color"  生成一个颜色选择表单

## 4. 新增表单属性

* required="required"  表示内容不能为空
* placeholder="提示文本"  表单的提示信息
* autofocus="autofocus"  自动聚焦属性
* autocomplete="on/off"  默认显示表单之前提交过的文字
* multiple="multiple"  可以多选文件提交