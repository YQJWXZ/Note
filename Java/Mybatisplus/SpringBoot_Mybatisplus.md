# Mybatisplus
基于Mybatis框架基础上开发的增强型工具，旨在简化开发、提高效率
## 1. 入门案例

手工添加mp起步依赖
```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```

定义数据接口，继承**BaseMapper<User>**
```java
@Mapper
public interface UserDao extends BaseMapper<User> {
    
}
```

## 2. Mybatisplus

特性：
* 无侵入
* 强大的CRUD操作
* 支持Lambda
* 支持主键自动生成
* 内置分页插件
* ···

### 2.1 标准数据层开发

标准CRUD操作：
```java
@SpringBootTest
class SpringBootMybatisplusDemoApplicationTests {
    @Autowired
    private UserDao userDao;
    
    @Test
    void testSave(){
        User user = new User();
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");

        userDao.insert(user);
    }
    
    @Test
    void testDelete(){
        userDao.deleteById(6);
    }
    
    @Test
    void testUpdate(){
        User user = new User();
        user.setId(1L);
        user.setName("Tom888");
        user.setPassword("tom888");
        userDao.updateById(user);
    }
    
    @Test
    void testGetById(){
        User user = userDao.selectById(2L);
        System.out.println(user);
    }

    @Test
    void testGetAll() {
        List<User> userList = userDao.selectList(null);
        System.out.println(userList);
    }

}
```

**lombok**
一个Java类库，提供了一组注解，简化POJO实体类开发
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

### 2.2 分页数据查询
> 分页查询  IPage<T> selectPage(Ipage<T> page)

1. 设置分页拦截器作为Spring管理的bean
```java
@Configuration
public class MybatisplusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        // 1. 定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor=new MybatisPlusInterceptor();
        // 2. 添加具体的拦截器
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor;
    }
}
```
2. 执行分页查询
```java
    @Test
    void testGetByPage(){
        IPage page = new Page(1,3);
        userDao.selectPage(page,null);
        System.out.println("当前页码："+page.getCurrent());
        System.out.println("每页显示数："+page.getSize());
        System.out.println("一共多少页："+page.getPages());
        System.out.println("一共多少条数据："+page.getTotal());
        System.out.println("数据："+page.getRecords());
    }
```

开启日志：
```yml
# TODO 开启mp的日志（输出到控制台）
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

```

### 2.3 DQL编程控制

1. 条件查询的三种方式
* lt 小于
* gt 大于
* le 小于等于
* ge 大于等于
* eq 等于

```java
@Test
void testGet1() {
/*        // 方式一：按条件查询
        QueryWrapper qw = new QueryWrapper();
        qw.lt("age",18);  // 小于
        // qw.gt("age",18);   // 大于
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);*/

/*        // 方式二： Lambda格式按条件查询
        QueryWrapper<User> qw = new QueryWrapper<>();
        qw.lambda().lt(User::getAge,18);
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);*/

        // 方式三： Lambda表达式按条件查询(接口变了)
        LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();
/*        // 年龄在10~30以内
        lqw.lt(User::getAge,30).gt(User::getAge,10);*/
        // 年龄小于10或者大于30
        lqw.lt(User::getAge,10).or().gt(User::getAge,30);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

2. 条件查询-null值处理
```java
@Test
void testGet2(){
        // null判定

        // 模拟页面传递过来的查询数据
        UserQuery uq=new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        
        LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();

        // 先判定第一个参数是否为true，如果为true连接当前条件
        lqw.lt(null!=uq.getAge2(),User::getAge,uq.getAge2())
           .gt(null!=uq.getAge(),User::getAge,uq.getAge());
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

3. 查询投影
```java
    @Test
    void testGet3(){
        // 查询投影

        // 模拟页面传递过来的查询数据
        UserQuery uq=new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        
        // 查询结果包含模型类中部分属性
//        LambdaQueryWrapper<User> lqw=new LambdaQueryWrapper<>();
//        lqw.select(User::getId,User::getName,User::getAge);
//        List<User> userList = userDao.selectList(lqw);
            
            
        // 查询结果包含模型类中未定义的属性    
//        QueryWrapper<User> qw=new QueryWrapper<>();
//        qw.select("id","name","age");
//        List<User> userList = userDao.selectList(qw);
//        System.out.println(userList);

            
        QueryWrapper<User> qw=new QueryWrapper<>();
        qw.select("count(*) as count");
        // 分组查询
        qw.groupBy("tel");
        List<Map<String, Object>> userList = userDao.selectMaps(qw);
        System.out.println(userList);

    }
```

4. 查询条件设置
* 范围匹配(> 、= 、between)
* 模糊匹配(like)
* 空判定(null)
* 包含性匹配(in)
* 分组(group)
* 排序(order)
* ···

5. 映射匹配兼容性

字段映射与表明映射
* 表字段与编码属性设计不同步  ``@TableField(value = "pwd")``
* 编码中添加了数据库中未定义的属性
```
@TableField(exist = false)
private Integer online;
```
* 隐藏字段查看权限   ``@TableField(value = "pwd",select = false)``
* 表名与编码开发设计不同步  ``1.@TableName("tbl_user")  2. 直接在yml文件中全局设置 table-prefix: tbl_``
```java
@Data  // 但是不包含构造方法@NoArgsConstructor、@AllArgsConstructor这两个注解
@TableName("tbl_user")  
public class User {
    private long id;
    private String name;
    @TableField(value = "pwd",select = false)
    private String password;
    private Integer age;   //  age下限
    private String tel;

    @TableField(exist = false)
    private Integer online;
}
```

### 2.4 DML编程控制

1. Insert

id生成策略控制：
* AUTO(0): 使用数据库id自增策略控制id生成
* NONE(1)：不设置id生成策略
* INPUT(2): 用户手工输入id
* ASSIGN_ID(3)：雪花算法生成id(可见荣放数值型与字符串型)
* ASSIGN_UUID(4)：以UUID生成算法作为id生成策略

#### 全局设置

2. Delete

多纪录操作：
```java
@Test
void testDelete2(){
        ArrayList<Long> list = new ArrayList<>();
        list.add(555L);
        list.add(556L);
        list.add(557L);
        list.add(558L);
        userDao.deleteBatchIds(list);
    }
```

逻辑删除：
为数据设置是否可用状态字段，删除时啥遏制状态字段为不可用状态，数据保留在数据库中
* 数据库表中添加逻辑删除标记字段
* 实体类中添加对应字段，并设定当前字段为逻辑删除标记字段
```java
    // 逻辑删除字段，标记当前纪录是否被删除
//    @TableLogic(value = "0",delval = "1")
    private Integer deleted;
```
* 配置逻辑删除字面值
```yml
# TODO 设置逻辑删除字段
      logic-delete-field: deleted
      logic-delete-value: 1
      logic-not-delete-value: 0
```

3. Update

乐观锁(并发问题)：
* 数据库表中添加锁标记字段
* 实体类中添加对应字段，并设定当前字段为逻辑删除标记字段
```java
@Version
private Integer version;
```
* 配置乐观锁拦截器实现锁机制对应的动态SQL语句拼装
```java
        // 3. 添加乐观锁的拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
```

### 2.5 代码生成器
```xml
<!--代码生成器-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>

<!--velocity模板引擎-->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

创建代码生成器对象，执行生成代码操作
```java
AutoGenerator autoGenerator = new AutoGenerator();
autoGenerator.excute();
```
``这些都到源码中去查看``

数据源相关配置 
设置全局配置 
包相关配置
策略配置

