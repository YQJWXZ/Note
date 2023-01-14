# Spring事务
Spring事务作用：在数据层或业务层保障一系列的数据库操作同成功同失败
```java
public interface PlatformTransactionManager{
    void commit(TransactionStatus status) throws TransactionException;
    void rollback(TransactionStatus status) throws TransactionException;
}
```
## 1. Spring事务管理
1. 在业务层接口上添加Spring事务管理

```java
// Spring注解事务通常添加在业务层接口中而不会添加到业务层实现类中，降低耦合
// 注解式事务可以添加到业务方法上表示当前方法开启事务，也可以添加到接口上表示当前接口所有方法开启事务
public interface AccountService{
    @Transactional
    public void transfer(String out, String in, Double money);
}
```

2. 设置事务管理器

```java
import org.springframework.context.annotation.Bean;

@Bean
public PlatformTransactionManager transactionManager(DataSource dataSource){
    DataSourceTransactionManager ptm = new DataSourceTransactionManager();
    ptm.setDataSource(dataSource);
    return ptm;
        }
```

3. 开启注解式事务驱动
> @EnableTransactionManagement(在SpringConfig方法上)

## 2. Spring事务角色
1. 事务管理员：发起事务方，在Spring中通常指代业务层开启事务的方法
2. 事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法

## 3. Spring事务属性
1. 事务相关配置
* readOnly 设置是否为只读事务
* timeout 设置事务超时时间（=-1 永不超时）
* rollbackFor 设置事务回滚异常  ``rollbackFor = {IOException.class}``
* propagation 设置事务传播行为


### 转账业务追加日志，数据库操作进行留痕
实现效果预期：
    无论转账是否操作成功，均进行转账操作的日志留痕
存在的问题：
    日志的纪录与转账操作隶属同一个事务，同成功同失败 ``try{}finally{}``
解决：
    事务传播行为  ``@Transactional(propagation = Propagation.REQUIRES_NEW)``
