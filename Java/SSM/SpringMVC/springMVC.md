# SpringMVC
一种基于Java实现MVC模型的轻量级Web框架
## 1. SpringMVC入门
> 容器初始化过程
>
> 单次请求工作流程


1. 使用SpringMVC技术需要先导入SpringMVC坐标与Servlet坐标
2. 创建SpringMVC控制器类（等同于SServlet功能）
```java
// 2. 定义Controller
// 2.1 使用@Controller定义bean
@Controller
public class UserController {
    // 2.2 设置当前操作的访问路径
    @RequestMapping("/save")
    // 2.3 设置 当前操作的返回值类型
    @ResponseBody
    public String save(){
        System.out.println("user save...");
        return "{'module':'springmvc'}";
    }
}
```
3. 初始化SpringMVC环境（同Spring环境），设定SpringMVC加载对应的bean
```java
// 3. 创建SpringMVC的配置文件，加载controller对应的bean
@Configuration
@ComponentScan("com.xiaoyi.controller")
public class SpringMVCConfig {

}
```
4. 初始化Servlet容器，加载SpringMVC环境，并设置SpringMVC技术处理的请求
```java
// 4. 定义一个servlet容器启动的配置类，在里面加载spring的配置
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {

    // 加载SpringMVC容器配置
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx=new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMVCConfig.class);
        return ctx;
    }

    // 设置哪些请求归属SpringMVC处理
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    // 加载spring容器配置
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

简化开发
```java
// 简化开发
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMVCConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```
### Controller加载控制与业务bean加载控制
* SpringMVC相关bean(表现层bean)
* Spring控制的bean
    * 业务bean(Service)
    * 功能bean(DataSource等)

> 因为功能不同，如何避免Spring错误的加载到SpringMVC的bean？
>
> 加载Spring控制的bean的时候排除掉SpringMVC控制的bean

> 1. SpringMVC加载的bean对应的包均在com.xiaoyi.controller包内
> 
> 2. Spring相关bean加载控制
> 
> 方式1：全扫描，排除掉controller包内的bean
```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;

@Configuration
@ComponentScan(value = "com.xiaoyi",
        excludeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION,
                classes = Controller.class
        )
)
public class SpringMVCConfig {

}
```
> 方式2：精准扫描，扫描service、dao包等
> 
> 方式3：不区分Spring与SpringMVC的环境，加载到同一个环境中
## 2. 请求与响应
1. 请求映射路径
设置模块名作为请求路径前缀  ``方法注解 类注解``
>  @RequestMapping("/user")

2. 请求参数
* Get请求
> http://localhost/user?name=xiaoyi&age=20
> 
> 方法内部传参``解决请求参数名与形参名不同 在形参前加@RequestParam("name") String userName``
* Post请求
> http://localhost/user
> 
> body --> x-www-form-urlencoded(Postman为例)
> 
> 中文乱码处理： 过滤器添加指定字符集
```java
    // 中文乱码处理：过滤器
@Override
protected Filter[] getServletFilters(){
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
        }
```

参数种类：
* 普通参数
* POJO类型参数
* 嵌套POJO类型参数
* 数组类型参数
* 集合类型参数


### json数据传递参数
* json数组
>  格式：["aa","bb","cc"]
* json对象(POJO)
>  格式：{"name":"xiaoyi","age":"20"}
* json数组(POJO)
> 格式：[
> 
>             {"name":"xiaoyi","age":"20"},
>             {"name":"xiaoyi","age":"20"}
>      
>          ]     

### 步骤：
    1. 加坐标jackson-databind、jackson-annotations、jackson-core
    2. 请求body中添加json数据
    3. 开启自动转换json数据的支持: 在Config配置中添加 @EnableWebMvc注解
    4. 设置jieshoujson数据： 在参数传递那儿添加 @RequestBody注解

```
@RequestBody 与 @RequestParam 的区别：
    * @RequestParam接收url地址传参，表单传参  [application/x-www-form-urlencoded]
    * @RequestBody用于接收json数据  [application/json]
```

3. 日期类型参数传递
* 日期类型数据基于系统不同格式也不尽相同
    * 2088-08-18~~~~
    * 2088/08/18
    * 08/18/2088
*  接收形参时，根据不同的日期格式设置不同的接收方式
```
public String dataParam(Date date,
                        @DateTimeFormat(pattern = "yyyy-MM-dd") Date date1,
                        @DateTimeFormat(pattern = "yyyy/MM/dd HH:mm:ss" Date date2)
```
4. 响应

HttpMessageConverter(类型转换器)接口

* 响应页面

```java
import org.springframework.web.bind.annotation.RequestMapping;

@RequestMapping("/toPage")
public String toPage(){
    return "page.jsp";
        }
```
* 响应数据
  1. 文本数据

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@RequestMapping("toText")
@ResponseBody    // 设置当前控制器返回值作为响应体
public String toText(){
    return "response text";
        }
```
   2. json数据(对象转json)

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@RequestMapping("/toJsonPOJO")
@ResponseBody    // 设置当前控制器返回值作为响应体
public User toJsonPOJO(){
    User user = new User();
    user.setname("赵云");
    user.setAge(41);
    return user;
        }
```
## 3. REST风格
1. REST

REST : 表现形式状态转换,这种资源的描述形式叫做REST风格

隐藏资源的访问行为，无法通过地址得知对资源是何种操作

访问行为：
* GET
* POST
* PUT
* DELETE

2. RESTful入门案例

RESTful : 根据REST风格对资源进行访问称为RESTful

1. 设置http请求动作
2. 设定请求参数（路径变量）
```java
@Controller
public class UserController {
    
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save(){
        System.out.println("users save...");
        return "{'module':'users save'}";
    }
    
    @RequestMapping(value = "/users/{id}", method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable Integer id){
        System.out.println("user delete..."+id);
        return "{'module':'user delete'}";
    }

    @RequestMapping(value = "/users", method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User user){
        System.out.println("user update..."+user);
        return "{'module':'user update'}";
    }

    @RequestMapping(value = "/users/{id}", method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }

    @RequestMapping(value = "/users", method = RequestMethod.GET)
    @ResponseBody
    public String getAll(){
        System.out.println("user getAll...");
        return "{'module':'user getAll'}";
    }

}
```
### @RequestBody   @RequestParam    @PathVariable 
* 区别
  * @RequestParam用于接收url地址传参或表单传参
  * @RequestBody用于接收json数据
  * @PathVariable用于接收路径参数，使用{参数名称}描述路径参数

3. 快速开发
> @RestController   等同于@Controller与@ResponseBody两个注解组合功能
> @GetMapping  @PostMapping  @PutMapping  @DeleteMapping

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping
    public String save(){
        System.out.println("users save...");
        return "{'module':'users save'}";
    }

    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("user delete..."+id);
        return "{'module':'user delete'}";
    }

    @PutMapping
    public String update(@RequestBody User user){
        System.out.println("user update..."+user);
        return "{'module':'user update'}";
    }

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }
    
    @GetMapping
    public String getAll(){
        System.out.println("user getAll...");
        return "{'module':'user getAll'}";
    }

}
```

### 案例：基于RESTful页面数据交互

1. 制作SpringMVC容器，并通过PostMan测试接口功能
```java
@RestController
@RequestMapping("/books")
public class BookController {
    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save ==> "+book);
        return "{'module':'book save success'}";
    }
    
    @GetMapping
    public List<Book> getAll(){
        List<Book> bookList=new ArrayList<>();

        Book book1=new Book();
        book1.setType("计算机");
        book1.setName("SpringMVC入门教程");
        book1.setDescription("小试牛刀");
        bookList.add(book1);

        Book book2=new Book();
        book2.setType("计算机");
        book2.setName("SpringMVC实战教程");
        book2.setDescription("一代宗师");
        bookList.add(book2);
        
        Book book3=new Book();
        book3.setType("计算机丛书");
        book3.setName("SpringMVC实战教程进阶");
        book3.setDescription("一代宗师呕心创作");
        bookList.add(book3);

        return bookList;
    }
}
```
2. **设置对静态资源的访问放行**
```java
@Configuration
public class SpringMVCSupport extends WebMvcConfigurationSupport {
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        // 当访问/pages/???的时候，走/pages目录下的内容，不要被springmvc拦截
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}
```
3. 前端页面通过异步提交访问后台控制器


## 4. SSM整合
1. SSM整合
   1. 创建工程
   2. SSM整合
      * Spring
          * SpringConfig
      * MyBatis
          * MybatisConfig
          * JdbcConfig
          * jdbc.properties
      * SpringMVC
          * ServletConfig
          * SpringMVCConfig
   3. 功能模块
      * 表与实体类
      * dao(接口+自动代理)
      * service(接口+实现类)
          * 业务层接口测试(整合JUnit)
      * controller
          * 表现层接口测试(PostMan)
2. 表现层数据封装
* 前端接收数据格式--创建结果模型类，**封装数据**到data属性中
* 前端接收数据格式--**封装操作**结果到code属性中
* 前端接收数据格式--**封装特殊消息**到message(msg)属性中

3. 异常处理器
出现异常现象的常见位置与常见诱因：

* 框架内部抛出的异常：使用不合规
* 数据层抛出的异常：外部服务器故障（如：服务访问超时）
* 业务层抛出的异常：业务逻辑书写（如：遍历业务书写）
* 表现层抛出的异常：数据收集、校验规则（如：不匹配的数据类型）
* 工具类抛出的异常：工具内书写不严谨不够健壮（如：必要释放的连接长期未释放）

> 所有的异常均抛出到表现层进行处理
>
> 异常使用AOP思想：异常处理器

异常处理器
```java
@RestControllerAdvice
public class ProjectExceptionAdvice {
    @ExceptionHandler(Exception.class)
    public void doException(Exception e){

    }
}
```
4. 项目异常处理方案
项目异常分类：
* 业务异常(BusinessException)
    * 发送对应消息传递给用户，提醒规范操作
* 系统异常(SystemException)
    * 发送固定消息传递给用户，安抚用户
    * 发送特定消息给运维，提醒维护
    * 记录日志
* 其他异常(Exception)
    * 发送固定消息传递给用户，安抚用户
    * 发送特定消息给编程人员，提醒维护
    * 记录日志
## 5. 拦截器
一种动态拦截方法调用的机制，在SpringMVC中动态拦截控制器方法的执行

作用：
* 在指定的方法调用前后执行预先设定的代码
* 阻止原始方法的执行

### 拦截器与过滤器的区别
* 归属不同：Filter属于Servlet技术，Interceptor属于SpringMVC技术
* 拦截内容不同：Filter对所有访问进行增强，Interceptor仅针对SpringMVC的访问进行增强

1. 入门案例
* 声明拦截器的bean，并实现HandInterceptor接口（注意扫描加载bean）
* 定义配置类，继承WebMvcConfigurationSupport，实现addInterceptor方法（注意扫描加载配置）
* 添加拦截器并设定拦截的访问路径，路径可以通过可变参数设置多个
* 使用标准 接口WebMvcConfigurer简化开发（注意侵入式较强）

拦截器执行顺序：
* preHandle
    * return true
        * controller
        * postHandle
        * afterCompletion
    * return false
        * 结束

2. 拦截器参数
* request
* response
* handle
* modelAndView
* ex

3. 拦截器工作流程
拦截器链
