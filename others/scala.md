

#### 2018.1.30



finatra ```httpObject httpRescure```

httpObject 转换 Feature[Option[T]] 到 Feature[T]
httpRescure 转换 Timeout等异常为统一的 httpException

总结如下：

- scalaTest ,test、shouldequal、assertFeature
- Rescure[RescureException:mainfest],mainfest 的使用， isAssigableFrom 
- Feature convert 模式，try[Option[T]] 如何包括 T, Throw，getornotfoundException 模式,统一notFond异常
- Feature convert 模式，rescue 统一异常结果
- implict 扩展 Feature 能力，添加功能
- 多个Feature 结果合并 使用 for comprehencen
- searchService 嵌套 service ， public ，private


####  2018.1.31

finatra exceptions

```ExceptionManager``` 注册 ```ExceptionMapper[T]```
```T``` 作为key，Mapper 作为 Value  Mapper toResponse 把 T 转换为 Response

总结如下

- SimpleResponse，RequestBuiler，InmemoryStateReceiver，RamdonStringUtil，TestInject（）.create

