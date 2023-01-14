# AOP
面向切面编程

作用： 在不惊动原始设计的基础上为其进行功能增强
Spring理念：无入侵式/无侵入式

## 1. 连接点、切入点、通知、切面、通知类
* 连接点：程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等
* 切入点：匹配连接点的式子
* 通知：在切入点处执行的操作，也就是共性功能
* 通知类：定义通知的类
* 切面：描述通知与切入点的对应关系

## 2. AOP入门（注解版）
1. 导入aop坐标  ``spring-context  aspectjweaver``
2. 定义接口与实现类
3. 定义通知类，制作通知
4. 定义切入点
5. 绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行位置
6. 定义通知类受Spring容器管理，并定义当前类为切面类

```java
import org.springframework.stereotype.Component;

@Component
@Aspect
// 切入点定义依托一个不具有实际意义的方法进行，即无参数、无返回值，方法体无实际逻辑
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt() {
    }

    @Before("pt()")
    public void before() {
        System.out.println(System.currentTimeMillis());
    }
}
```

7. 开启Spring对AOP注解驱动支持

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@ComponentScan("com.itheima")
@EnableAspectJAutoProxy
public class SpringConfig{
    
}
```
## 3. AOP工作流程（SpringAOP本质：代理模式）
1. Spring容器启动
2. 读取切面配置中的切入点
3. 初始化bean，判定bean对应的类中的方法是否匹配到任意切入点
    * 匹配失败，创建对象
    * 匹配成功，创建原始对象（目标对象）的代理对象
4. 获取bean执行方法
    * 获取bean，调用方法并执行，完成操作
    * 获取的bean是代理对象时，根据代理对象的运行模式运行原始方法与增强的内容，完成操作

AOP核心概念：
* 目标对象：原始功能去掉共性功能对应的类产生的对象，这种对象是无法完成最终工作的
* 代理：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

## 4. AOP切入点表达式
描述实现类或者接口

标准格式：
> 动作关键词（访问修饰符 返回值 包名.类/接口名.方法名（参数）异常名）

通配符：
```
* : 单个独立的任意符号
.. : 多个连续的任意符号，用于简化包名与参数的书写
```

书写技巧：
* 所有代码按照标准规范开发
* 描述切入点通常描述接口
* 返回值类型对于增删改类使用精准类型加速匹配，对于查询类使用*通配快速描述
* 接口名/类名书写名称与模块相关的采用*匹配，如UserService --> *Service
* 方法名书写以动词进行精准匹配，如getById --> getBY*
* 通常不使用异常作为匹配规则

## 5. AOP通知类型
1. 前置通知  ``@Before("pt()")``
2. 后置通知  ``@After("pt()")``
3. 环绕通知  ``@Around("pt()")``  （常用）

```java
@Around("pt2()")
public Object around(ProceedingJoinPoint pjp) throws Throwable{
    System.out.println("around before advice ...");
    // 表示对原始操作的调用
        Integer ret = (Integer) pjp.proceed();
        System.out.println("around after advice ...");
        return ret;
        }
```
4. 返回后通知  ``@AfterReturning("pt2()")``
5. 抛出异常后通知  ``@AfterThrowing("pt2()")``

## 6. 测量业务层接口万次执行效率

```java
@Around("ProjectAdvice.servicePt()")
public void runSpeed(ProceedingJoinPoint pjp) throws Throwable{
        // 获取执行签名信息
        Signature signature = pjp.getSignature();
        // 通过签名获取执行类型（接口名）
        String className = signature.getDeclaringTypeNmae();
        // 通过签名 获取执行操作名称（方法名）
        String methodName = signature.getName();
        
        long start = System.currentTimeMillis();
        for(int i=0;i<10000;i++){
            pjp.proceed();
        }
        long end = System.currentTimeMillis();
        System.out.println("万次执行："+className+"."+methodName+"--->"+(end-start)+"ms");
        }
```

## 7. AOP通知获取数据
1. 获取参数
> @Before   参数为JoinPoint jp
> @Around   参数为ProceedingJoinPoint pjp
2. 获取返回值
> @AfterReeturning(value = "pt()", returning = "ret")  参数为Object ret
> @Around
3. 获取异常

## 8. 百度网盘密码数据兼容处理
 空格处理（trim方法）