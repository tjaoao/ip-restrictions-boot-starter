# ip-restrictions-boot-starter
> SpringBoot项目中实现对请求IP的自动限制与拦截
>
> Implementing IP request interception and restriction in Spring Boot.
>
> 作者：[zyyzyykk](https://github.com/zyyzyykk)
>
> 源码：https://git.kkbapps.com/kk/ip-restrictions-boot-starter
>
> maven仓库地址：https://mvnrepository.com/artifact/com.kkbapps/ip-restrictions-boot-starter
>
> 更新时间：2023-12-11

### 💪 使用

1.在Springboot项目中引入依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.kkbapps/ip-restrictions-boot-starter -->
<dependency>
    <groupId>com.kkbapps</groupId>
    <artifactId>ip-restrictions-boot-starter</artifactId>
    <!-- 建议引入最新RELEASE版本 -->
    <version>1.1.3-RELEASE</version>
</dependency>
```

2.在需要请求限制的方法上添加 **@EnableIPLimit** 注解

```java
@EnableIPLimit(count = 100, interval = 1000)
public String needIPLimit(String param) {
```

- 注解参数说明：
  - count：监控周期内同一IP最大访问次数
  - interval：监控周期内同一IP两次访问最小间隔时间（毫秒）
  - 若不添加注解参数，则配置取决于application.properties中的自定义配置以及默认值

3.在 `application.properties` 中可进行自定义配置属性：

```properties
# 相关配置以kkbapps.ip开头，其他配置见提示
kkbapps.ip.forbid-ip=true	# 是否封禁超出限制的ip，默认监控周期内有效
```

4.配置ip黑白名单：**1.1.2及以上版本**

- .properties文件中配置：

  ```properties
  # 配置ip黑名单
  kkbapps.ip.ip-black-list[0]=192.168.42.1
  kkbapps.ip.ip-black-list[1]=192.168.42.2
  ```

- .yml文件中配置：

  ```yml
  # 配置ip黑名单
  kkbapps:
    ip:
      ip-black-list:
        - 192.168.42.1
        - 192.168.42.2
  ```

5.获取ip请求信息：**1.1.3及以上版本**

```java
@EnableIPLimit(count = 100, interval = 1000)
public String needIPLimit() {
    // 获取ip请求信息
    IpRequestInfo ipRequestInfo = IPContext.get();
    // 请求ip: 192.168.42.1
    System.out.println(ipRequestInfo.getIp());
    // ip周期内访问次数: 5
    System.out.println(ipRequestInfo.getCount());
    // ip最近访问时间: Mon Dec 11 19:27:39 CST 2023
    System.out.println(ipRequestInfo.getLastDate());
}
```

### 💡 说明

1.`ip-restrictions-boot-starter` 是一个基于SpringBoot框架的maven依赖包，能够对特定方法、特定IP的请求进行限制与拦截

2.拦截采用AOP实现，**@EnableIPLimit** 注解建议添加在 Controller层、Service层的方法上，防止动态代理失效

3.当请求超出设定的限制，会抛出 **IpRequestErrorException** 异常，可对此异常进行自定义捕获

### 👨‍💻 更新记录

##### 1.1.3-RELEASE：latest

新增在添加有 **@EnableIPLimit** 注解的方法中获取本次ip请求信息

##### 1.1.2-RELEASE：

新增ip黑白名单配置：黑名单ip直接拒绝请求，白名单ip直接放行

##### 1.1.0-RELEASE：

监控粒度由请求ip变为请求ip+请求方法，实现更细粒度的监控

##### 1.0.8-RELEASE：

更新了获取请求ip的方法，废除是否使用nginx代理的配置属性

##### 1.0.5-RELEASE：修复监控时间bug

修复监控时间bug，设置监控时间最小单位为天，默认监控周期为一天，取值范围（1-30）

##### 1.0.3-RELEASE、1.0.4-RELEASE：

**监控时间有bug**，不建议使用

##### 1.0.2-RELEASE：

第一个基础版本

##### 1.0.0-RELEASE：

发布中央仓库测试版

### 🏘️ 关于此项目

作者：[zyyzyykk](https://github.com/zyyzyykk/)

欢迎对此项目提出宝贵的意见或建议，也可以加入我们一起进行此项目的维护与开发
