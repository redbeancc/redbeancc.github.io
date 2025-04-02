---
title: SwaggerConfig配置类
tags:
  - swagger
  - spring
categories:
  - 问题
description: "\U0001F96B 开启 Swagger 的配置"
abbrlink: c81b37f
date: 2022-04-08 11:09:02
cover:
---
### SwaggerConfig配置类
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any()).build();
    }


    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("熹烨网络小说分析系统管理API")
                .description("熹烨网络小说分析系统的API文档.")
                .version("1.0")
                .build();
    }
}
```

### 

## 然后访问localhost:9090/swagger-ui.html