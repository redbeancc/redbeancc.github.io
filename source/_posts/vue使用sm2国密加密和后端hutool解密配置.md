---
title: vue使用sm2国密加密和后端hutool解密配置
tags:
  - vue
  - sm2
  - hutool
categories:
  - 教程
description: "\U0001F353 非对称算法，公钥加密，私钥解密，用 redis 存储公钥"
abbrlink: 7f7fec03
date: 2022-04-21 13:20:27
cover:
---
# vue使用sm2国密加密和后端hutool解密配置
后端pom文件引入相关依赖
```xml
<!-- sm2国密 -->
<dependency>  
    <groupId>org.bouncycastle</groupId>
    <artifactId>bcprov-jdk15to18</artifactId>
    <version>1.69</version>
</dependency>
```
结合hutool工具类，获取公钥，在service层
```java
    /**
     * 获取公钥q
     */
    private String privatekey;
    @Override
    public String getSM2() {
        SM2 sm2 = SmUtil.sm2(); // 创建密钥
        // sm2的加解密时有两种方式即 C1C2C3前端的就是0、 C1C3C2前端的就是1，
        sm2.setMode(SM2Engine.Mode.C1C3C2);
        // 生成私钥
        this.privatekey = HexUtil.encodeHexStr(sm2.getPrivateKey().getEncoded());
        // 生成公钥
        String publickey = HexUtil.encodeHexStr(sm2.getPublicKey().getEncoded());
        return publickey;
    }
```
```java
  /**
     * 获取公钥q
     */
    @Override
    public Result getSM2() {
        String publickey = "PUBLICKEY";
        String privatekey = "PRIVATEKEY";
        String prik = stringRedisTemplate.opsForValue().get(privatekey);
        String pubk = stringRedisTemplate.opsForValue().get(publickey);
        if (StrUtil.isBlank(prik) || StrUtil.isBlank(pubk)) {
            stringRedisTemplate.delete(publickey);
            stringRedisTemplate.delete(privatekey);
            SM2 sm2 = SmUtil.sm2(); // 创建密钥
            // sm2的加解密时有两种方式即 C1C2C3前端的就是0、 C1C3C2前端的就是1，
            sm2.setMode(SM2Engine.Mode.C1C3C2);
            // 生成私钥
            String privakey = HexUtil.encodeHexStr(sm2.getPrivateKey().getEncoded());
            // 生成公钥
            String publkey = HexUtil.encodeHexStr(sm2.getPublicKey().getEncoded());
            stringRedisTemplate.opsForValue().set(publickey, publkey);
            stringRedisTemplate.opsForValue().set(privatekey, privakey);
            prik = stringRedisTemplate.opsForValue().get(privatekey);
            pubk = stringRedisTemplate.opsForValue().get(publickey);
        }
        return Result.success(pubk);
    }
```
前端vue下载sm-crypto
```
npm install sm-crypto --save
```
代码
```javascript
    request.get("/user/sm").then((res) => {
        this.pubk = res.data
        console.log(this.pubk)
    })
```
```javascript
    this.user.email = this.sm2Encrypt(this.user.email,this.pubk)
    this.user.password = this.sm2Encrypt(this.user.password,this.pubk)
    console.log(this.user.email)
    console.log(this.user.password)
    return
```
```javascript
  sm2Encrypt(data) {
    const sm2 = require('sm-crypto').sm2;
    console.log(data)
    console.log(this.pubk)
    const cipherMode = 1; // 1 - C1C3C2，0 - C1C2C3，默认为1
    return sm2.doEncrypt(data, this.pubk, cipherMode)
  },
```