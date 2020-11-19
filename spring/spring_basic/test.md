## overview
- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2020-10-29  


## principle

You can instantiate objects by using new operator
you can use mock objects instead of real dependencies
you need to move beyond unit testing and start integration(with a Spring ApplicationContext)
- perform integration testing without requiring deployment of your application
- or connect to other infrastructure


## Mockito


```
org.springframework.cloud.gateway.filter.ForwardRoutingFilterTests 代码分析

MockitoJUnitRunner 用来启动单元测试

@InjectMocks

```

verify and stubbing
