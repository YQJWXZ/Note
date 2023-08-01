# Swagger

1. 使用方式
* 导入knife4j坐标
```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
```
* 导入knife4j相关配置类
```java
@Configuration
@EnableSwagger2
@EnableKnife4j
public class WebMvcConfig extends WebMvcConfigurationSupport {
    @Bean
    public Docket createRestApi() {
        // 文档类型
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.itheima.reggie.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("瑞吉外卖")
                .version("1.0")
                .description("瑞吉外卖接口文档")
                .build();
    }
}
```
* 设置静态资源，否则接口文档页面无法访问
```java
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
        registry.addResourceHandler("/backend/**").addResourceLocations("classpath:/static/backend/");
        registry.addResourceHandler("/front/**").addResourceLocations("classpath:/static/front/");
    }
```
* 在loginCheckFilter中设置不需要处理的请求路径

2. 常用注解
* @Api  用在请求的类上，例如Controller，表示对类的说明
* @ApiModel  用在类上，通常是实体类，表示一个返回响应数据的信息
* @ApiModelProperty  用在属性上，描述响应类的属性
* @ApiOperation  用在请求的方法上，说明方法的用途、作用
* @ApiImplicitParams  用在请求的方法上，表示一组参数说明
* @ApiImplicitParam  用在@ApiImplicitParams注解中，指定一个请求参数的各个方面