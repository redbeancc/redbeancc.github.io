---
title: RabbitMQ 学习笔记
tags:
  - rabbitmq
  - 消息队列
categories:
  - 学习
description: "\U0001F9E7 RabbitMQ 学习笔记"
cover: 'https://img-blog.csdnimg.cn/direct/e80de7c9d1a5427bb4215bea219f55e2.webp'
swiper_index: 2
abbrlink: fcfb9005
date: 2024-01-13 17:02:13
---
# RabbitMQ

> RabbitMQ：高性能的异步通讯组件

## 1. 初识 MQ

### 1.1 同步操作

> - 本次操作必须知道上次操作的结果
> - 缺点
>   1. 拓展性差
>   2. 性能下降
>   3. 级联失败
> - 优点：
>   1. 即时性高，即刻知道结果

### 1.2 异步操作

> - 每次的操作相互独立，互不影响
> - 三种角色
>   - 消息发送者：生产者
>   - 消息代理（broker）
>   - 消息接收：消费者
> - 优点
>   1. 生产者只需生产一次
>   2. 消费者监听消息代理，便于拓展，解除耦合
>   3. 无需等待，性能变好
>   4. 故障隔离
>   5. 缓存消息，**削峰填谷**
> - 缺点
>   1. 及时性差，不能立即得到结果
>   2. 不确定下游业务是否执行成功
>   3. 业务安全依赖于 broker 的可靠性

### 1.3 MQ

MQ：**Message Queue**，即消息队列（先进先出），异步调用中的 broker

![image-20240111152843297](https://img-blog.csdnimg.cn/img_convert/8acacb05d2737fa61dacdbf823dbeb81.png)

## 2. Rabbit MQ

### 2.1 安装

Rabbit MQ 官网：https://www.rabbitmq.com/

**Docker 安装**

```powershell
docker run \
  -e RABBITMQ_DEFAULT_USER=itheima \
  -e RABBITMQ_DEFAULT_PASS=123456 \
  -v mq-plugins:/plugins \
  --name mq \
  --hostname mq \
  -p 15672:15672 \
  -p 5672:5672 \
  --network hmall \
  -d \
  rabbitmq:3.8-management
# 15672端口：图形化界面
# 5672端口：sh
```

```
docker run -e RABBITMQ_DEFAULT_USER=itheima -e RABBITMQ_DEFAULT_PASS=123456 -v mq-plugins:/plugins --name mq --hostname mq -p 15672:15672 -p 5672:5672 --network hmall -d rabbitmq:3.8-management
```

![image-20240113180015170](https://img-blog.csdnimg.cn/img_convert/2fe7bdc87c3461c0975ec9522bde8958.png)

- Publisher：生产者
- RabbitMQ Server Broker：RabbitMQ 消息代理
- VirtualHost：虚拟主机，起数据隔离作用，每个vh中的exchange和queue相互独立。
- exchange：交换机，消息必须由exchange分发给不同的queue
- queue：消息队列，接收exchange发送的消息
- consumer：消费者，监听queue

### 2.2 快速入门

AMQP：Advance Message Queuing Protocol（高级消息队列**协议**），与语言平台无关，即可以跨语言。

Spring AMQP：spring 基于 AMQP 协议的基础上用 java 重新封装的 api。

#### 1. 创建聚合项目

![image-20240113181518570](https://img-blog.csdnimg.cn/img_convert/6c875792e40acf7230fde1ac9f884751.png)

```
# mq_demo pom文件
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>mq_demo</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>consumer</module>
        <module>publisher</module>
    </modules>
    <packaging>pom</packaging>

    <parent>
        <artifactId>spring-boot-starter-parent</artifactId>
        <groupId>org.springframework.boot</groupId>
        <version>2.7.11</version>
        <relativePath/>
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <!--        AMQP依赖，包含RabbitMQ-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
        <!--        单元测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    </dependencies>

</project>
```

```
# publisher pom文件
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <artifactId>publisher</artifactId>

    <parent>
        <artifactId>mqDemo</artifactId>
        <groupId>com.example</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

</project>

```



#### 1. 父项目引入

这样每个微服务都可以使用

```
        <!--        AMQP依赖，包含RabbitMQ-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
```

#### 2. 每个微服务配置MQ服务器信息

```
logging:
  pattern:
    dateformat: MM-dd HH:mm:ss:SSS
spring:
  rabbitmq:
    host: 192.168.138.10 # 地址
    port: 5672           # 端口
    virtual-host: /hall  # 虚拟主机
    username: hmall      # 用户名
    password: 123456     # 密码
```

注意：

1. 上面的配置信息必须一一对应，需要与15672端口查看
2. 消费者与生产者都必须配置

```
    @Autowired
    private RabbitTemplate rabbitTemplate;
    @Test
    void testSendMessage2Queue() {
        // 队列名
        String queueName = "simple.queue";
        // 消息
        String msg = "hello, AMQP";
        // 发送消息
        rabbitTemplate.convertAndSend(queueName,msg);
    }
```

![image-20240122200658523](https://img-blog.csdnimg.cn/img_convert/f2ad1fb886f478330be6925fc3e2fdf1.png)

#### 3. 接收消息

SpringAMQP 提供声明式的消息监听，需要通过**注解**在方法上声明要监听的队列名称，将来Spring AMQP 就会把消息传递给当前方法。

```
@Component
@Slf4j
public class MqListener {

    @RabbitListener(queues = "simple")
    // msg 类型和上面传送的类型一致
    public void ListenerSimpleQueue(String msg) {
        System.out.println("消费者监听到了 simple 的消息：【" + msg + "】");
    }
}
```

注意：

1. 启动的是consumer 的启动类，不是测试类
2. consumer 也得配置 MQ 服务器信息

![image-20240122203054753](https://img-blog.csdnimg.cn/img_convert/a5f40f92bf7ba0a2613b66d1a1735f67.png)

## 3. Work Queues

任务模型：**让多个消费者绑定一个消息队列，共同消费队列中的信息（每条信息只会被其中之一的消费者消费）**

![image-20240122203313713](https://img-blog.csdnimg.cn/img_convert/8f62fc05c89865c6b664cb44c3f534da.png)

> 1. 创建 work.queue 消息队列
>
> 2. 生产者一秒钟产生 50 个消息
>
> 3. c1 一秒消费一条
>
>    c2 两秒消费一条

### 3.1. 初始化

#### 1. 创建 work.queue 消息队列

![image-20240122204146329](https://img-blog.csdnimg.cn/img_convert/53d5512c5746f9ec0a17a87e2e317663.png)

####  2. 生产者生产消息

```java
    @Test
    void testWorkQueue() throws InterruptedException {
        String queueName = "work.queue";
        for (int i = 1; i <= 50; i++) {
            String msg = "hello, worker, message_" + i;
            rabbitTemplate.convertAndSend(queueName,msg);
            Thread.sleep(20);
        }
    }
```

#### 3. 消费者消费

```java
    @RabbitListener(queues = "work.queue")
    public void ListenerWorkQueue1(String msg) {
        System.out.println("消费者 1 监听到了 work 的消息：【" + msg + "】");
    }

    @RabbitListener(queues = "work.queue")
    public void ListenerWorkQueue(String msg) {
        System.err.println("消费者 2 监听到了 work 的消息.........：【" + msg + "】");
    }
```

![image-20240122205146112](https://img-blog.csdnimg.cn/img_convert/6c88ffcfa9429cfe63a1e5924368f8a1.png)

**观察到：**

- **消费者 1 全是奇数，消费者 2 全是偶数**
- **信息只会被消费一次**
- **消费者 1 和消费者 2 所有消费的信息之和 = 生产所有的信息**

修改代码：

- 假设消费者 1 的性能好，消费者 2 的性能相对弱一点

```java
    @RabbitListener(queues = "work.queue")
    public void ListenerWorkQueue1(String msg) throws InterruptedException {
        System.out.println("消费者 1 监听到了 work 的消息：【" + msg + "】");
        Thread.sleep(20);
    }

    @RabbitListener(queues = "work.queue")
    public void ListenerWorkQueue(String msg) throws InterruptedException {
        System.err.println("消费者 2 监听到了 work 的消息.........：【" + msg + "】");
        Thread.sleep(200);
    }
```

![image-20240122205740016](https://img-blog.csdnimg.cn/img_convert/fab3e169e4b9f3181a57db04dee22f81.png)

**观察到：即使是性能不同**

- **消费者1 和 消费者2 都是对半的消息数量**
- **同时消费者 1 消费奇数，消费者 2 消费偶数**
- **消费者1 消费过了，消费者2 不会消费**

#### 4. 思考

轮询的结果是一人投一个，如果想让性能好的机器多消费一点，性能差的机器消费少一点怎么办？

### 3.2. 消费者消息推送限制

> 默认情况下，RabbitMQ会将消息依次轮询投递给绑定在队列上的每一个消费者。但并没有考虑到消费者是否已经处理完消息，可能出现消息堆积。
>
> 因此我们需要修改 application.yml ，设置 preFectch 值为1，确保同一时刻最多投递给消费者 1 条消息

```yml
spring:
  rabbitmq:
    listener:
      simple:
        prefetch: 1      # 每次只能取 1 条信息，信息处理完成才能获取下一条（消费者端开启）
```

![image-20240122211121926](https://img-blog.csdnimg.cn/img_convert/dfd818c5a76dbd47a1df2076552c42f8.png)

**观察到：**

- **序号变成顺序**

- **性能好的多接收消息，性能差的处理的消息少**

- **也就是处理完了一条消息，下一条消息才发放出来**

#### 意义

当产生的信息在队列中远远超过**单个消费者**消费的能力，这时候出现**消费堆积**

处理消费堆积的方法之一：**增加消费者**，同时消费者的消费能力有大小，所以根据消费者的性能来消费消息意义重大。

### 3.3. 总结

Work 模型的使用：

- 多个消费者绑定一个队列，可以加快消息处理速度
- 同一条消息只会被一个消费者处理
- 通过设置 prefetch 来控制消费者预取的消息数量，处理完一条在处理下一条，实现能者多劳

## 4. 交换机 exchange

> 真正生产环境都会经过 exchange 来发送信息，而不是直接发送到队列，交换机的类型有以下三种：
>
> - Fanout：广播
> - Direct：定向
> - Topic：话题

![image-20240122223303507](https://img-blog.csdnimg.cn/img_convert/9c8f8328ca8b2eb37d83850ed0603129.png)

### 4.1 Fanout 交换机

> Fanout exchange 会将接收到的消息**广播**到**每一个**跟其绑定的 queue ，所以也叫广播模式。

![image-20240122223515945](https://img-blog.csdnimg.cn/img_convert/22b313f62698926a22f5d44efa95b993.png)

<span style="font-size:24px">**4.1.1 测试**</span>

> 1. 在可视化页面中创建，队列 fanout.queue1 和 fanout.queue2
> 2. 在可视化页面中创建，交换机 hmall.fanout，将两个队列将其绑定
> 3. 在 consumer 服务中，编写两个消费者方法，分别监听 fanout.queue1 和 fanout.queue2
> 4. 在 publisher 服务中，编写测试方法，向 hmall.fanout 发送消息

![image-20240122224156765](https://img-blog.csdnimg.cn/img_convert/c68ac21bd8f923bc214e6e7c4861940c.png)

**1. 在可视化页面中创建，队列 fanout.queue1 和 fanout.queue2**

![image-20240122224709591](https://img-blog.csdnimg.cn/img_convert/fc99118355841721ad9cf0d7ece9c200.png)

**2. 在可视化页面中创建，交换机 hmall.fanout，将两个队列将其绑定**

![image-20240122224807992](https://img-blog.csdnimg.cn/img_convert/cd0169d00d3e002f6d9698444471a727.png)

![image-20240122224857362](https://img-blog.csdnimg.cn/img_convert/1509192e4ead711d7c93910c6bc439a8.png)

**3. 在 consumer 服务中，编写两个消费者方法，分别监听 fanout.queue1 和 fanout.queue2**

```java
    @RabbitListener(queues = "fanout.queue1")
    public void listenFanoutQueue1(String msg) {
        System.out.println("消费者 1 监听到了 fanout.queue1 的消息：【" + msg + "】");
    }

    @RabbitListener(queues = "fanout.queue2")
    public void listenFanoutQueue2(String msg) {
        System.out.println("消费者 2 监听到了 fanout.queue2 的消息：【" + msg + "】");
    }
```

**4. 在 publisher 服务中，编写测试方法，向 hmall.fanout 发送消息**

```java
    @Test
    void testFanoutExchange() {
        String exchangeName = "hmall.fanout";
        String msg = "hello, everyone";
        // routingKey 暂未设置，可以为 null 或 ""
        rabbitTemplate.convertAndSend(exchangeName,"",msg);
    }
```

![image-20240122230240815](https://img-blog.csdnimg.cn/img_convert/ca2966eab4c1a87faa1f1559a5e8c9d6.png)

<span style="font-size:24px">**4.1.2 总结**</span>

**交换机的作用是什么？**

- **接收 publisher 生产的消息**
- **将消息按照规则路由到与之绑定的队列**
- **FanoutExchange的会将消息路由到每个绑定的队列**

### 4.2 Direct 交换机

> Direct exchange 会将接收到的消息根据规则路由到指定的 Queue，因此成为**定向**路由
>
> - 每个 Queue 都与 Exchange 设置一个 BindingKey
> - 发布者发布消息时，指定消息的 RoutingKey
> - Exchange 将消息路由到 BindingKey 和 RoutingKey 一致的队列

![image-20240123163101379](https://img-blog.csdnimg.cn/img_convert/c713449a5dda4967ffc2e9ea3fad832a.png)

<span style="font-size: 24px;">**4.2.1 案例**</span>

> 1. 在可视化页面创建，队列 direct.queue1 和 direct.queue2
> 2. 在可视化页面创建，交换机 hmall.direct，将两个队列与其绑定
> 3. 在 consumer 服务中编写，两个消费者方法，分别监听 direct.queue1 和 direct.queue2
> 4. 在 publisher 服务中编写，测试方法，利用不同的 RoutingKey 向 hmall.direct 发送消息

![image-20240123163634828](https://img-blog.csdnimg.cn/img_convert/2e706c854dd1bbcbd42ed019d33a762b.png)

**1. 在可视化页面创建，队列 direct.queue1 和 direct.queue2**

![image-20240123163759623](https://img-blog.csdnimg.cn/img_convert/400ccb5d9682473a441f9ab9f6896c7c.png)

**2. 在可视化页面创建，交换机 hmall.direct，将两个队列与其绑定 **

![](https://img-blog.csdnimg.cn/img_convert/730988d5a157e2d4ab54c21ef7850afb.png)

绑定：

- 一次 RoutingKey 只能写一个
- 两个的写两次

![image-20240123164342863](https://img-blog.csdnimg.cn/img_convert/269255271937f176ce96e95bebb0a909.png)

**3. 在 consumer 服务中编写，两个消费者方法，分别监听 direct.queue1 和 direct.queue2**

```java
    @RabbitListener(queues = "direct.queue1")
    public void listenDirectQueue1(String msg) {
        System.out.println("消费者 1 接收到 direct.queue1 的消息：【" + msg + "】");
    }

    @RabbitListener(queues = "direct.queue2")
    public void listenDirectQueue2(String msg) {
        System.out.println("消费者 2 接收到 direct.queue2 的消息：【" + msg + "】");
    }
```

**4. 在 publisher 服务中编写，测试方法，利用不同的 RoutingKey 向 hmall.direct 发送消息**

```java
    @Test
    void testDirectExchange() {
        String exchangeName = "hmall.direct";
        String red_msg = "红色消息";
        String yellow_msg = "黄色消息";
        String blue_msg = "蓝色消息";
        rabbitTemplate.convertAndSend(exchangeName,"red",red_msg);
        rabbitTemplate.convertAndSend(exchangeName,"yellow",yellow_msg);
        rabbitTemplate.convertAndSend(exchangeName,"blue",blue_msg);
    }
```

![image-20240123165705143](https://img-blog.csdnimg.cn/img_convert/060e2d6688a7b0fa40dcc702035cbebf.png)

<span style="font-size:24px">**4.2.2 总结**</span>

**描述下 Direct 交换机和 Fanout 交换机的差异？**

- **Fanout 交换机是*广播* 发送到每一个与之绑定的队列**
- **Direct 交换机是根据 *RoutingKey* 判断发送给哪个队列**
- **如果多个队列具有相同的 RoutingKey，则与 Fanout 功能类似**

### 4.3 Topic 交换机

> Topic Exchange 与 Direct Exchange 类似，区别在于 RoutingKey 可以是多个单词的列表，并且以 **.** 分割
>
> Queue 和 Exchange 指定的 BindingKey 时可以使用通配符：
>
> - **#**：代指 0 或 多个单词
> - *****：代指一个单词

![image-20240123171620175](https://img-blog.csdnimg.cn/img_convert/8599453aad0bc3836b2be71eaaaae8bd.png)

<span style="font-size: 24px;">**4.3.1 案例**</span>

> 1. 在可视化页面创建，队列 topic.queue1 和 topic.queue2
> 2. 在可视化页面创建，交换机 hmall.topic，将两个队列与其绑定
> 3. 在 consumer 服务中编写，两个消费者方法，分别监听 topic.queue1 和 topic.queue2
> 4. 在 publisher 服务中编写，测试方法，利用不同的 RoutingKey 向 hmall.topic发送消息

![image-20240123172442268](https://img-blog.csdnimg.cn/img_convert/7fd3b71463c486624029246e94418375.png)

**1. 在可视化页面创建，队列 topic.queue1 和 topic.queue2**

![image-20240123171944540](https://img-blog.csdnimg.cn/img_convert/e8aa2149f8267b04a4251e02e4f27055.png)

**2. 在可视化页面创建，交换机 hmall.topic，将两个队列与其绑定**

![image-20240123172604980](https://img-blog.csdnimg.cn/img_convert/237240a2face2b779f5620fa327e8acc.png)

![image-20240123173001646](https://img-blog.csdnimg.cn/img_convert/f3d38c29f16b170658ab1c6ee4a5d977.png)

**3. 在 consumer 服务中编写，两个消费者方法，分别监听 topic.queue1 和 topic.queue2**

```java
    @RabbitListener(queues = "topic.queue1")
    public void listenTopicQueue1(String msg) {
        System.out.println("消费者 1 接收到 topic.queue1 的消息（china.#）：【" + msg +"】");
    }

    @RabbitListener(queues = "topic.queue2")
    public void listenTopicQueue2(String msg) {
        System.out.println("消费者 2 接收到 topic.queue2 的消息（#.news）：【" + msg +"】");
    }
```

**4. 在 publisher 服务中编写，测试方法，利用不同的 RoutingKey 向 hmall.topic发送消息**

```java
    @Test
    void testTopicExchange() {
        String exchangeName = "hmall.topic";
        rabbitTemplate.convertAndSend(exchangeName,"china.weather","china.weather");
        rabbitTemplate.convertAndSend(exchangeName,"china.news","china.news");
        rabbitTemplate.convertAndSend(exchangeName,"japan.news","japan.news");
    }
```

![image-20240123174518139](https://img-blog.csdnimg.cn/img_convert/a5e80b69ebf4819510d8fffc7bbaca39.png)

<span style="font-size:24px">**4.3.2 总结**</span>

**描述下 Direct 交换机和 Topic 交换机的差异？**

- **Topic 交换机接收的消息 RoutingKey 可以是多个单词，以 . 分割**
- **Direct 交换机接收的消息 RoutingKey 是定死的**
- **Topic 交换机与队列绑定时的 bindingKey 可以指定通配符**
- **#：代指 0 或 多个单词**
- ***：代指一个单词**

## 5. java 声明队列和交换机

### 5.1 基于 bean 声明

> SpringAMQP 提供了几个类，用来声明队列、交换机及其绑定关系：
>
> - Queue：用于声明队列，可以用工厂类 QueueBuilder 构建
> - Exchange：用于声明交换机，可以用工厂类 ExchangeBuilder 构建
> - Binding：用于声明队列和交换机的绑定关系，可以用工厂类 BindingBuilder 构建

![image-20240123193003397](https://img-blog.csdnimg.cn/img_convert/621332777d522d9de41def552c8072e2.png)

**Consumer 创建 config/FanoutConfiguration**

```java
// Consumer/config/FanoutConfiguration
@Configuration
public class FanoutConfiguration {

    // 交换机创建
    @Bean
    public FanoutExchange fanoutExchange() {
        // 两种方式
        // ExchangeBuilder.fanoutExchange("hmall.fanout2").build();
        return new FanoutExchange("hmall.fanout2");
    }

    // 队列创建
    @Bean
    public Queue fanoutQueue3() {
        // 也可以使用 队列工厂类创建队列，其中 durable 代表其持久化
        // QueueBuilder.durable("fanout2.queue3").build();
        return new Queue("fanout2.queue3");
    }

    @Bean
    public Queue fanoutQueue4() {
        Queue queue4 = QueueBuilder.durable("fanout2.queue4").build();
        return queue4;
    }

    // 绑定 binding
    // 实现队列和交换机的绑定
    // 参数：交换机，队列
    @Bean
    public Binding fanoutBinding3(FanoutExchange fanoutExchange,Queue fanoutQueue3) {
        // 绑定(bind) 队列(Queue) 给(to) 交换机(exchange)
        return BindingBuilder.bind(fanoutQueue3).to(fanoutExchange);
    }

    @Bean
    public Binding fanoutBinding4() {
        // 直接使用方法传参
        return BindingBuilder.bind(fanoutQueue4()).to(fanoutExchange());
    }
}
```

![image-20240123195720117](https://img-blog.csdnimg.cn/img_convert/1bfbde83be9d65053468e7630d1d3457.png)

**若想实现 Direct 交换机创建，代码参考如下**

```java
// Consumer/config/DirectConfiguration
@Configuration
public class DirectConfiguration {
    @Bean
    public DirectExchange directExchange() {
        return ExchangeBuilder.directExchange("hmall.direct2").build();
    }

    @Bean
    public Queue directQueue1() {
        return QueueBuilder.durable("direct2.queue1").build();
    }

    @Bean
    public Queue directQueue2() {
        return QueueBuilder.durable("direct2.queue2").build();
    }

    @Bean
    public Binding directBinding1Red() {
        return BindingBuilder.bind(directQueue1()).to(directExchange()).with("red");
    }

    @Bean
    public Binding directBinding1Blue() {
        return BindingBuilder.bind(directQueue1()).to(directExchange()).with("bule");
    }

    @Bean
    public Binding directBinding2Red() {
        return BindingBuilder.bind(directQueue2()).to(directExchange()).with("red");
    }

    @Bean
    public Binding directBinding2Yellow() {
        return BindingBuilder.bind(directQueue2()).to(directExchange()).with("yellow");
    }
}
```

![image-20240123201652631](https://img-blog.csdnimg.cn/img_convert/c996abb5b117c63d8a3fe187f29bf622.png)

**从以上代码不难看出，创建 Direct 交换机和队列代码繁杂，因此，接下来提出基于注解的声明。**

### 5.2 基于注解的声明

> SpringAMQP 还提供了基于 @RabbitListener 注解来声明队列和交换机的方式
>
> 例如：使用 注解 创建上例交换机和队列
>
> **ctrl + p：可用于提示参数列表**

```java
// Consumer/listeners/MqListener
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "direct3.queue1", durable = "true"),
            exchange = @Exchange(name = "hmall.direct3", type = ExchangeTypes.DIRECT),
            key = {"red","blue"}
    ))
    public void listenDirectQueue3(String msg) {
        System.out.println("消费者 1 收到了来自 " +
                "交换机（hmall.direct3）中队列（direct3.queue1）的消息：【" + msg + "】");
    }

    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "direct3.queue2",durable = "true"),
            exchange = @Exchange(name = "hmall.direct3",type = "direct"),
            key = {"red","yellow"}
    ))
    public void listenDirectQueue4(String msg) {
        System.out.println("消费者 2 收到了来自 " +
                "交换机（hmall.direct3）中队列（direct3.queue2）的消息：【" + msg + "】");
    }
```

![image-20240123204316489](https://img-blog.csdnimg.cn/img_convert/cc65c90265a954afbd92657c175e0021.png)

### 5.3 总结

**声明队列、交换机、绑定关系的 Bean 是什么？**

- **Queue**
- **FanoutExchange、DirectExchange、TopicExchange**
- **Binding**

**基于 @RabbitListenner 注解创建队列和交换机有哪些常见注解？**

- **@Queue**
- **@Exchange**

## 6. 消息转换器

> Spring 对消息对象的处理是基于 JDK 的ObjectOutputStream 完成序列化的。存在以下问题：
>
> - JDK 的序列化有安全风险
> - JDK 序列化的消息太大
> - JDK 序列化的消息可读性差
>
> 建议采用  JSON 序列化代替默认的 JDK 序列化，要做两件事：
>
> - 在 publisher 和 consumer 中都要引入 jackson 依赖
>
>   ```xml
>   <dependency>
>       <groupId>com.fasterxml.jackson.core</groupId>
>       <artifactId>jackson-databind</artifactId>
>   </dependency>
>   ```
>
> - 在 publisher 和 consumer 中都要配置 MessageConverter
>
>   ```java
>   @Bean
>   public MessageConverter messageConverter() {
>       return new Jackson2JsonMessageConverter();
>   }
>   ```

**publisher 测试代码**

```java
    @Test
    void testSendObject() {
        Map<String,Object> msg = new HashMap<>(2);
        msg.put("name","zhangsan");
        msg.put("age",18);
        rabbitTemplate.convertAndSend("object.queue",msg);
    }
```

![image-20240124162009652](https://img-blog.csdnimg.cn/img_convert/5ee1b29efef926e611370f1c6849d5a6.png)

**Consumer 接收消息**

```java
    @RabbitListener(queues = "object.queue")
    // 接收类型和传送类型一致
    public void listenObjectQueue(Map<String,Object> msg) {
        System.out.println("消费者接收到 object.queue 的消息：【" + msg +"】");
    }
```

