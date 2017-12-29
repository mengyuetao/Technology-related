## Finagle 源码分析之 Mux.client


- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2017-12-29

Finagle 客户端实现过程，由于协议无关性，这里以Mux协议为例，Http等流程相同。

```
The core of Finagle is protocol-agnostic, 
meaning its internals provide an extensible RPC subsystem without 
defining any details of specific client-server protocols. 
Thus in order to provide usable APIs for clients and servers, 
there are a number of Finagle subprojects 
that implement common protocols.
```



![详细图](pic/mux.client.png)


Mux.Client 的 trait 的调用时序如下：

* 1 Mux.apply new org:Mux.Client
* 2 StackClient StackClient.stack
* 3 StackClient.stack return new StackBuilder.addModule
* 4 org:Parameterized , orig.configure
* 5 orig:Mux.Client orig.newClient
    * 6 orig:EndpointerStackClient orig.newClient
        * 7 orig:StackClient orig.endpointer
            * 8 orig:StackClient orig.copy1
            * 9 copy1:EndpointerStackClient copy1.newTransport
                *   10 copy1:Mux.Client copy1.newTransport
            * 11 new EndpointerModule( transport, copy1.newDispatch)
        * 12 orig:Mux.Client  copy1.stack
            * 11 StackClient StackClient.stack
                * 12 StackClient return new StackBuilder.addModule
        * 13 (stack ++ endpointer).make
            * 14 endpointer:EndpointModule  endpointer.mk
            * 15 Module.mk
* 16 sf:ServiceFactory  sf.apply
    * 17 sf_next:ServiceFactory sf_next.apply
    * 18 copy1.newDispatch:EndpointStackClient
        * 19 copy1.newDispatch:Mux.client
            * 20 newRequestResponse:mux.dispatcher
                * 21 createServiceFromDispatcher
* 22 createServiceAsFilter
* 23 service.apply
* 24 filter.apply
* 25 mux.dispatch service.apply


Mux 调用apply创建新的客户端 Mux.client （步骤1），StackClient 返回需要的 Module （步骤2，3）
newClient 创建 ServiceFactory，这里首先创建 endpointer（步骤7）

 **最重要的方法**
newTrsport，newDispatch （步骤11）， [filter 嵌入](01-finagle-stack.md) 在步骤（13）
endpointer 绑定到一个 transport，每次请求 service时候，调用一次 newDispath 并提供 transport。
实现这两个模版方法里可以关联不同的协议栈，实现不同类型的客户端如 Mux，http。
