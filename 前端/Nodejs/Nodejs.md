# Node.js

1. 浏览器中的JavaScript运行环境
* V8引擎负责解析和执行JavaScript代码
* 内置API是由运行环境提供的特殊接口，只能在所属的运行环境中被调用

2. Node.js
> 基于Chrome V8引擎的JavaScript运行环境

## 1. fs文件系统模块

### 1.1 读取写入
> const fs = require('fs')   导入fs模块 

* fs.readFile()  读取指定文件中的内容
    * fs.readFile(path, [options], callback)
```js
    const fs = require('fs');
    // 参数1：读取文件的存放路径
    // 参数2：读取文件时候采用的编码格式，一般默认指定utf-8
    // 参数3：回调函数，拿到读取失败和成功的结果 err dataStr
    fs.readFile('./files/1.txt', 'utf-8', function (err, dataStr) {
        // 如果读取成功，则err的值为null
        // 如果读取失败，则err的值为错误对象 dataStr的值为undefined
        console.log(err);
        console.log("-------");
        console.log(dataStr);
        
        // 判断文件是否读取成功
        if(err){
            return console.log("读取文件失败"+err.message);
        }
        console.log('读取文件成功'+dataStr);
    })
```
* fs.writeFile()
    * fs.writeFile(file, data, [options], callback)
```js
    const fs = require('fs');

    // 参数1：表示文件的存放路径
    // 参数2：表示要写入的内容
    // 参数3：回调函数
    fs.writeFile('./files/2.txt','abcd', function (err) {
        // 如果文件写入写入成功，则err的值为null
        // 如果文件写入失败，则err的值等于一个错误对象
        console.log(err);


        // 判断文件是否写入成功
        if(err){
            return console.log("文件写入失败"+err.message);
        }
        console.log('文件写入成功'+dataStr);
    })
```

### 1.2 fs模块 - 路径动态拼接
在使用fs模块操作文件时，如果提供的操作路径是以./或../开头的相对路径时，很容易出现路径动态拼接错误的问题
> 代码在运行时，会以执行node命令时所处的目录，动态拼接出被操作文件的完整路径
```js
    const fs = require('fs');
    // __dirname 表示当前文件所处的目录
    fs.readFile(__dirname + './files/1.txt', 'utf-8', function (err, dataStr) {
        console.log(err);
        console.log("-------");
        console.log(dataStr);
        
        if(err){
            return console.log("读取文件失败"+err.message);
        }
        console.log('读取文件成功'+dataStr);
    })
```
## 2. path路径模块
如果要在JavaScript代码中，使用path模块来处理路径，则需要使用如下的方式先导入它：
> const path = require('path')

* path.join([..paths])  用来将多个路径片段拼接成一个完整的路径字符串
  * ../ 会抵消前面的路径
  * *今后凡是涉及到路径拼接的操作，都要使用path.join()方法进行处理*
* path.basename(path, [ext])  用来从路径字符串中，将文件名解析出来
* path.extname(path)  获取路径中的扩展名 

## 3. http模块

http模块是Node.js官方提供的、用来创建web服务器的模块。  ``http.createServer()``
1. 导入http模块
```js
// 导入http模块
const http = require('http')
```

2. 创建web服务器实例
```js
const server = http.createServer()
```

3. 为服务器实例绑定request事件
```js
server.on('request',(req,res) =>{
    res.setHeader('Content-Type','text/html; charset=utf-8')
    // .....
})
```

4. 启动服务器
```js
server.listen(80, ()=>{
    // ....
})
```

优化资源的请求路径：
```js
let fpath = ''
if(url === '/'){
    fpath = path.join(__dirname, './clock/index.html')
}else{
    fpath = path.join(__dirname,'./clock', url)
}
```


## 4. 模块化

1. Nodejs中模块的分类
* 内置模块
* 自定义模块
* 第三方模块

2. 模块作用域 

3. 向外共享模块作用域中的成员
* module对象  ``存储了和当前模块有关的信息``
* module.exports对象  ``在自定义模块中可以使用此对象，将模块内的成员共享出去，供外界使用``
* exports对象   ``默认情况下，exports和module.exports指向同一对象``

> 使用require()方法导入模块时，导入的结果永远是以module.exports指向的对象为准

4. Nodejs中的模块化规范

遵循了CommonJS模块化规范，CommonJS规定了模块的特性和各模块之间如何相互依赖

CommonJS规定：
* 每个模块内部，module变量代表当前模块
* module变量是一个对象，它的exports属性是对外的接口
* 加载某个模块，其实是加载该模块的module.exports属性。require()方法用于加载模块

### 4.1 npm与包
> npm install 包的完整名称

* node_modules 用来存放所有已安装到项目中的包
* package-lock.json 配置文件，用来记录node_modules目录下的每一个包的下载信息，例如包的名字、版本号、下载地址等

1. 包管理配置文件
> npm init -y   快速新建package.json文件
* dependencies节点  ``用来记录使用npm命令安装了哪些包``
* devDependencies节点  ``项目开发会用到的，项目上线后不会用到的包记录到此节点 npm install 包名 -D ``

2. 包的分类
* 项目包
  * 开发依赖包
  * 核心依赖包
* 全局包

3. 规范的包结构
* 包必须以单独的目录而存在
* 包的顶级目录下必须包含package.json这个包管理配置文件
* package.json中必须包含name,version,main这三个属性

4. 开发属于自己的包
* 初始化包的基本结构
  * 包的根目录
    * package.json (包管理配置文件)
    * index.js (包的入口文件)
    * README.md (包的说明文档)
* 初始化package.json
```json
{
  "name": "",
  "version": "",
  "main": "index.js",
  "description": "",
  "keywords": ["",""],
  "license": "ISC"
}
```
* 在index.js中定义转义HTML的方法
```js
function htmlEscape(htmlStr){
    return htmlStr.replace(/<|>|"|&/g,(match)=>{
        switch (match) {
          case '<':
              return '&lt;'
          case '>':
              return '&gt;'
          case '"':
              return '&quot;'
          case '&':
              return '&amp;'
        }
    })
}
```
* 在index.js中定义还原HTML的方法
```js
function htmlUnEscape(str){
    return str.replace(/&lt;|&gt;|&quot;|&amp;/g,(match)=>{
        switch (match) {
          case '&lt;':
              return '<'
          case '&gt;':
              return '>'
          case '&quot;':
              return '"'
          case '&amp;':
              return '&'
        }
    })
}
```
* 将不同的功能进行模块化拆分
```js
const date = require('./src/dateFormat')
const escape = require('./src/htmlEscape')

module.exports = {
    ...date,
    ...escape
}
```
* 编写包的说明文档
* 发布包
> npm pulish


### 4.2 模块的加载机制

1. 优先从缓存中加载

2. 内置模块的加载机制  ``内置模块加载优先级最高``

3. 自定义模块的加载机制  ``必须指定以./或../开头的路径标识符``

4. 第三方模块的加载机制


## 5. Express
> npm i express@4.17.1

1. 创建基本的Web服务器
```js
const express = require('express')

const app = express()

app.listen(80,()=>{
    // .....
})
```

2. 监听GET请求
> app.get('请求URL', function(req,res) {/*处理函数*/})

3. 监听POST请求
> app.post('请求URL', function(req,res) {/*处理函数*/})

4. 把内容响应给客户端
> res.send()

5. 获取URL中携带的查询参数
> req.query()

6. 获取URL中的动态参数
> req.params()    app.get('/user/:id/:name',(req,res)=>{})

7. 托管静态资源
> express.static()  非常方便的创建一个静态资源服务器
> 
> app.use('/public', express.static('public'))  挂载路径前缀

### 5.1 nodemon
> npm install -g nodemon

### 5.2 Express路由

路由：映射关系  ``在Express中，路由指的是客户端的请求与服务器处理函数之间的映射关系``

模块化路由：(将路由抽离为单独的模块)
* 创建路由模块对应的js文件
* 调用express.Router()函数创建路由对象
* 向路由对象上挂载具体的路由
* 使用module.exports向外共享路由对象
* 使用app.use()函数注册路由模块
```js
var express = require('express')  // 1. 导入express
var router = express.Router()  // 2. 创建路由对象

router.get('/user/list', function (req,res) {  // 3. 挂载获取用户列表的路由
  res.send()
})
router.post('./user/add',function (req,res) {  // 4. 挂载添加用户的路由
  res.send()
})

module.exports = router  // 5. 向外导出路由对象
```

### 5.3 Express中间件

1. 中间件：特指业务流程的中间处理环节

> app.get('/', function(req,res,next){next();}
>
> 中间件函数的形参列表中，必须包含next参数。而路由处理函数只包含req和res
> 
> next函数是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由

2. 中间件的作用：多个中间件之间，共享同一份req和res。基于这样的特性，可以在上游的中间件中，
统一为req或res对象添加自定义的属性或方法，供下游的中间件或路由进行使用

* 全局生效的中间件  ``app.use()``
* 局部生效的中间件

3. 使用中间件的5个使用注意事项：
* 一定要在路由之前注册中间件
* 客户端发送过来的请求，可以连续调用多个中间件进行处理
* 执行完中间件的业务代码之后，不要忘记调用next()函数
* 为了防止代码逻辑混乱，调用next()函数后不要写额外的代码
* 连续调用多个中间件时，多个中间件之间，共享req和res对象

4. 中间件的分类
* 应用级别的中间件
  * app.use()、app.get()、app.post()
* 路由级别的中间件
  * express.Router()
* 错误级别的中间件
  * function(err,req,res,next)
  * 错误级别的中间件，必须注册在所有路由之后
* Express内置的中间件
  * express.static()
  * express.json()
  * express.urlencoded({extended: false})
* 第三方的中间件
  * 运行npm install body-parser 安装中间件
  * 使用require导入中间件
  * 调用app.use()注册并使用中间件

5. 自定义中间件

### 5.4 使用Express写接口

1. 创建API路由模块
```js
// apiRouter.js [路由模块]
const express = require('express')
const apiRouter = express.Router()

// bind your router here...

module.exports = apiRouter

// ----------

// app.js [导入并注册路由模块]
const apiRouter = require('./apiRouter.js')
const app = express
app.use('./api', apiRouter)
```

2. 编写GET接口
```js
apiRouter.get('./get',(req,res) =>{
    const query = req.query
  
    req.send({
      status: 0,
      msg: 'GET请求成功！',
      data: query
    })
})
```

3. 编写POST接口
```js
apiRouter.post('./post',(req,res) =>{
    const body = req.body
  
    req.send({
      status: 0,
      msg: 'POST请求成功！',
      data: body
    })
})
```

4. 接口的跨域问题
* CORS(垮资源共享，主流的解决方案，推荐使用)
* JSONP(有缺陷的解决方案，只支持GET请求)

5. 使用CORS中间件解决跨域问题
* 运行npm install cors 安装中间件
* 使用const cors = require('cors') 导入中间件
* 在路由之前调用app.use(cors()) 配置中间件

### 5.5 前后端的身份认证

1. 在Express中使用Session认证
> npm install express-session
```js
var session = require('express-session')

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))
```

向session中存数据：
```js
app.post('/api/login',(req,res)=>{
    if(req.body.username !== 'admin'|| req.body.password !== '000000'){
        return res.send({status: 1, msg: '登陆失败'})
    }
    
    req.session.user = req.body
    req.session.islogin = true
   
    res.send({status: 0, msg: '登录成功'})
})
```

从session中取数据：
```js
app.get('./api/username', (req,res) => {
    if(!req.session.islogin) {
        return res.send({status: 1, msg: 'fail'})
    }
    
    res.send({status: 0, msg: 'success', username: req.session.user.username})
})
```

清空session:
```js
app.post('./api/logout', (req,res) => {
    req.session.destory()
    res.send({
      status: 0,
      msg: '退出登录成功'
    })
})
```

2. JWT认证机制
JWT的工作原理：用户的信息通过Token字符串的形式，保存在客户端浏览器中。服务器通过还原Token字符串的形式来认证用户的身份

JWT的组成部分：Header(头部).Payload(有效荷载).Signature(签名)  ``Payload部分才是真正的用户信息``

JWT的使用方式：把JWT放在HTTP请求头的Authorization字段中  ```Authorization: Bearer <token>```

* 当前端请求后端接口不存在跨域问题的时候，推荐使用Session身份认证机制
* 当前端需要跨域请求后端接口的时候，不推荐使用Session身份认证机制，推荐使用JWT认证机制

3. 在Express中使用JWT
> npm install jsonwebtoken express-jwt
* 导入JWT相关的包
* 定义secret密钥  ``const secretKey = ' ' 本质上就是一个字符串``
* 登录成功后生成JWT字符串
```js
app.post('./api/login', function (req, res) {
  res.send({
    status: 200,
    message: '登录成功',
    token: jwt.sign({username: userinfo.username}, secretKey, {expiresIn: '30s'})
  })
})
```
* 将JWT字符串还原为JSON对象
> app.use(expressJWT({ secret: secretKey }).unless( { path: [/^\/api\//] }))  unless用来指定哪些接口不需要访问权限
* 使用req.user获取用户信息
* 捕获解析JWT失败后产生的错误