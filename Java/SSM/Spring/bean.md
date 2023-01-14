# bean

## 1. bean配置
1. bean基础配置
2. bean别名配置
>     <bean id="bookService" name="service service2 bookEbi" class="impl.service.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookServiceImpl">
3. bean作用范围配置
>  单例设计模式：<bean id="bookDao" name="dao" class="impl.dao.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookDaoImpl" **scope="singleton"**/>

>  多例设计模式<bean id="bookDao" name="dao" class="impl.dao.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookDaoImpl" **scope="prototype"**/>

### 1.为什么bean默认为单例？
### 2. 适合交给容器进行管理的bean
1. 表现层对象
2. 业务层对象
3. 数据层对象
4. 工具对象

### 3. 不适合交给容器进行管理的bean
* 封装实体的域对象

## 2. bean实例化

### 实例化bean的三种方法：
1. 构造方法（常用）

``
bean本质上就是对象，创建bean使用**无参构造方法**完成
``
2. 静态工厂
>     <bean id="orderDao" class="com.xiaoyi.springDemo.IOCAndDIDemo.factory.OrderFactory" factory-method="getOrderDao"/>
3. 实例工厂
>     <bean id="userFactory" class="com.xiaoyi.springDemo.IOCAndDIDemo.factory.UserFactory"/>
>     <bean id="userDao" factory-bean="userFactory" factory-method="getUserDao"/>

4. 针对实例工厂做的一些改良（FactoryBean）：
>     <bean id="userDao" class="com.xiaoyi.springDemo.IOCAndDIDemo.factory.UserDaoFactoryBean"/>
```java
import org.springframework.beans.factory.FactoryBean;

public class UserDaoFactoryBean implements FactoryBean<userDao>{
    // 代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception{
        return new UserDaoImpl;
    }
    
    public Class<?> getObjectType(){
        return UserDao.class;
    }
    
    public boolean isSingleton(){
        return true;   // true: 单例   false: 多例
    }
}
```

## 3. bean的生命周期
bean生命周期控制：在bean创建后销毁前做一些事情
> 定义两个方法init,destory表示bean初始化对应的操作以及bean销毁前对应的操作

>     <bean id="bookService" class="impl.service.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookServiceImpl" init-method:"init" destory-method="destory"/>

**要想看到init,destory方法的执行必须要关闭容器**
1. ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
    ctx.close();   ``Application没有提供close方法``
2. 设置关闭钩子，在虚拟机退出前先关闭容器再退出虚拟机  ``ctx.registerShutdownHook();``
3. 使用接口的形式``implents InitializingBean, DisposableBean  然后重写afterPropertiesSet和destory方法``

bean生命周期：
* 初始化容器
  1. 创建对象（内存分配）
  2. 执行构造方法
  3. 执行属性注入（set操作）
  4. 执行bean初始化方法
* 使用bean
  1. 执行业务操作
* 关闭/销毁容器
  1. 执行bean销毁方法