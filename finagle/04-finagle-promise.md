## Finagle 源码分析之 Promise


- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-1-2

在阅读代码时，com.twitter.finagle.mux.ClientDispatcher 使用Promise加递归可以实现无限循环的读数据，通过分析，需要深入理解Promise 实现原理，
包括waitingQueue，Interapter等，请读取源码。


```

com.twitter.finagle.mux.ClientDispatcher

 private[this] val processAndRead: Message => Future[Unit] =
    msg => {
      process(msg)
      readLoop()
    }

  /**
   * Reads indefinitely from the transport, handing successfully
   * decoding messages to `processAndRead`.
   */
  private[this] def readLoop(): Future[Unit] =
    trans.read().flatMap(processAndRead)

  readLoop().onFailure {
    case exc: Throwable =>
      trans.close()
      val result = Throw(exc)
      for (u <- messages.synchronized(messages.unmapAll())) {
        u() = result
      }
  }
```



* 第一次 readLoop，返回 Feature-01
* onFailure 添加 handler 到 Fature-01 waitQueue
* tans.read 读取到数据处理数据并返回 Feature-02
* flatMap 会 merge Feature01 到 Feature-02
* onFailure 被添加到 Fature-02 waitQueue
* 由于 Fature-01 没有被引用，将被释放
* 如此直到 Fature-N 失败，OnFailure 触发，关闭Trans。


## 实现细节

 ####  respond 函数

  -  生成新的Future ， chain 老的 future
  - K[A] Monitored


 #### transform 函数

 - 生成新的Future 状态为Transforming， FowardInterrupt到原有的 Feature
 - K[A] Transform
 - 原有的Future 完成后，become fn 返回的 feature

 #### setValue 函数

 - 使用 独立的线程执行  全部的 K[A]
