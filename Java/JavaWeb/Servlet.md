# Servlet

## 1. 快速入门
1. 创建Web项目，导入Servlet依赖坐标
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```
2. 创建：定义一个类，实现Servlet接口，并重写接口中所有的方法
3. 在类上使用@WebServlet注解，配置该Servlet的访问路径
4. 访问：启动Tomcat, 浏览器输入URL访问该Servlet
```java
@WebServlet("/demo")
public class ServletDemo implements Servlet{
    public void service(){}
}
```
## 2. Servlet生命周期

Servlet运行在Servlet容器(Web服务器)中，其生命周期由容器来管理，分为4个阶段：
1. 加载和实例化  ``默认情况下，当Servlet第一次被访问时，由容器创建Servlet对象``
```
@WebServlet(urlPatterns="/demo", loadOnStartup=1)
1. 负整数：第一次被访问时创建Servlet对象
2. 0或正整数：服务器启动时创建Servlet对象，数字越小优先级越高
```
2. 初始化 ``在Servlet实例化后，容器将调用Servlet的init()方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作，该方法只调用一次``
3. 请求处理 ``每次请求Servlet时，Servlet容器都会调用Servlet的service()方法对请求进行处理``
4. 服务终止 ``当需要释放内存或者容器关闭时，容器就会调用Servlet实例的destroy()方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet实例，该实例随后会被Java的垃圾收集器所回收``

## 3. Servlet方法
1. void init(ServletConfig config)
2. void service(ServletRequest req, ServletResponse resp)  提供服务方法
3. void destroy() 销毁方法
4. ServletConfig getServletConfig()  获取ServletConfig对象
5. String getServletInfo()  获取Servlet信息

## 4. Servlet体系结构
```
 Servlet   Servlet体系根接口
    ^
    |
 GenericServlet   Servlet抽象实现类
    ^
    | 
 HttpServlet  对HTTP协议封装的Servlet实现类
```
``将来开发B/S架构的web项目，都是针对HTTP协议，所以我们自定义Servlet，会继承HttpServlet``

## 5. urlPattern配置

1. 一个Servlet，可以配置多个urlPattern
> @WebServlet(urlPatterns={"/demo1", "/demo2"})

2. urlPattern配置规则
* 精确匹配
* 目录匹配
> @WebServlet("/user/*")
* 拓展名匹配
> @WebServlet("*.do")
* 任意匹配  ``会覆盖掉tomcat中的DefaultServlet``
> @WebServlet("/")
> 
> @WebServlet("/*")

## 6. XML配置Servlet

Servlet从3.0版本后开始支持使用注解配置

XML配置方式编写Servlet:
1. 编写Servlet类继承HttpServlet
2. 在web.xml中配置Servlet
```xml
<!--Servlet全类名-->
<sevlet>
    <servlet-name>demo1</servlet-name>
    <servlet-class>com.xiaoyi.servlet.ServletDemo1</servlet-class>
</sevlet>
<!--Servlet访问路径-->
<servlet-mapping>
    <servlet-name>demo1</servlet-name>
    <url-pattern>/demo1</url-pattern>
</servlet-mapping>
```
## 7. Request&Response

### 7.1 Request(获取请求数据)
1. Request继承体系
```
 Servlet   Java提供的请求对象根接口
    ^
    |
 HttpServletRequest  Java提供的对Http协议封装的请求对象接口
    ^
    |
 RequestFacade  Tomcat定义的实现类
```
2. Request获取请求数据
* 请求行
    * request.getMethod()  获取请求方式
    * request.getContextPath()  获取虚拟目录(项目访问路径)
    * request.getRequestURL()  获取URL(完整路径) ``返回类型为StringBuffer,需toString``
    * request.getRequestURI()  获取URI(局部路径)
    * request.getQueryString()  获取请求参数(**GET**)
* 请求头
    * request.getHeader(String name) 根据请求头名称，获取值
* 请求体
    * ServletInputStream getInputStream()  获取字节输入流
    * BufferedReader getReader()  获取字符输入流(**POST**)

3. 通用方式获取请求参数
* Map<String, String[]> getParameterMap() 获取所有参数Map集合
* String[] getParameterValues(String name) 根据名称获取参数值(数组)
* String getParameter(String name) 根据名称获取参数值(单个值)

请求参数中文乱码处理：
> POST解决方案：request.setCharacterEncoding("UTF-8");
> 
> GET解决方案：URL编码再URL解码
```
URL编码：
1. 字符串按照编码方式转为二进制
2. 每个字节转为2个16进制并在前边加上%
encode = URLEncoder.encode(username, "utf-8");

URL解码：
decode = URLDecoder.decode(encode,"utf-8");
```
4. Request请求转发

请求转发：一种在服务器内部的资源跳转方式
> req.getRequestDispatcher("资源B路径").forward(req,resp);


请求转发特点：
* 浏览器地址栏路径不发生变化
* 只能转发到当前服务器的内部资源
* 一次请求，可以在转发的资源间使用request共享数据 


请求转发资源间共享数据：使用Request对象
* void setAttribute(String name, Object o);  存储到request域中
* Object getAttribute(String name); 根据key, 获取值
* void removeAttribute(String name); 根据key, 删除该键值对


### 7.2 Response(设置响应数据)

1. Response继承体系
```
 Servlet  Java提供的请求对象根接口
    ^
    |
 HttpServletResponse  Java提供的对Http协议封装的请求对象
    ^
    |
 ResponseFacade  Tomcat定义的实现类
```

2. Response设置响应数据
* 响应行 ``HTTP/1.1 200 OK``
    * void setStatus(int sc);  设置响应状态码
* 响应头 ``Content-Type:text/html``
    * void setHeader(String name, String value);  设置响应头键值对
* 响应体  ``html、css、js代码``
    * PrintWriter getWriter();  获取字符输出流
    * ServletOutputStream getOutputStream();  获取字节输出流

3. 重定向
> resp.setStatus(302);
> 
> resp.setHeader("location","资源B的路径");

> resp.sendRedirect("资源B的路径");

重定向特点：
* 浏览器地址栏路径发生变化
* 可以重定向到任意位置的资源
* 两次请求，不能在多个资源使用request共享数据

4. 资源路径问题
* 浏览器使用：需要加虚拟目录
* 服务端使用：不需要加虚拟目录

动态获取虚拟目录：
```java
    String contextPath = request.getContextPath();
    response.sendRedirect(context + "/resp2");
```

5. Response响应数据
* 字符数据
    * PrintWriter writer = resp.getWriter();  获取字符输出流(**流不需要关闭**)
    * writer.write(); 写数据
* 字节数据
    * 读取文件 -> 获取输出流 -> 完成流的copy
```java
  FileInputStream fis = new FileInputStream("D://a/jpg");
  ServletOutputStream fos = response.getOutputStream();
  IOUtils.copy(fis,fos);   // IOUtils工具类导入(commons-io)
  fis.close();
```
> 中文数据乱码问题: response.setContentType("text/html; charset=utf-8");