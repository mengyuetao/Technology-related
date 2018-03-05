
- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-3-5


## Implicit

知识点比较重要，否则很难阅读优秀个代码。

#### Implicit Arguments

Implicit Arguments 主要场景如下：
- Execution Contexts
- Capabilities
- Constraining Allowed Instances  
- Implicit Evidence
- Working Around Erasure  

难点1

Using implicitly，context bound

难点2

Constraining Allowed Instances 是指使用 canbuildfrom 的模式，例子（JROW，SROW）

难点3

Implicit Evidence 主要是类型检查可以 toMap <:<(A, (T, U))，<:< 是一个类型

> With implicit evidence, we didn’t use the implicit object in the computation. Rather, we only used its existence as confirmation that certain type constraints were satisfied.

难点4
这个场景比较少，知道就可以
Working Around Erasure ,JVM “forgets” the type arguments for parameterized types



#### Improving Error Messages ,@implicitNotFound

用来增加提示可读性

#### Phantom Types

场景，做一些流程等的限制

#### Implicit Conversion

    - Implicit Conversions ,scala.StringContext

#### Implicit Resolution Rules

了解就好

#### Scala’s Built-in Implicits
