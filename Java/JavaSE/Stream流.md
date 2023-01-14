# Stream流
1. 什么是流？


2. 作用：结合lambda表达式，简化集合、数组的操作


3. 使用步骤：

（1）先得到一条Stream流，把数据放上去

* 单列集合：Collection中的默认方法 
```
list.stream.forEach(name->sout(name);
```

* 双列集合：需先转为单列集合

  (1)获取键：
```
  hm.keySet().stream().forEach();
```
  (2)获取键值对：
```  
hm.entrySet().stream().forEach();
```

* 数组：

 ```
 Arrays.strean(arr).forEach();
 ```

* 一堆零散数据：
```
  Stream.of(1,2,3,4,5).forEach();
  静态方法of：形参是一个可变参数，可以传递一堆零散数据，
也可以传递数组，但数据必须为引用数据类型。如果传递基本数据类型，
会把整个数组当做一个元素，放到Stream流中（输出的是地址）。
```

（2）中间方法、终结方法操作

* 中间方法
  * filter(Predicate <? super T> predicate): 过滤
  * limit(long maxSize): 获取前几个元素
  * skip(long n) :跳过前几个元素
  * distinct():元素去重（依赖hashCoded和equals方法）
  * concat(String a,String b): 合并a,b两个流为一个流
  * map(Function<T,R> mapper): 转换流中的数据类型 

         1. 中间方法，返回新的Stream流，原来的Stream流只能使用一次，建议链式编程。
         2.修改Stream流中的数据，不会影响原来集合或者数组中的数据
```
  * list.stream().filter(s->s.startWith("张)).forEach();
  * list.stream().filter(s->Integer.parseInt(s.split("-")[1])).forEach();

```

* 终结方法
  * forEach(): 遍历
  * count(): 统计
  * toArray(): 收集流中数据，放到数组中
  * collect(): 收集流中数据，放到集合中
    * collect(Collectors.toSet())   会去重
    * collect(Collectors.toList())   不会去重
    * collect(Collectors.toMap(keyMappeer,valueMapper))   键的规则，值的规则
```
  * list.stream().toArray(value->new String[value]);
  * list.stream().filter(s->"男".equals(s.split("-")[1])).collect(Collectors.toSet());
  * list.stream().filter(s->"男".equals(s.split("-")[1])).collect(Collectors.toList());
```
```java
        ArrayList<String> list2=new ArrayList<>();

        Collections.addAll(list2,"张无忌-男-15","周芷若-女-14","赵敏-女-13","张强-男-20",
        "张三丰-男-100","张翠山-男-40","张良-男-35","王二麻子-男-37","谢广坤-男-40");
// 方法一
        Map<String, Integer> newMap = list2.stream()
                .filter(s -> "男".equals(s.split("-")[1]))
                /**
                 * toMap：   参数一表示键的生成规则
                 *           参数二表示值的生成规则
                 *
                 *参数一：
                 *        Function泛型一： 表示流中每一个数据的类型
                 *                泛型二： 表示Map集合中键的数据类型
                 *        方法apply形参：依次表示流里面的每一个数据
                 *                方法体：生成键的代码
                 *                返回值：已经生成的键
                 *
                 * 参数二：
                 *        Function泛型一： 表示流中每一个数据的类型
                 *                泛型二： 表示Map集合中值的数据类型
                 *        方法apply形参：依次表示流里面的每一个数据
                 *                方法体：生成值的代码
                 *                返回值：已经生成的值
                 */
                .collect(Collectors.toMap(new Function<String, String>() {
                    @Override
                    public String apply(String s) {
                        /// 张无忌-男-15
                        return s.split("-")[0];
                    }
                }, new Function<String, Integer>() {
                    @Override
                    public Integer apply(String s) {
                        return Integer.parseInt(s.split("-")[2]);
                    }
                }));

        System.out.println(newMap);
// 方法二
 Map<String, Integer> newMap = list2.stream()
                .filter(s -> "男".equals(s.split("-")[1]))
                .collect(Collectors.toMap(
                        s -> s.split("-")[0]
                        ,
                        s -> Integer.parseInt(s.split("-")[2])
                ));
System.out.println(newMap);
```
 
