# Filter

过滤器，可以把对资源的请求拦截下来，从而实现一些特殊的功能, 比如权限控制、统一编码处理、敏感字符处理

## 1. Filter快速入门
1. 定义类，实现Filter接口
2. 配置Filter拦截资源的路径：在类上定义@WebFilter注解
3. 在doFilter方法中输出一句话，并放行
```java
@WebFilter("/*")
public class FilterDemo implements Filter{
    public void init(FilterConfig filterConfig){}
        
    public void doFilter(ServletRequest request,ServletResponse response,FilterChain chain){
        System.out.println("FilterDemo...");
        
        // 放行
        chain.doFilter(request,response);
    }
    public void destroy(){}    
}
```

## 2. Filter执行流程

执行放行前逻辑 --> 放行 --> 访问资源 --> 执行放行后逻辑

## 3. Filter使用细节

1. Filter拦截路径配置
* 拦截具体的资源
* 目录拦截
* 后缀名拦截
* 拦截所有

2. 过滤器链
