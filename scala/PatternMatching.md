
- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-3-5


## Pattern Matching

Pattern Matching 的主要作用是解析数据，

> Pattern matching is a powerful “protocol” for extracting data inside data structures

调用类型的 unapply 方法，unapplySeq 是指类型为 Seq 类型的情况

### 难点

- Matching on Sequences （case head +: tail）
    - 语法糖1，使用 +: 对象的 upapply 方法， +:.unapply(collection)
    - 语法糖2，types with two type parameters can be written with infix notation


### 下面特性，知道就可以

- Matching on Tuples
- Guards in case Clauses
- Matching on case Classes ，
- Matching on Variable Argument Lists， @ _*
- Matching on Regular Expressions
- Binding Variables in case Clauses

### 变量的解析
- case class
- 正则表达式

```
val selectRE = s"""SELECT\\s*(DISTINCT)?\\s+($cols)\\s*FROM\\s+($table)\\s*($tail)?;""".r
val selectRE(distinct2, cols2, table2, otherClauses) = | "SELECT col1, col2 FROM atable;"

```
