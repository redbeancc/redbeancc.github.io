---
title: swagger3 引入 springboot 3
tags:
  - swagger
  - spring
categories:
  - 教程
abbrlink: '18407320'
date: 2023-11-10 15:51:36
description:
cover:
---
# swagger3 引入 springboot 3
## 依赖
```xml
        <dependency>
            <groupId>io.swagger.core.v3</groupId>
            <artifactId>swagger-annotations</artifactId>
            <version>2.2.15</version>
        </dependency>
        <dependency>
            <groupId>org.springdoc</groupId>
            <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
            <version>2.1.0</version>
        </dependency>
```

## 配置类
```java
@Configuration
//@EnableSwagger2
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
                .title("在线商店管理API")
                .description("在线商店系统的API文档.")
                .version("1.0")
                .build();
    }
}

```

## 注释修改
```java
@Api(tags = "")     
改为     
@Tag(name = "")
 
@ApiModel(value="", description="")     
改为    
@Schema(name="", description="")
 
@ApiModelProperty(value = "", required = true)     
改为    
@Schema(name= "", description = "", required = true)
 
@ApiOperation(value = "", notes = "")    
改为    
@Operation(summary = "", description = "")
 
@ApiParam    改为     @Parameter
 
@ApiResponse(code = 404, message = "")  
改为
@ApiResponse(responseCode = "404", description = "")
```
## 地址
```
localhost:8088/swagger-ui/index.html
```