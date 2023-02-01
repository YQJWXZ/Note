# Html

## 1. Web标准

* 结构(html)
* 表现(css)
* 行为(js)

## 2. Html标签

### 1. 标题标签

### 2. 段落和换行标签

<br />

### 3. 文本格式化标签

```html
<strong>加粗</strong>
<em>倾斜</em>
<del>删除线</del>
<ins>下划线</ins>

<b>加粗</b>
<i>倾斜</i>
<s>删除线</s>
<u>下划线</u>
```

### 4. <div>和<span>标签

盒子，用来装内容

### 5. 图像标签

* src
* alt 替换文本，图像不能显示的文字
* title 提示文本，鼠标放到图像上，显示的文字
* weight
* height
* border 设置图像的边框粗细

### 6. 链接标签(anchor)

1. 外部链接
2. 内部链接
3. 空链接  `` href = "#" ``
4. 下载链接  `` href = "xxx.zip" ``
5. 锚点链接  `` href = "#p1" ``

> <p id="p1">要跳转的本页面其他位置<a href="#">回到顶部</a></p>

* href
* target 指定页面的打开方式，_self为默认值，_blank为在新窗口中打开

### 7. 表格标签

1. ``<table> </table>`` 定义表格
2. ``<tr> </tr>``  定义行
3. ``<th> </th>``  表头单元格
4. ``<td>  </td>`` 定义单元格
5. ``<thead> </thead>``  表格的头部区域
6. ``<tbody> </tbody>``  表格的主体区域

* align 表格相对周围元素的对齐方式  ``left、center、right``
* border 表格单元边框  `` 1或"" ``
* cellpadding 规定单元沿与其内容之间的空白，默认1像素
* cellspacing 规定单元格之间的空白，默认2像素
* width 规定表格的宽度

```html
<table>
    <thead>
    <tr>  <th>姓名</th>  <th>性别</th>  <th>年龄</th>  </tr>
    </thead>
    
    <tbody>
    <tr>  <td>xxx</td>  <td>xxx</td>  <td>xxx</td>  </tr>
    <tr>  <td>xxx</td>  <td>xxx</td>  <td>xxx</td>  </tr>
    </tbody>
</table>
```

#### 合并单元格

1. 跨行合并  `` rowsoan = "合并单元格的个数" ``
2. 跨列合并  `` colspan = "合并单元格的个数" ``

### 8. 列表标签

1. 无序列表

```html
<ul>
    <li>xxxxxx</li>
    <li>xxxxxx</li>
    <li>xxxxxx</li>
</ul>
```

2. 有序列表

```html
<ol>
    <li>xxxxx</li>
    <li>xxxxx</li>
    <li>xxxxx</li>
</ol>
```

3. 自定义列表

```html
<dl>
    <dt>关注我们</dt>
    <dd>新浪微博</dd>
    <dd>官方微博</dd>
    <dd>联系我们</dd>
</dl>
```

> list-style: none;  去掉li前面的项目符号

### 9. 表单标签

1. 表单域

* action：url地址，用于指定接收并处理表单数据的服务器程序的url地址
* method: get/post
* name: 指定表单域名称

2. 表单控件（表单元素）

* input：输入表单元素
  * button: 定义可点击按钮，后期结合js使用（如：获取短信验证码的）
  * checkbox：定义复选框
  * file：定义输入字段和浏览按钮，**供文件上传**
  * hidden: 定义隐藏的输入字段
  * image: 定义图像形式的提交按钮
  * password：定义密码字段
  * radio：单选按钮
  * reset：重置按钮
  * submit：提交按钮
  * text：单行的输入字段
  * data: 日期
* ``<lable>`` input元素自定义标注（用于绑定一个表单元素）

* name: 定义input元素的名称
* value：规定input元素的值
* checked: 规定此input元素首次加载时应当被选中
* maxlength：规定输入字段中的字符的最大长度

```html
<form action="url地址" method="post" name="xxx">
    <input type="text" name="uesrname">
    <input type="password" name="password">
    <input type="hidden" name="id">
    <input type="date" name="birthday">
    
<!--单选-->
    <input type="radio" name="sex" id="man" checked>
    <label for="man">男</label>
    <input type="radio" name="sex" id="woman">
    <label for="woman">女</label>
    
<!--多选-->
    <input type="checkbox" name="fav" value="唱歌">
    <input type="checkbox" name="fav" value="逛街">
    <input type="checkbox" name="fav" value="游戏">

    <input type="file" name="avatar">
</form>
```

* select：下拉表单元素
selected="selected": 默认选项

```html
<selecte>
    <option>选项1</option>
    <option selected="selected">选项2</option>
    <option>选项3</option>
</selecte>
```

* textarea：文本域元素

```html
<textarea rows="显示的行数" cols="每行中的字符数">
    文本内容
</textarea>
```

## 3. 特殊字符

* &nbsp 空格符
* &lt 小于
* &gt 大于
* &copy 版权
* &reg 注册商标
* ···

##  网站favicon图标

favicon.ico一般用于作为缩略的网站标志，它显示在浏览器的地址栏或者标签上