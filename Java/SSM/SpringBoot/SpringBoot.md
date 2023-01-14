# SpringBoot

简化Spring应用的初始搭建以及开发过程
## 1. 入门案例

SpringBoot程序快速启动  
1. 对SpringBoot项目打包（执行Maven构建指令package）
2. cmd 执行启动指令  ``java -jar xxxxxxxxx.jar``

### 起步依赖
### 引导类

SpringBoot程序启动：
* 启动方式
* SpringBoot在创建项目时，采用jar的打包方式
* SpringBoot的引导类是项目的入口，运行main方法就可以启动项目

### 切换web服务器：（tomcat 与 jetty）
> Jetty比Tomcat更轻量级，可拓展性更强（相较于Tomcat），谷歌应用引擎(GAE)已经全面切换为Jetty
```xml
<dependencies>
<!--    排除tomcat依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
<!--    加载jetty依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

## 2. 基础配置
### 1. 配置文件格式
SpringBoot提供了多种属性配置方式：(resources文件下)
* application.properties 
> sever.port=80

* application.yml (常用)
> sever:
> 
>        port: 81

* application.yaml
> sever:
> 
>        port: 82

配置文件加载顺序：
application.properties > application.yml > application.yaml

### 2. yaml格式
数据序列化格式
```yaml
lession: SpringBoot

sever:
  port: 80
enterprise:
    name: xiaoyi
    age: 20
    tel: 346244792
    
    # 数组格式
    subject:
      - Java
      - 前端
      - 大数据
```
优点：
* 容易阅读
* 容易与脚本语言进行交互
* 以数据为核心，重数据轻格式

YAML文件拓展名：
* .yml(主流)
* .yaml

语法规则：
* 大小写敏感
* 缩进表示层级关系
* 属性值前面添加空格
* #表示注释

### 3. yaml数据读取方式
```
经典白学：(直接读取)
    @Value("${lesson}")
    private String lesson;
    @Value("${sever.port}")
    private Integer port;
    @Value("${enterprise.subject[0]}")
    private String subject_00;
    
又是白学：（封装后读取）
    @Autowired
    private Environment environment;
    
    sout(environment.getProperty("lesson"));
    sout(environment.getProperty("sever.port"));
    sout(environment.getProperty("enterprise.subject[0]"));

这会儿没白学：（封装后读取）
    1. 定义一个yaml文件中与enterprise具有相同属性的POJO类
    2. 类上面加 @Compoent 注解
    3. 再加一个 @ConfigurationProperties(prefix = "enterprise") 的注解
    4. controller中
            @Autowired
            private Enterprise enterprise;
```

自定义对象封装数据警告解决方案：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

### 4. 多环境开发配置
yaml文件格式：
```yml
# 设置启用的环境
spring:
  profiles:
    active: dev
---
# 开发
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 80
---
# 生产
spring:
  profiles: pro
server:
  port: 81
---
# 测试
spring:
  profiles: test
server:
  port: 82
```
### 5. 多环境命令行启动参数设置
打成jar包, cmd运行：
> java -jar xxxxxxx.jar --spring.profiles.active=test

> java -jar xxxxxxx.jar --server.port=88

### 6. 多环境开发控制
Maven控版本，SpringBoot加载Maven版本

Maven与SpringBoot多环境兼容
1. Maven中设置多环境属性
```xml
<profiles>
<!--    开发环境-->
    <profile>
        <id>dev_env</id>
        <properties>
            <profile.active>dev</profile.active>
        </properties>
<!--        设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile> 

<!--    生产环境-->
    <profile>
        <id>pro_env</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile> 

<!--    测试环境-->
    <profile>
        <id>env_test</id>
        <properties>
            <profile.active>test</profile.active>
        </properties>
    </profile>
</profiles>
```

2. SpringBoot中引用Maven属性
```yml
# 设置启用的环境
spring:
  profiles:
    active: ${profile.active}
---
# 开发
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 80
---
# 生产
spring:
  profiles: pro
server:
  port: 81
---
# 测试
spring:
  profiles: test
server:
  port: 82
```
3. 执行Maven打包指令（第一次打包失败）

``${profile.active}这个信息没有被解析到``

对资源文件开启对默认占位符的解析：
```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-resources-plugin</artifactId>
            <configuration>
                <encoding>UTF-8</encoding>
                <useDefaultDelimiters>true</useDefaultDelimiters>
            </configuration>
        </plugin>
    </plugins>
</build>
```
### 7. 配置文件分类
SpringBoot中4级配置文件：
* 1级：file: config/application.yml    【最高】
* 2级：file: application.yml
* 3级：classpath: config/application.yml
* 4级：classpath: application.yml

## 3. 整合第三方技术

### 1. SpringBoot整合junit

```java
//@SpringBootTest
@SpringBootTest(classes = SpringBootApplication.class)
class SpringBootJunitApplicationTests {
    @Autowired
    private BookService bookService;

    @Test
    public void testSave() {
        bookService.save();
    }
}
```
### 2. SpringBoot整合SSM(实际在整合Mybatis，Spring、SpringMVC不用整合)

当前模块需要的技术集： ``Mybatis、MySQL``

设置数据源参数：
```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8
    username: root
    password: yyl021104
    type: com.alibaba.druid.pool.DruidDataSource
```

定义数据层接口与映射配置：
```java
@Mapper
public interface BookDao {

    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```
