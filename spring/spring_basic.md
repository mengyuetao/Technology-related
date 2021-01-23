## overview
- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2020-12-10  

# 容器
- 理解 BeanFactory是核心，`ApplictionContext` 实际上就是BeanFactory 通过 meta 信息来创建Bean
- `Envoroment` 实际上那就是结合 `ActiveProfile` 还有  `propertiesSource` 构建 meta，进而创建Bean；
- spring autoconfig， 其实施要配置一个autoconfig的路径的

# 测试

principle
You can instantiate objects by using new operator
you can use mock objects instead of real dependencies
you need to move beyond unit testing and start integration(with a Spring ApplicationContext)
perform integration testing without requiring deployment of your application or connect to other infrastructure

- 为了通过Junit集成测试  `applicationContent` 组装的Bean,spring-test 定义了这个测试这个 Content 构建的基础设施
- 相比spring-test, Springboot 简化了规则 `springBootApplication` 所以只要使用springboot那套就可以了，spring-test只做理解就好
- 但springboot加载了太多的东西，所以就有了分片测试，搞了一大堆的*test，无非就是扫描规则不通，配置不同的 applicationContent
- 集成测试，通过 wiremock 可以构建基于http的Stub ，支撑集成测试  http://wiremock.org/
- 单元测试 Mockito

```
org.springframework.cloud.gateway.filter.ForwardRoutingFilterTests 代码分析
MockitoJUnitRunner 用来启动单元测试 @InjectMocks
学会 verify and stubbing
```

# 分层理解
怎么去理解spring层次，更好的理解各个模块的关系
起点，理解 springboot
中点，理解auto-config，找个源码阅读 spring-cloud-gateway 这个项目实现了异步的http服务，可以通过阅读源码了解spring原理

>Spring Boot auto-configuration attempts to automatically configure your Spring application based on the jar dependencies that you have added.

```
    可以从这些类开始 GatewayAutoConfiguration routePredicateHandlerMapping HandlerMapping
    基础 Auto-configuration，starter，定位 META-INF/spring.factories
    原理 @Configuration + @Conditional
    使用 start 和 autoconfig （使使用整体简单，但是各种依赖，使调整配置变得复杂，需要理解内部autoconfig的加载关系）
```

# 技术点 负载均衡

负载均衡可参考实践项目 makou

相关文档：
https://spring.io/guides/gs/spring-cloud-loadbalancer/
https://www.jianshu.com/p/dea0fff94e64  Spring Cloud升级之路 - Hoxton - 3. 负载均衡从ribbon替换成spring-cloud-loadbalancer
https://www.cnblogs.com/yourbatman/p/11532729.html  为何一个@LoadBalanced注解就能让RestTemplate拥有负载均衡的能力？


#技术点 异步调用
https://projectreactor.io/docs/core/release/reference/
