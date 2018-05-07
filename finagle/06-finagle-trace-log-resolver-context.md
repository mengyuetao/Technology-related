## Finagle 源码分析之 log、resolver、trace、context

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-05-07

#### log

基本上是对 slf4j 封装， 但是加入了一些 Scala的写法



### resolver

com.twitter.finagle.Resolver

待补充

### Contexts


Contexts.Local (1) ->
(1) LocalContext (1) ->
(N) Local (Map[key,any])(1) ->
(N) ThreadLoal(array[Option[_]])

入上所诉，

```
1. 一个Local是ThreadLocal array中的一个条目。
2. 一个LocalContext是一个存放Map的Local
3. Contexts.Local == new LocalContext
```

### trace


流程 ：

1. HttpClientTraceInitializer 调用 letTracerAndNextId
2. HttpServerTraceInitializer 调用 letTracerAndNextId
3. letTracerAndNextId  执行
      1. 如果当前上下文是否有 id，没有生成新的Id，有使用现有的id
      2. 生成子Id
      3. 查询 tracer， 执行 record 方法

tracer 配置？

```
client，server 使用 withTracer 方法，如不配置使用默认
```

trace 如何跨多服务？

http实现如下:

```
com.twitter.finagle.Http.client  处理栈中
  使用  HttpClientTraceInitializer 添加当前上下文的 traceId 到 request

com.twitter.finagle.Http.server  处理栈中
  使用  HttpServerTraceInitializer 在 Request 中解析 traceId
```

mux 实现如下：

```
放入 Contexts.broadcast  , 通过 MarshalledContext 实现
```

trace 记录？

```
ClientTracingFilter
```
