# JSP

Java Server Pages, Java服务端页面

## 1. 快速入门
1. 导入JSP坐标
```xml
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>3.0</version>
    <scope>provided</scope>
</dependency>
```
## 2. JSP原理
1. JSP = HTML + Java  **本质就是一个Servlet**

2. JSP在被访问时，由JSP容器(Tomcat)将其转换为Java文件(Servlet)，再由JSP容器(Tomcat)将其编译，最终对外提供服务的其实就是这个字节码文件

## 3. JSP脚本
1. <%...%>  内容会直接放到_jspService()方法之中
2. <%=...%> 内容会放到out.print()中，作为out.print()的参数
3. <%!...%> 内容会放到_jspService()方法之外，被类直接包含

## 4. EL表达式(Expression Language)
> ${expression} 获取域中存储的key为expression的数据

JavaWeb中的四大域对象：
* page  当前页面有效
* request 当前请求有效
* session 当前会话有效
* application 当前应用有效
> EL表达式获取数据，会一次从这4个域中寻找，直到找到为止

## 5. JSTL标签
JSP标准标签库，使用标签取代JSP页面上的Java代码
1. JSTL快速入门
```xml
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency> 
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
```
2. 在JSP页面上引入JSTL标签库

``<%@ taglib prefix="c" uri="http://java.sun.com/jstl/core" %>``

3. 使用
> < c:if >
```jsp
    <c:if test="${status == 1}">
        <h1>true</h1>
    </c:if>
    
    <c:if test="${status == 0}">
        <h1>false</h1>
    </c:if>
    
    <c:forEach items="${brands}" var="brand">    // items: 被遍历的容器 var: 遍历产生的临时变量  varStatus: 遍历状态对象
    <tr align="center">
        <td>${brand.id}</td>
        <td>${brand.brandName}</td>
    </tr>
    </c>
    
    <c:forEach begin="0" end="10" step="1" var="i">
        ${i}
    </c>
``` 
> <c:forEach></c:forEach>
```
<c:choose>
    <c:when> </c:when>
    <c:otherwise>  </c:otherwise>
</c:choose>
```
## 6. MVC模式和三层架构

1. MVC是一种分层开发的模式，其中：
* M: Model, 业务模型，处理业务
* V: View, 视图，页面展示
* C: Controller, 控制器，处理请求，调用模型和视图

2. 三层架构
* 表现层: 接收请求，封装数据，调用业务逻辑，响应数据 ``com.example.web/controller``
* 业务逻辑层：对业务逻辑进行封装，组合数据访问层中基本功能，形成复杂的业务逻辑功能  ``com.example.service``
* 数据访问层：对数据库的CRUD基本操作  ``com.example.dao/mapper``