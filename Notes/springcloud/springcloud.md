# Eureka注册中心

## 搭建EurekaServer

springboot用的是：2.3.9.RELEASE

搭建EurekaServer服务步骤如下：

1.创建项目，引入 spring-cloud-starter-netflix-eureka-server 的依赖（测试用的是2.2.7.RELEASE）

2.编写启动类，添加 @EnableEurekaServer 注解

3.添加application.yml文件，编写如下配置

```
server:
  port: 10086

# 下述两步都是为了注册服务
spring:
  application:
    name: enrekaserver   #eure的服务名
eureka:
  client:
    service-url:  #eureka的地址信息（这里是把自己也注册到eureka上）
      defaultZone: http://127.0.0.1:10086/eureka
```

## 注册user-service

将user-service服务注册到EurekaServer步骤如下：

1.在user-service项目中引入spring-cloud-starter-netflix-eureka-client 依赖（测试用的是2.2.7.RELEASE）

2.在application.yml 编写如下配置：

## 服务实例复制

右键服务-->copy configuration··· --> 取名+设置端口号

<img src="../../assets/image-20210831204945981.png"/>

## 服务发现

在order-service完成服务拉取

服务拉取是基于服务名称获取服务列表，然后对服务列表做负载均衡

1.修改OrderService代码，修改访问url路径，用服务名代替ip:端口

2.在order-service项目启动类中的RestTemplate添加负载均衡注解（@LoadBalanced）



1.引入eureka-client依赖

2.在application.yml中配置eureka地址

3.1 给restTemplate添加@LoadBalanced注解，实现负载均衡

3.2 用服务提供者的服务名远程调用