---
title: springBoot 集成 hutool邮箱发送功能
tags:
  - spring
  - hutool
categories:
  - 教程
description: "\U0001F452 springBoot 集成 hutool邮箱发送功能"
abbrlink: 94e8ad0d
date: 2022-04-26 13:28:26
cover:
---
# springBoot 集成 hutool邮箱发送功能
pom文件导入依赖
```xml
<!--- 邮箱 -->
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.6.2</version>
</dependency>
```

在resource文件夹下创建config目录，config目录下创建mail.setting文件<br/>
文件如下
```
from = 熹烨小说分析系统<x.xxxx@qq.com>
pass = #####邮件服务器密码
sslEnable = true
```
#### 在service中使用
```java
public Result send(SendEmailVo sendEmailVo) {
    String publickey = sendEmailVo.getKey();
    String privatekey = stringRedisTemplate.opsForValue().get(publickey);
    if (StrUtil.isBlank(privatekey)) {
        return Result.error("406", "注册时间过长，请刷新页面");
    }
    String toemail = Sm2.doDecrypt(sendEmailVo.getEmail(), privatekey);
    stringRedisTemplate.delete(publickey);

    String rellycode = RandomUtil.randomString(6);
    MailUtil.send(toemail,"邮箱验证" , "您的验证码是：<h1>" + rellycode + "</h1>，有效时间为 <h3>5分钟</h3>", true);
    stringRedisTemplate.opsForValue().set(toemail, rellycode,5,TimeUnit.MINUTES);
    return Result.success("发送邮件成功，请进入邮箱查看",null);
}
```