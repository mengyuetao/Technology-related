

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-3-5

## 概述

Programming.Scala.2nd.Edition Chapter3 重点理解


## 基础

这里描述java基础上的一些差异点，scala对学过java的人来说还是比较容易上手的。

- infix postfix
- 无参方法
- Precedence Rules  :
- Internal and External DSL
- if
- for Comprehensions
- exceptions
- structural type
- by-name parameter
- Enumerations

 ## 难点

- 理解函数 infix postfix
- 无参方法使用，有副作用，去掉括号
- Precedence Rules  : 右到左 如 +: ::
- Internal and External DSL 概念区别
- if Statements Return Values
- for Comprehensions
    - generator ->    , generating individual values from collection
    - Guards: Filtering Values
    - Yielding
- exceptions 鼓励少用，取消checked exception ，可使用@throw 与java 交互增加可读
- structural type, 要了解使用场景
- by-name parameter are in a sense lazy, because evaluation is deferred, but possibly repeated over and over again, 例如 how “loop” constructs can be re‐ placed with recursion
- Enumerations scala 提供了一个类，与虚拟即级别无关。
