




# spring gateway 源码

## gateway config

- 路径 `org.springframework.cloud.gateway.config`



# spring 基础设施

- `@ConfigurationProperties `

```
@ContextConfiguration    springboot 的 @SpringBootTest 互换使用

```

需要 ApplicationContextAware 才能取到 ApplictionContext
Environment 决定 properties 的顺序，PropertySource 就是一种来源（如配置文件，环境变量等，其实自己不用去搞了）
