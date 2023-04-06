# Ajax

## 1. 开端

### 1.1 客户端与服务器

### 1.2 URL地址

1. URL: 统一资源定位符
2. URL地址的组成部分：
* 客户端与服务器之间的通信协议
* 存有该资源的服务器名称
* 资源在服务器上的具体存放位置

### 1.3 分析网页的打开过程

### 1.4 服务器对外提供了哪些资源

* HTML 骨架
* CSS 颜值
* JavaScript 行为
* 数据 灵魂  ``XMLHttpRequest``

> var xhrObj = new XMLHttpRequest();

## 2. Ajax

异步JavaScript和XML: 在网页中利用XMLHttpRequest对象和服务器进行数据交互的方式

典型应用场景：
* 用户名检测
* 搜索提示
* 数据分页显示
* 数据的增删改查
* ...

### 2.1 JQuery中的Ajax

1. 发起Ajax请求
* $.get() 发起get请求
> $.get(url, [data], [callback]);  data可以是 {id: 1}

* $.post()
> $.post(url, [data], [callback]);

* $.ajax()
```js
    $.ajax({
        type: '',
        url: '',
        data: {},
        success: function(res) {}
})
```

2. 接口

使用Ajax请求数据时，*被请求的URL地址*，就叫做数据接口。同时，每个接口必须有请求方式

接口文档：
* 接口名称
* 接口URL
* 调用方式
* 参数格式
* 响应格式
* 返回示例(可选)

## 3. form表单与模板引擎

### 3.1 form表单

1. 组成部分
* 表单标签
* 表单域 ``文本框、密码框、隐藏域、多行文本框、复选框、单选框、下拉选择框、文件上传框``
* 表单按钮

2. 属性
* action  向何处发送表单数据(URL地址)
* method  
* target  在何处打开action URL 
    * _blank  在新窗口中打开
    * _self  默认，在相同的框架中打开
* enctype  发送表单数据之前如何对数据进行编码
    * application/x-www-form-urlencoded 默认，在发送前编码所有字符
    * multipart/form-data 不对字符编码(*在使用包含文件上传控件的表单时，必须使用该值*)

3. 表单同步提交

缺点：
* 跳转到action URL所指向的地址，用户体验差
* 页面之前的状态和数据会丢失

解决：
* 表单只负责采集数据，Ajax负责将数据提交到服务器

4. 通过Ajax提交表单数据

* 监听表单提交事件：
```js
    $('#form1').submit(function (e) {
        ...;
})

    $('#form1').on('submit',function (e) {
        ...;
    })
```

* 阻止表单默认提交行为
```js
   $('#form1').submit(function (e) {
        // 阻止表单的提交和页面的跳转
        e.preventDefault();
}) 

    $('#form1').on('submit',function (e) {
        e.preventDefault();
    })
```

* 快速获取表单中的数据
> $(selector).serialize();  必须为每个表单元素添加name属性
```js
    $('form1').serialize();

    // 调用结果
    // username=用户的值&password=密码的值
```

### 3.2 art-template模板引擎

1. 标准语法
> {{}}  在{{}}内可以进行变量输出、循环数组等操作

2. 原文输出
> {{@ value}}

3. 条件输出
> {{if value}} 按需输出的内容 {{/if}}
> 
> {{if value}}  按需输出的内容 {{else if v2}}  按需输出的内容 {{/if}}

4. 循环输出
```
  {{each arr}}
      {{$index}} {{$value}}
  {{/each}}
```

5. 过滤器
过滤器的本质，就是一个function处理函数
> {{value | filterName}}

```
定义过滤器的基本语法:
  template.defaults.imports.filterName = function(value) {/*return处理的结果*/}
```

### 3.3 模板引擎的实现原理

1. 正则与字符串操作
基本语法：
  * exec()函数用于检索字符串中的正则表达式的匹配
  * 如果字符串中有匹配的值，则返回该匹配值，否则返回null

分组：
  * 正则表达式中()包起来的内容表示一个分组，可以通过分组来提取自己想要的内容

字符串中的replace函数：
  * replace()函数用于在字符串中用一些字符替换另一些字符
  * var result = '123456'.replace('123', 'abc')   **abc456**