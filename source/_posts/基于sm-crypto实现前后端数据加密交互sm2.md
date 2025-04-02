---
title: 基于sm-crypto实现前后端数据加密交互sm2
tags:
  - spring
  - vue
  - sm2
categories:
  - 教程
description: "\U0001F4D6 前后端数据加密交互"
abbrlink: 9f8a29c0
date: 2022-04-23 13:26:22
cover:
---
# 基于sm-crypto实现前后端数据加密交互sm2
### springboot引入依赖
```xml
        <!-- 国密 -->
        <dependency>
            <groupId>com.antherd</groupId>
            <artifactId>sm-crypto</artifactId>
            <version>0.3.2</version>
        </dependency>
```
###### 获取公钥和私钥放在redis数据库中
###### 公钥做k，私钥做v
```java
    /**
     * 获取公钥
     * @return
     */
    @Override
    public Result getSM2() {
        Keypair keypair = Sm2.generateKeyPairHex();
        String publicKey = keypair.getPublicKey();
        String privateKey = keypair.getPrivateKey();
        stringRedisTemplate.opsForValue().set(publicKey, privateKey, 10, TimeUnit.MINUTES);
        return Result.success(publicKey);
    }
```
###### 在login中，进行校验
```java
    // 对邮箱和密码进行解密
    String privatekey = stringRedisTemplate.opsForValue().get(userLoginVo.getKey());
    if (StrUtil.isBlank(privatekey)) {
        return Result.error("406", "登录时间过长，请刷新页面");
    }
    String email = Sm2.doDecrypt(userLoginVo.getEmail(), privatekey);
    String password = Sm2.doDecrypt(userLoginVo.getPassword(), privatekey);
    stringRedisTemplate.delete(userLoginVo.getKey());
```
### vue安装sm-crypto
```
npm install sm-crypto --save
```
###### 在页面created的时候，获取公钥，和获取验证码一样
###### 存入到user.key中，这个key就是从后端获取的公钥
###### 调用前端的插件，按照公钥进行加密
```javascript
      getKey() {
        this.request.get("/user/sm2").then(res => {
          if (res.code === "200") {
            this.user.key = res.data
          }
        })
      },
      doSM2(data) {
        const sm2 = require('sm-crypto').sm2
        let encryptData = sm2.doEncrypt(data,this.user.key,1)
        return encryptData
      },
```
###### 加密完成后赋值给新的对象，该对象为发送对象
```javascript
login() {
        this.$refs['userForm'].validate((valid) => {
          if (valid) {
            this.userSend.captcha = this.user.captcha
            this.userSend.code = this.user.code
            this.userSend.key = this.user.key
            this.userSend.email = this.doSM2(this.user.email)
            this.userSend.password = this.doSM2(this.user.password)
            request.post("/user/login", this.userSend).then((res) => {
              if (res.code === '200') {
                this.user = {}
                this.userSend = {}
                this.$message.success(res.msg)
                this.$router.push("/")
              } else {
                this.$message.error(res.msg)
                this.user = {}
                this.userSend = {}
                this.getCaptcha()
                this.getKey()
                return false
              }
            })
          }
        })
      },
```
###### 最后发送时的效果
```json
captcha: "e5e7bbcb9db6435db5d234d059b4fa32"
code: "svku"
email: "dba2161072c4650a1e9cf6bfd812b162cb1f8396ffe63f9ac636e9ecb17cb4e4e7cca95852cb1bd1262a54eaeee2b05272f39181e461b34d256e3ade4ab71032146baa68f6ec4040ec87be556a03b2907a6f6dadf4b4a1b9a3f35137ee3ca0d450648d1a9a8ec1a686c865a8e8"
key: "0447a7b506bc611fd9b23402321c050e0ad4acc4ade3e05f1b49ca84c43de7f0fa40ba45870c64dac055b23b82bf0858c3026c8d6cc4b600aa16049f048d56bbfa"
password: "03cc8340ca06c0e069b3f1edbe9abf038a605894a9c0b856b1273ceb9dbf47aac8333392ceef42df636e079b3721b6510793f8bd0c980bcae4c1d6306a90d0ba972280d818047691ce00e12d8247272e1e0b531edb549d934ce1306d9fe5867a49be2b99d6"
```