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

> $.get(url, [data], [callback]); data可以是 {id: 1}

* $.post()

> $.post(url, [data], [callback]);

* $.ajax()

```js
    $.ajax({
    type: '',
    url: '',
    data: {},
    success: function (res) {
    }
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

* action 向何处发送表单数据(URL地址)
* method
* target 在何处打开action URL
    * _blank 在新窗口中打开
    * _self 默认，在相同的框架中打开
* enctype 发送表单数据之前如何对数据进行编码
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
...
    ;
})

$('#form1').on('submit', function (e) {
...
    ;
})
```

* 阻止表单默认提交行为

```js
   $('#form1').submit(function (e) {
    // 阻止表单的提交和页面的跳转
    e.preventDefault();
})

$('#form1').on('submit', function (e) {
    e.preventDefault();
})
```

* 快速获取表单中的数据

> $(selector).serialize(); 必须为每个表单元素添加name属性

```js
    $('form1').serialize();

// 调用结果
// username=用户的值&password=密码的值
```

### 3.2 art-template模板引擎

1. 标准语法

> {{}} 在{{}}内可以进行变量输出、循环数组等操作

2. 原文输出

> {{@ value}}

3. 条件输出

> {{if value}} 按需输出的内容 {{/if}}
>
> {{if value}} 按需输出的内容 {{else if v2}} 按需输出的内容 {{/if}}

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

```js
var str = '<div>我是{{name}}</div>';
var pattern = /{{([a-zA-Z]+)}}/;

var patternResult = pattern.exec(str);
str = str.replace(patternResult[0], patternResult[1]);
console.log(str);
```

字符串中的多次replace操作(使用while循环)

```js
var str = '<div>{{name}}今年{{age}}岁了</div>';
var pattern = /{{\s*([a-zA-Z]+)\s*}}/;

var patternResult = null;
while ((patternResult = pattern.exec(str))) {
    str = str.replace(patternResult[0], patternResult[1]);
}

console.log(str); 
```

replace替换为真值

```js
var data = {name: '张三', age: 20};

var str = '<div>{{name}}今年{{age}}岁了</div>';
var pattern = /{{\s*([a-zA-Z]+)\s*}}/;

var patternResult = null;
while ((patternResult = pattern.exec(str))) {
    str = str.replace(patternResult[0], data[patternResult[1]]);
}

console.log(str); 
```

### 3.4 实现简单的模板引擎

1. 实现步骤

* 定义模板结构

```html

<script type="text/html" id="tp1-user">
    <div>姓名: {{name}}</div>
    <div>年龄: {{age}}</div>
    <div>性别: {{gender}}</div>
    <div>住址: {{address}}</div>
</script>
```

* 预调用模板引擎

```html

<script>
    // 定义数据
    var data = {name: 'zs', age: 28, gender: '男', address: '北京顺义'};
    // 调用模板函数
    var htmlStr = template('tpl-user', data);
    // 渲染html结构
    document.getElementById('user-box').innerHTML = htmlStr;
</script>
```

* 封装template函数

```js
function template(id, data) {
    var str = document.getElementById(id).innerHTML;
    var pattern = /{{\s*([a-zA-Z]+)\s*}}/;

    var patternResult = null;
    while ((patternResult = pattern.exec(str))) {
        str = str.replace(patternResult[0], data[patternResult[1]]);
    }

    console.log(str);
}
```

* 导入并使用自定义的模板引擎

``<script src="./js/template.js"></script>``

## 4. Ajax加强

### 4.1 XMLHttpRequest的基本使用

xhr(XMLHttpRequest)是浏览器提供的Javascript对象，通过它，可以*请求服务器上的数据资源*

1. 使用xhr发起GET请求

* 创建xhr对象
* 调用xhr.open()函数
* 调用xhr.send()函数
* 监听xhr.onreadystatechange()事件

2. readyState属性

* 0 UNSENT
* 1 OPENED
* 2 HEADERS_RECEIVED
* 3 LOADING
* 4 DONE Ajax请求完成，这意味着数据传输已经彻底完成或失败

```js
 // 1. 创建xhr对象
var xhr = new XMLHttpRequest();

// 2. 调用open函数，指定请求方式与URL地址
xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks');

// 3. 调用send函数，发起Ajax请求
xhr.send();

// 4. 监听onreadystatechange事件
xhr.onreadystatechange = function () {
    // 4.1 监听xhr对象的请求状态readyState; 与服务器响应的状态status
    if (xhr.readyState === 4 && xhr.status === 200) {  // xhr.status并不是响应的数据部分，而是http请求状态的一部分
        // 4.2 打印服务器响应回来的数据
        console.log(xhr.responseText);
    }
}
```

3. 使用xhr发起带参数的GET请求

使用xhr发起带参数的GET请求时，只需要在调用xhr.open期间，为URL地址指定参数即可：
> xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1&booknname=西游记')  这种在URL地址后面拼接的参数，叫做查询字符串

4. 查询字符串
   在URL的末尾加上用于向服务器发送信息的字符串(变量)

```js
$.ajax({
    method: 'GET',
    url: 'http://www.liulongbin.top:3006/api/getbooks',
    data: {
        id: 1,
        bookname: '西游记'
    },
    success: function (res) {
        console.log(res)
    }
})
```

5. URL编码与解码

* encodeURI()  编码
* decodeURI()  解码

6. 使用xhr发起POST请求

* 创建xhr对象
* 调用xhr.open()函数
* *设置Content-Type属性*
* 调用xhr.send()函数，*同时指定要发送的数据*
* 监听xhr.onreadystatechange()事件

```js
 // 1. 创建xhr对象
var xhr = new XMLHttpRequest();

// 2. 调用open函数，指定请求方式与URL地址
xhr.open('POST', 'http://www.liulongbin.top:3006/api/getbooks');

// 3. 设置Content-Type属性
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

// 4. 调用send()，同时将数据以查询字符串的形式，提交给服务器
xhr.send('bookname=水浒传&author=施耐庵&publisher=天津图书出版社');

// 5. 监听onreadystatechange事件
xhr.onreadystatechange = function () {
    // 4.1 监听xhr对象的请求状态readyState; 与服务器响应的状态status
    if (xhr.readyState === 4 && xhr.status === 200) {  // xhr.status并不是响应的数据部分，而是http请求状态的一部分
        // 4.2 打印服务器响应回来的数据
        console.log(xhr.responseText);
    }
}
```

### 4.2 数据交换格式

数据交换格式，就是服务器端与客户端之间进行数据传输与交换的格式

1. XML(EXtensible Markup Language, 可扩展标记语言)

2. JSON(JavaScript Object Notation)
   JSON就是JavaScript对象和数组的字符串表示法，*本质就是字符串*

> var obj = {a: 'Hello', b: 'World'}
>
> var json = '{"a": "Hello", "b": "World"}'

3. JSON的两种结构

* 对象结构 {}
* 数组结构  []

4. JSON与JS对象的互转

> JSON -> JS
> var obj = JSON.parse('{"a": "Hello", "b": "World"}')

> JS -> JSON
> var json = JSON.stringify({a: 'Hello', b: 'World'})

5. 序列化与反序列化
   把数据对象转换为字符串的过程，叫做序列化  ``JSON.stringify()``

把字符串转换为数据对象的过程，叫做反序列化  ``JSON.parse()``

### 4.3 封装自己的Ajax函数
```html
<!-- 1. 导入自定义的ajax函数库-->
<script src="./xxx.js"></script>
<!--2. 调用自定义的xxx函数，发起Ajax数据请求-->
<script>
    xxx({
        method: '请求类型',
        url: '请求地址',
        data: { /* 请求参数对象 */},
        success: function (res) {  // 成功的回调函数
            console.log(res)
        }
    })
</script>
```
* 定义options参数选项
    * method
    * url
    * data
    * success
* 处理data参数
    * 需要把data对象，转化成查询字符串的格式，从而提交给服务器
    * 需提前定义resolveData函数
* 定义xxx函数
    * 在函数中创建xhr对象，并监听onreadystatechange事件
* 判断请求类型
    * 不同的请求类型，对应xhr对象的不同操作

```js
    /**
 * 处理data函数
 * @param {data} 需要发送到服务器的数据
 * @return {string} 返回拼接好的查询字符串  name=zs&age=18
 */
function resolveData(data) {
    var arr = [];
    for (let k in data) {
        arr.push(k + '=' + data[k])
    }
    ;
    return arr.join('&')
}

function xxx(options) {
    var xhr = new XMLHttpRequest();
    // 把外界传递过来的单参数对象，转换为 查询字符串
    var qs = resolveData(options.data);
    
    
    if (options.method.toUpperCase() === 'GET') {
        // 发起GET请求
        xhr.open(options.method, options.url + '?' + qs);
        xhr.send();
    } else if (options.method.toUpperCase() === 'POST') {
        // 发起POST请求
        xhr.open(options.method, options.url);
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send(qs);
    }
    
    
    // 监听请求状态改变的事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
            var result = JSon.parse(xhr.responseText);
            options.success(result);
        }
    }
}
```

## 5 XMLHttpRequest Level2的新特性

1. 旧版XMLHttpRequest的缺点
* 只支持文本数据的传输，无法用来读取和上传文件
* 传送和接收数据时，没有进度信息，只能提示有没有完成

2. 新特性
* 可以设置HTTP请求的时限
* 可以使用FormData对象管理表单数据
* 可以上传文件
* 可以获得数据传输的进度信息

### 5.1 设置HTTP请求时限

> xhr.timeout = 3000  单位ms

```js
 // 设置 超时时间
xhr.timeout = 30
 // 设置 超时以后的处理函数
 xhr.ontimeout = function () {
   console.log('请求超时了！')
 }
```

### 5.2 FormData对象管理表单数据
1. 提交表单数据
```js
    // 1. 新建FormData对象
    var fd = new FormData();

    // 2. 为FormData添加表单项
    fd.append('uname','zs');
    fd.append('upwd','123456');
    
    // 3. 创建XHR对象
    var xhr = new XMLHttpRequest();
    
    // 4. 指定请求类型与URL地址
    xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata');
    
    // 5. 直接提交FormData对象，这与提交网页表单的效果完全一样
    xhr.send(fd);

    // 监听请求状态改变的事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
            var result = JSon.parse(xhr.responseText);
            options.success(result);
        }
}
```
2. 获取网页表单的值
```js
    // 获取表单元素
    var form = document.querySelector('#form');

    // 监听表单元素的submit事件
    form.addEventListener('submit',function (e) {
        // 阻止表单的默认提交行为
        e.preventDefault();
        // 根据form表单创建FormData对象，会自动将表单数据填充到FormData对象中
        var fd = new FormData(form);
        var xhr = new XMLHttpRequest();
        
        xhr.open('POST','http://www.liulongbin.top:3006/api/formdata');
        xhr.send(fd);
        xhr.onreadystatechange = function (){
            if (xhr.readyState === 4 && xhr.status === 200) {
                var result = JSon.parse(xhr.responseText);
                options.success(result);
            }
        }
    })
```

### 5.3 上传文件
1. 定义UI结构
```html
    <input type="file" id="file1" />
    <button id="btnUpload">上传文件</button>
</br>
<!--img标签，来显示上传成功之后的照片-->
<img src="" id="img" width="800">
```
2. 验证是否选择了文件
3. 向FormData中追加文件
4. 使用xhr发起上传文件的请求
5. 监听onreadystatechange事件
```js
    // 1. 获取到文件上传按钮
    var btnUpload = document.querySelector('#btnUpload');
    // 2. 为按钮绑定单击事件处理函数
    btnUpload.addEventListener('click',function () {
        // 3. 获取到用户选择的文件列表
        var files = document.querySelector('#file1').files;
        if(files.length<=0){
            return alert('请选择要上传的的文件');
        }
        
        // 向FormData追加文件
        var fd = new FormData();
        // 将用户选择的文件，添加到FormData中
        fd.append('avatar',files[0]);
        
        // 使用xhr发起上传文件的请求
        var xhr = new XMLHttpRequest();
        xhr.open('POST','http://www.liulongbin.top:3006/api/formdata');
        xhr.send(fd);

        // 监听onreadystatechange事件
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4 && xhr.status === 200) {
                var data = JSon.parse(xhr.responseText);
                if(data.status === 200){
                    // 上传文件成功
                    // 将服务器返回的图片地址，设置为<img>标签的src属性
                    document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url;
                }else{
                    // 上传文件失败
                    console.log(data.message);
                }
            }
        }
    })
```

### 5.4 文件上传的进度

1. 计算文件上传的进度

> 通过监听xhr.upload.onprogress事件

2. 显示文件上传的进度
* 导入需要的库(bootstrap)
```js
     // 1. 获取到文件上传按钮
var btnUpload = document.querySelector('#btnUpload');
// 2. 为按钮绑定单击事件处理函数
btnUpload.addEventListener('click',function () {
    // 3. 获取到用户选择的文件列表
    var files = document.querySelector('#file1').files;
    if(files.length<=0){
        return alert('请选择要上传的的文件');
    }

    // 向FormData追加文件
    var fd = new FormData();
    // 将用户选择的文件，添加到FormData中
    fd.append('avatar',files[0]);

    // 使用xhr发起上传文件的请求
    var xhr = new XMLHttpRequest();
    
    // 监听文件上传的进度
    xhr.upload.onprogress = function (e) {
        // e.lengthComputable 是一个布尔值，表示当前上传的资源是否具有可计算的长度
        if(e.lengthComputable) {
            // e.loaded 已传输的字节
            // e.total 需传输的字节
            // 计算出上传的进度
            var percentComplete = Math.ceil((e.loaded/e.total)*100);
            
            // 动态显示进度条
            $('#percent')
                // 设置进度条的宽度
                .attr('style', 'width:' + percentComplete + '%')
                // 显示当前上传的进度百分比
                .html(percentComplete + '%');
        }
    }
    
    // 监听上传完成的事件
    xhr.upload.onload = function () {
        $('#percent')
            // 移除上传中的类样式
            .removeClass()
            // 添加上传完成的类样式
            .addClass('progress-bar progress-bar-success');
    }
    
    xhr.open('POST','http://www.liulongbin.top:3006/api/formdata');
    xhr.send(fd);

    // 监听onreadystatechange事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
            var data = JSon.parse(xhr.responseText);
            if(data.status === 200){
                // 上传文件成功
                // 将服务器返回的图片地址，设置为<img>标签的src属性
                document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url;
            }else{
                // 上传文件失败
                console.log(data.message);
            }
        }
    }
})   
```

## 6 jQuery高级

### 6.1 jQuery实现文件上传
1. 定义UI结构
2. 验证是否选择了文件
3. 向FormData中追加文件

```js
$(function () {
    $('#btnUpload').on('click',function () {
        var files = $('#file1')[0].files;
        if(files.length<=0){
            return alert('请选择文件后再上传!');
        }
        
        // 追加文件
        var fd = new FormData();
        fd.append('avatar', files[0]);
        // 发起jQuery的Ajax请求，上传文件
        $.ajax({
            method: 'POST',
            url: 'http://www.liulongbin.top:3006/api/upload/avatar',
            data: fd,
            // 不对FormData中的数据进行url编码，而是将FormData数据原样发送到服务器
            proceessData: false,
            // 不修改Content-Type属性，使用FormData默认的Content-Type值
            contentType: false,
            success: function (res) {
                
            }
        })
    })
})
```

### 6.2 jQuery实现loading效果

1. ajaxStart(callback)
> $(document).ajaxStart()函数会监听当前文档内所有的Ajax请求

```js
    // 自jQuery版本1.8起，该方法只能被附加到文档
    // 监听到Ajax请求发起了
    $(document).ajaxStart(function () {
        $('#loading').show()
    })
```

2. ajaxStop(callback)

```js
    // 监听到Ajax完成的事件
    $(document).ajaxStop(function () {
        $('#loading').hide()
    })
```

## 7. axios

Axios是专注于*网络数据请求*的库

### 7.1 axios发起GET请求
> axios.get('url', {param: { /* 参数 */}}).then(callback)

```js
document.querySelector('#btn1').addEventListener('click',function () {
    var url = 'http://www.liulongbin.top:3006/api/get';
    var paramObj = { name: 'zs', age: 20};
    axios.get(url, {param: paramObj}).then(function (res) {
        // console.log(res);
        // res.data是服务器返回的数据
        console.log(res.data);
    })
})
```

### 7.2 axios发起POST请求
> axios.post('url', {param: { /* 参数 */}}).then(callback)

```js
document.querySelector('#btn1').addEventListener('click',function () {
    var url = 'http://www.liulongbin.top:3006/api/post';
    var dataObj = { address: '北京', location: '顺义区'};
    axios.post(url, {param: dataObj}).then(function (res) {
        // console.log(res);
        // res.data是服务器返回的数据
        console.log(res.data);
    })
})
```

### 7.3 直接使用axios发起请求
```js
axios({
    mathod: '请求类型',
    url: '请求的URL地址',
    data: { /* POST数据 */ },
    params: { /* GET参数 */ }
}).then(callback)
```

## 8 跨域与JSONP

### 8.1 同源策略和跨域
1. 同源
    如果两个页面的*协议*，*域名*和*端口*都相同，则两个页面具有相同的源
2. 同源策略(Same origin policy)
    是浏览器提供的一个安全功能，A网站的JS，不允许和非同源的网站C之间进行资源的交互
3. 跨域
    同源的对立面
5. 浏览器对跨域请求的拦截
    浏览器允许发起跨域请求，但是，跨域请求回来的数据，会被浏览器拦截，无法被页面获取到
6. 如何实现跨域数据请求
* JSONP  只支持GET请求
* CORS  不兼容低版本

### 8.2 JSONP
1. JSONP是JSON的一种"使用模式"，可用于解决主流浏览器的跨域数据访问的问题
2. 实现原理：通过`<script>`标签的src属性，请求跨域的数据接口，并通过函数调用的形式，接收跨域接口响应回来的数据
> JSONP和Ajax之间没有任何关系，不能把JSONP请求数据的方式叫做Ajax，因为JSONP没有用到XMLHttpRequest这个对象
```html
<script>
    function success(data) {
        console.log('拿到了data数据:');
        console.log(data);
    }
</script>

<!--<script src="./js/getdata.js?callback=success"></script> -->
<script src="http://ajax.frontend.xiaoyi.net:3006/api/jsonp?callback=success&name=zs&age=20"></script>
```
3. JQuery中的JSONP
> jQuery采用的是动态创建和移除`<script>`标签的方式，来发起JSONP数据请求
```js
$.ajax({
    url: 'http://ajax.frontend.xiaoyi.net:3006/api/jsonp?name=zs&age=20',
    // 如果使用$.ajax()发起JSONP请求，必须指定datatype为jsonp
    datatype: 'jsonp',
    // 默认情况下，使用jQuery发起JSONP请求，会自动携带一个callback=jQueryxxx的参数，jQueryxxx是随机生成的一个回调函数的名称
    success: function (res) {
        console.log(res)
    }
})
```

4. 自定义参数及回调函数的名称
```js
$.ajax({
    url: 'http://ajax.frontend.xiaoyi.net:3006/api/jsonp?name=zs&age=20',
    datatype: 'jsonp',
    // 发送到服务端的参数名称，默认值为callback
    jsonp: 'callback',
    // 自定义的回调函数名称，默认为jQueryxxx格式
    jsonpCallback: 'abc',
    success: function (res) {
        console.log(res)
    }
})
```
### 8.3 防抖和节流

1. 防抖策略是当事件被触发后，延迟n秒后再执行回调，如果在这n秒内事件又被触发，则重新计时
```js
    var timer = null;
    // 定义防抖函数
    function debounceSearch(keywords) {
        timer = setTimeout(function () {
            // 发起Ajax请求
            getSuggestList(keywords)
        },500)
    }
    
    // 在触发keyup事件时，立即清空timer
    $('#ipt').on('keyup',function () {
        clearTimeout(timer),
        // ......
        debounceSearch(keywords)
    })
```

2. 节流策略：减少一段时间内事件的触发频率


## 9 HTTP协议
通信：信息的传递和交换
三要素：
* 通信的主体
* 通信的内容
* 通信的方式

### 9.1 HTTP请求
1. 请求行  ``请求方式、URL和HTTP协议版本``
2. 请求头  ``多行键值对``
3. 空行
4. 请求体

### 9.2 HTTP响应
1. 状态行  ``HTTP协议版本、状态码、状态码的描述文本``
2. 响应头部  ``多行键值对``
3. 空行
4. 响应体

### 9.3 HTTP响应状态码

三个十进制数字组成，第一个十进制数字定义了状态码的类型
* 1**  信息
* 2**  成功
* 3**  重定向
* 4**  客户端错误
* 5**  服务器错误