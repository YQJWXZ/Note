# Spring Cache

Spring Cache: 基于注解的缓存功能，提供了一层抽象，底层可以切换不同的cache实现。具体就是通过CacheManager接口来统一不同的缓存技术

CacheManager:
* EhCacheCacheManager  使用EhCache作为缓存技术
* GuavaCacheManager  使用Google的GuavaCache作为缓存技术
* RedisCacheManager  使用Redis作为缓存技术

1. 常用注解
* @EnableCaching  开启缓存注解功能
* @Cacheable  在方法执行前spring先查看缓存中是否有数据，有数据直接返回，，没有数据调用方法并将方法返回值放到缓存中
> @Cacheable(value = "userCache", key = "#id", unless = "#result == null")
* @CachePut  将方法的返回值放到缓存中
> @CachePut(value = "userCache", key = "#user.id")
* @CacheEvict  将一条或多条数据从缓存中删除
* > @CacheEvict(value = "userCache", key = "#id")  #root.args[0] #p0

2. Spring Cache使用Redis缓存技术
* 导坐标 ``spring-boot-starter-data-redis、spring-boot-starter-cache``
* 配置application.yml
```yml
spring:
  cache:
    redis:
      time-to-live: 1800000 #设置缓存有效期
```
