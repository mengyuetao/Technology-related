#### 容器概要

- 理解 BeanFactory是核心，`ApplictionContext` 实际上就是BeanFactory 通过 meta 信息来创建Bean `Envoroment` 实际上那就是结合 `ActiveProfile` 还有  `propertiesSource`
构建 meta，进而创建Bean； spring autoconfig， 其实施要配置一个autoconfig的路径的。
- `@SpringBootConfiguration`

### 测试概要
- 由于Spring起来有一个  `applicationContent` 测试主要是支撑集成测试Junit5,spring-test 定义了这个 Content 构建的基础设施，springboot 简化了规则（于@springBootApplication）所以只要使用springboot那套就可以了，spring-test只做理解就好，但是springboot加载了太多的东西，所以就有了分片测试，搞了一大堆的*test，无非就是扫描规则不通，配置不同的 applicationContent
