# Maven
## 1. 分模块开发与设计
1. 创建Maven模块
2. 书写模块代码
3. 通过Maven指令安装模块到本地仓库中（install指令）

> 团队内部开发需要发布模块功能到团队内部可共享的仓库中（私服） 


## 2. 依赖管理
1. 依赖传递
* 直接依赖
* 间接依赖

依赖传递冲突问题：
* 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
* 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
* 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

2. 可选依赖
可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性（不透明）
> <optional>true</optional>
3. 排除依赖
排除依赖是隐藏当前资源对应的依赖关系（不需要）
```xml
    <exclusions>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
    </exclusions>
```

## 3. 聚合与继承
1. 聚合
聚合：将多个模块组织成一个整体，同时进行项目构建的过程称为聚合
聚合工程：通常是一个不具有业务功能的“空”工程（有且仅有一个pom文件）
作用：使用聚合工程可以 将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建（某个模块更新时，与已更新模块关联的其他模块同步更新）

聚合工程开发：
* 创建Maven模块，设置打包类型为pom  `` <packaging>pom</packaging> ``
* 设置当前聚合工程所包含的子模块名称
```xml
<modules>
    <module>../maven_ssm</module>
    <module>../maven_pojo</module>
    <module>../maven_dao</module>
</modules>
```

2. 继承
描述的是两个工程之间的关系，与Java中的继承类似，子工程可以继承父工程中的配置信息，常见于依赖关系的继承

作用：
* 简化配置
* 减少版本冲突

继承关系：
* 创建Maven模块，设置打包类型为pom
> <packaging>pom</packaging>

* 在父工程中的pom文件中配置依赖关系（子工程将沿用父工程中的依赖关系）

* 父工程中配置子工程中可选的依赖关系

```xml
<!--  定义依赖管理-->
<dependencyManagement>
    <dependencies>
        <dependeency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependeency>
    </dependencies>
        </dependencyManagement>
```
* 子工程中配置当前工程所继承的工程

> 子工程使用父工程中的可选依赖时，仅需要提供群组id和项目id，无需提供版本，版本由父工程统一提供，避免版本冲突
>
> 子工程还可以定义父工程中没有定义的依赖关系

```xml
<!--定义该工程的父工程-->
<parent>
    <groupId>com.xiaoyi</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-SNAPSHOT</version>
<!--    填写父工程中的pom文件-->
    <realtivePath>../maven_ 01_parent/pom/xml</realtivePath>
</parent>
```
### 聚合与继承的区别
1. 作用：
* 聚合用于快速构建项目
* 继承用于快速配置

2. 相同点：
* 聚合与继承的pom.xml文件打包方式均为pom，可以将两种关系制作到同一个pom文件中
* 聚合与继承均属于设计型模块，并无实际的模块内容

3. 不同点：
* 聚合在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些
* 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己

## 4. 属性管理
属性配置与使用
1. 定义属性
```xml
<propertie>
    <spring.version>5.2.10.RELEASE</spring.version>
    <junit.version>4.12</junit.version>
</propertie>
```
2. 引用属性
```xml
<dependcy>
    xxxxxxxxx
    <version>${spring.version}</version>
</dependcy>
```

配置文件加载属性：
1. 定义属性
```xml
<propertie>
    <spring.version>5.2.10.RELEASE</spring.version>
    <junit.version>4.12</junit.version>
    <jdbc.url>jdbc:mysql://127.0.0.1:3306/ssm_db?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8</jdbc.url>
</propertie>
```

2. 配置文件中引用属性
```
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=yyl021104
```

3. 开启资源文件目录加载属性的过滤器
```xml
<build>
    <resources>
        <resource>
            <directory>${project.basedir}/src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

4. 配置maven打war包时，忽略web.xml检查

属性：
* 自定义属性（常用）  ``${spring.version}``
* 内置属性  ``${basedir}  ${version}``
* Setting属性  ``${settings.localRepository}``
* Java系统属性  ``${user.home}``
* 环境变量属性   ``${env.JAVA_HOME}``

版本管理：
* 工程版本
  1. SNAPSHOT(快照版本)
  2. RELEASE(发布版本)
* 发布版本
  1. alpha版
  2. beta版
  3. 纯数字版

## 5. 多环境配置与应用
1. 多环境开发
* 定义多环境
```xml
<profiles>
<!--    开发环境-->
    <profile>
        <id>env_dep</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.0.0.1:3306/ssm_db</jdbc.url>
        </properties>
<!--        设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile> 
    
<!--    生产环境-->
    <profile>
        <id>env_pro</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
        </properties>
    </profile> 
    
<!--    测试环境-->
    <profile>
        <id>env_test</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.2.2.1:3306/ssm_db</jdbc.url>
        </properties>
    </profile>
    
</profiles>
```

* 使用多环境
> mvn install -P pro_env

2. 跳过测试

> mvn指令： mvn install -D skipTests

``执行的项目构建指令必须包含测试生命周期，否则无效果。例如执行compile生命周期，不经过test生命周期``

细粒度控制跳过测试：
```xml
<build>
    <resources>
        <resource>
            <directory>${project.basedir}/src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.12.4</version>
            <configuration>
                <skipTests>false</skipTests> <!--设置跳过测试-->
<!--                排除掉不参与测试的内容-->
                <excludes>
                    <exclude>**/BooksServiceTest.java</exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```
## 6. 私服
1. 私服
Nexus
> nexus.exe/run nexus
> 
> localhost:8081
2. 私服仓库分类
* 宿主仓库（hosted）: 保存自主研发+第三方资源
* 代理仓库（proxy）: 代理连接中央仓库
* 仓库组（group）: 为仓库编组简化下载操作

### 本地仓库访问私服权限设置

### 工程上传到私服服务器位置

pom配置当前工程保存在私服中的具体位置
```xml
<distributionManagement>
  <repository>
    <id>xiaoyi-release</id>
    <url>http://localhost:8081/repository/xiaoyi-release/</url>
  </repository>
  
  <snapshotRepository>
    <id>xiaoyi-snapshot</id>
    <url>http://localhost:8081/repository/xiaoyi-snapshot/</url>
  </snapshotRepository>
</distributionManagement>
```
> 发布命令 mvn deploy