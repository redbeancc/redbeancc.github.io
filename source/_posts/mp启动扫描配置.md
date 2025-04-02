---
title: mp启动扫描配置
tags:
  - mybatisplus
  - spring
categories:
  - 问题
description: "\U0001F964 MybatisPlus 启动扫描"
abbrlink: d08f8977
date: 2022-04-08 11:07:09
cover:
---
# mp启动扫描配置
## MybatisPlusConfig
```java
@Configuration
@EnableTransactionManagement
@MapperScan("com.redbean.noveldata.mapper")
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        return paginationInterceptor;
    }
}
```