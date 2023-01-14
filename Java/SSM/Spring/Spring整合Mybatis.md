# Spring整合Mybatis

核心配置
```java
import org.springframework.context.annotation.Bean;

@Bean
public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
    SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
    ssfb.setTypeAliasesPackage("com.itheima.domain");
    ssfb.setDataSource(dataSource);
    return ssfb;
        }
```

mapper配置

```java
import org.springframework.context.annotation.Bean;

@Bean
public MapperScannerConfiguer mapperScannerConfigurer(){
    MapperScannerConfigurer msc = new MapperScannerConfigurer();
    msc.setBasePackage("com.itheima.dao");
    return msc;
        }
```