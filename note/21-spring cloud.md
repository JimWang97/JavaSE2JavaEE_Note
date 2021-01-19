# spring cloud

springcloud + springcloud alibaba

Nginx RabbitMQ

## 微服务框架入门

独立进程。单一应用程序划分成一组小的服务。服务与服务之间采用轻量级的通信机制胡乡写作（通常是HTTP协议的RESTFUL）

 ### springcould

是分布式微服务架构的一站式解决方案，是多种微服务架构落地技术的集合体，俗称微服务全家桶。

![image-20200803151315333](/Users/striverwang/Library/Application Support/typora-user-images/image-20200803151315333.png)

![image-20200803151145816](/Users/striverwang/Library/Application Support/typora-user-images/image-20200803151145816.png)

![image-20200803152059679](/Users/striverwang/Library/Application Support/typora-user-images/image-20200803152059679.png)

![image-20200803152138416](/Users/striverwang/Library/Application Support/typora-user-images/image-20200803152138416.png)



### 版本

springboot 2.x版本，spring cloud使用H版本

![image-20200803153445251](/Users/striverwang/Library/Application Support/typora-user-images/image-20200803153445251.png)

## cloud各种组件的停更/升级和替换

- 服务注册中心
  - Eureka ×
  - Zookeeper √
  - Consul √
  - Nacos √ 学习重点 springcloud alibaba
- 服务调用
  - Ribbon √
  - LoadBalance √  
- 服务调用2
  - Feign ×
  - OpenFeign √
- 服务降级熔断
  - Hystrix ×
  - Resilience4j √
  - alibaba sentinel √ 国内用的多
- 服务网关
  - Zuul ×
  - gateway √
- 服务配置
  - Config ×
  - Nacos √
- 服务总线
  - Bus ×
  - Nacos √

# 订单-支付模块微服务

## 父工程project空间新建

创建maven项目，设置编码，支持注解，java8

- dependencyManagement和Dependencies的区别
  - dependencyManagement父类管理，一般是最顶层的父POM。
  - 子项目会向上找直到dependencyManagement，找到了子类就不再需要版本号了。
  - 可以使每一个子项目版本一致
  - 只是声明依赖，并不会引入依赖。子项目需要显示声明要引入的依赖。



## Rest微服务构建

![image-20200805200627333](/Users/striverwang/Library/Application Support/typora-user-images/image-20200805200627333.png)

### cloud-provider-payment8001微服务提供者支付Module模块

1. 建module
2. 改POM
3. 写yml
4. 主启动
5. 业务类

#### 业务类

1. 建表sql
2. 创建entities、返回前端的json实体
3. 创建dao层,mappe
4. service层
5. controller层



#### 热部署插件

1. 添加devtools jar包到pom`<artifactId>spring-boot-devtools</artifactId>`
2. 添加插件到父类pom中`<artifactId>spring-boot-maven-plugin</artifactId>`
3. 开启自动编译选项![image-20200807204400972](/Users/striverwang/Library/Application Support/typora-user-images/image-20200807204400972.png)
4. REGISTRY中![image-20200807204534802](/Users/striverwang/Library/Application Support/typora-user-images/image-20200807204534802.png)![image-20200807204858661](/Users/striverwang/Library/Application Support/typora-user-images/image-20200807204858661.png)



### Cloud-consumer-order80 微服务消费者订单Module模块

1. 创建entities
2. 创建controller

不需要service和dao，通过微服务的形式去获取其他微服务处理的结果即可。

需要实现两个微服务模块的相互调用

#### RestTemplate

提供便捷访问远程http服务的方法

是httpClient的封装



创建ApplicationContextConfig，引入RestTemplate手动注入

```
return new RestTemplate();
```

### 工程重构

把多个模块中重复代码进行重构。

新建一个模块cloud-api-common，在maven中进行install。删除多个模块中重复的代码，在pom中引入

```
<dependency>
  <groupId>com.wyj.springcloud</groupId>
  <artifactId>cloud-api-common</artifactId>
  <version>${project.version}</version>
</dependency>
```

![image-20200807225104781](/Users/striverwang/Library/Application Support/typora-user-images/image-20200807225104781.png)



## Eureka服务注册与发现

传统的rpc远程调用时，管理每个服务与服务之间依赖关系比较复杂，管理难，需要使用服务治理，管理服务与服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册

**什么是服务注册与发现**

Eureka采用了cs的设计架构，Eureka Server作为服务注册功能的服务器，它是服务注册中心。而系统中其他的微服务，使用Eureka的客户端连接到server并维持心跳链接，这样就可以通过server监控各个微服务是否正常运行

![image-20200808100857954](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808100857954.png)

一个消费者对应多个服务者和多个服务注册中心

### Eureka服务端安装

**单机版**

1. 创建cloud-eureka-server7001包
2. 配置yml
3. 创建主启动类

#### Eureka客户端注册服务

改cloud-provider-payment8001模块的pom，引入eureka，然后在主启动类中使用@EnableEurakeClient

![image-20200808110813181](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808110813181.png)

#### Eureka消费者注册服务

改cloud-sonsumer-order80模块的pom，同8001



![image-20200808111615607](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808111615607.png)



**集群版**

集群就需要服务端相互注册，互相守望

1. 创建cloud-eureka-server7002

2. 修改host文件 

   ```
   127.0.0.1 eurake7001.com
   127.0.0.1 eurake7002.com
   ```

3. 在yml中配置，使其相互注册

   ```
   server:
     port: 7002
   
   eureka:
     instance:
       hostname: eureka7002.com #eureka服务端的实例名称
     client:
       register-with-eureka: false     #false表示不向注册中心注册自己。
       fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
       service-url:
         #集群指向其它eureka
         defaultZone: http://eureka7001:7001/eureka/
   ```

#### 将8001和80发布到两台eureka集群中

```
    service-url:
      defaultZone: http://localhost:7001/eureka, http://localhost:7002/eureka
```

在yml中同时注册进7001和7002即可



#### 服务提供者的集群配置

按照8001创建8002.

把8001的所有都复制一份，cloud-provider-payment8002.

注意application.yml中的port需要修改，两个模块共用一个`spring.application.name = cloud-payment-service`



这样就有localhost:8001和localhost:8002同时能够提供服务了。但是在consumer中目前是写死的去访问localhost:8001，那么我们需要把这个URL改成服务的名称。

```java
public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
```

必须全大写

![image-20200808130805092](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808130805092.png)

不再对外暴露地址，而是微服务名称

需要开启负载均衡功能，才能正常访问，不然不知道到底访问这个微服务下的哪一台主机。

![image-20200808131011554](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808131011554.png)这个注解开启负载均衡，这个其实就是Ribbon的负载均衡功能

此时调用http://localhost/consumer/payment/get/1就会发现端口是8001和8002轮流出现了。

### actuator信息补充

```
eureka.instance.instance-id=payment8001
```

![image-20200808144740310](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808144740310.png)



```
prefer-ip-address: true
```

![image-20200808145103397](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808145103397.png)

左下角显示ip



### 服务发现

对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息

1. 修改8001的controller，添加一个DiscoveryClient，并写一个新的方法用来返回

```
@Resource
private DiscoveryClient discoveryClient;

@GetMapping(value="/payment/discovery")
    public Object discovery(){
        List<String> service = discoveryClient.getServices();
        for(String element : service){
            log.info("element:" + element);
        }

        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance : instances) {
            log.info(instance.getServiceId() + "\t" + instance.getHost() + "\t" + instance.getPort() + "/t" + instance.getUri());
        }
        return this.discoveryClient;
    }
```

2. 主启动类加上@EnableDiscoveryClient注释



### Eureka自我保护机制

![image-20200808150405090](/Users/striverwang/Library/Application Support/typora-user-images/image-20200808150405090.png)

某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存

防止因为网络的拥挤或者卡顿而导致的服务端没有收到心跳包，而让服务实例被注销的情况



#### 如何禁止自我保护机制

服务端server

Eureka.server.enable-self-preservation = false

eureka.server.eviction-interval-in-ms=.... 心跳时间



客户端client

```
#Eureka客户端向服务端发送心跳的时间间隔，单位为秒(默认是30秒)

eureka.instance.lease-renewal-interval-in-seconds: 1

#Eureka服务端在收到最后一次心跳后等待时间上限，单位为秒(默认是90秒)，超时将剔除服务

eureka.instance.lease-expiration-duration-in-seconds: 2
```

## Zookeeper

## Counsul

分布式服务发现和配置管理系统

提供了服务治理、配置中心、控制总线等功能。每一个功能都可以单独使用

有可视化web界面



# springcloud 狂神说

微服务框架的4个核心问题：

- 服务很多，客户端该怎么访问
- 这么多服务，服务之间如何通信
- 这么多服务，如何治理
- 服务器挂了怎么办



解决方案：

- spring cloud netflix 一站式解决方案
  - api 网关，zuul组件
  - 通信：Feign
  - 服务注册发现：Eureka
  - 熔断机制：Hystrix
- apache dubbo zookeeper 半自动，需要整合别人的
  - api没有，需要找第三方组件
  - Dubbo服务通信
  - 服务发现注册：zookeeper
  - 没有熔断机制
- spring cloud alibaba 最新的一站式解决方案，更简单



## Eureka服务注册与发现

基于CS架构，基于REST的服务

![image-20201004120037470](/Users/striverwang/Library/Application Support/typora-user-images/image-20201004120037470.png)

## Ribbon

客户端负载均衡的工具

属于进程式LB，消费方从服务注册中心获知有哪些地方可用，然后自己再从这些地址中选出一个合适的服务器。

