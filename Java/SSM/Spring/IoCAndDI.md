# 核心概念
1. IoC/DI
2. IoC容器
3. Bean

代码书写现状：耦合度偏高
解决：使用对象时，在程序中不要主动使用new产生对象，转换为由外部提供对象

目标： 充分解耦
* 使用IoC容器管理bean
* 在IoC容器内将有依赖关系的bean进行关系绑定

## 1. IoC(控制反转)
对象的创建控制权由程序转移到外部(IoC容器,用来充当IoC思想中的"外部")
IoC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为Bean

1. 管理什么？
2. 如何将被管理的对象告知IoC容器？（配置）
3. 被管理的对象交给IoC容器，如何获取到IoC容器？ （接口）
4. IoC容器得到后，如何从容器中获取bean？ （接口方法） 

步骤：
1. 导坐标
2. 定义Spring管理的类 （接口）
```java
public interface BookService {
    public void save();
}
```
```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao=new BookDaoImpl();

    public void save(){
        System.out.println("book service save ···");
    }
}
```
3. 创建Spring配置文件，配置对应类作为Spring管理的bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--    配置bean-->

    <bean id="bookDao" class="impl.dao.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookDaoImpl"/>
    <bean id="bookService" class="impl.service.com.xiaoyi.SpringDemo.IoCAndDIDemo.BookServiceImpl"/>
</beans>
```
4. 初始化IoC容器(Spring核心容器/Spring容器)，通过容器获取bean
```java
public class APP2 {
    public static void main(String[] args) {

        // 获取IoC容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("applicationContext.xml");

        // 获取bean
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();

        System.out.println("---------");

        BookService bookService = (BookService)ctx.getBean("bookService");
        bookService.save();
    }
}
```
## 2. DI(依赖注入)
在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

1. 基于IoC管理bean
2. Service中使用new形式创建的Dao对象是否保留？ （否）
3. Service中需要的Dao对象如何进入到Service中？ （提供方法）
4. Service与Dao间的关系如何描述？ （配置）

