---
title: spring boot 集成 redis
tags:
  - spring
  - redis
categories:
  - 教程
description: "\U0001F9C5 在 spring boot 项目中使用 redis"
abbrlink: 126591a7
date: 2022-04-08 11:15:03
cover:
---
# spring boot 集成redis
## pom文件中加入依赖
```xml
<!-- redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
## 在application.yml里添加redis相关配置
```yml
redis:
    host: 127.0.0.1
    port: 6379
```
## 引用
### 在需要引用的业务代码中，比如service层引入
****
**StringRedisTemplate是专门的string类型，如果有别的类型可以用RedisTemplate**
**因为json数据一般都是string，所以这里就用了StringRedisTemplate**
```java
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
```

### StringRedisTemplate中所有的get，set等操作都在ops中
```java
String KEY = "USER_FIND_ALL";
String userFindAllJsonStr = stringRedisTemplate.opsForValue().get(KEY);

List<SysUser> all = sysUserMapper.findAll();
stringRedisTemplate.opsForValue().set(KEY, JSONUtil.toJsonStr(all));

// 如果要删除的话，delete是在stringRedisTemplate下
stringRedisTemplate.delete(KEY);

// 设置过期时间
stringRedisTemplate.opsForValue().set(KEY, JSONUtil.toJsonStr(all),60*5, TimeUnit.SECONDS);
/*
SECONDS 单位：秒
天，小时，分钟在java.util.concurrent.TimeUnit包中都有
这里设置的是过期时间为：5分钟
*/
```

### 用hutool工具类，可以将我们的string类型转换为泛类型，也可以将泛类型转换为字符串类型
```java
// 泛类型转换为字符串
JSONUtil.toJsonStr(new User());
// 字符串转换为泛类型
List<SysUser> all = JSONUtil.toBean(userFindAllJsonStr, new TypeReference<List<SysUser>>() {},true);
// JSONUtil.toBean(字符串, new TypeReference<类型>() {},是否忽略错误);
```
{% note warning flat%}
！！记得new TypeReference<类型>()后面加上{}
{% endnote %}