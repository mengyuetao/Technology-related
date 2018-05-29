

# regular-expressions

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2017-12-29

# 概述

正则表达式在软件开放中经常用到，但是用的时候有很多的坑，这里简单表述一下

正则表达式几种类型

1. BRE  基本,metacharater, {},[],[],^,$,.,*.\, 同时（），{} 使用时需要表达成 \(\)， \{\}
2. ERE  在BRE的基础上，加入了 ？ + | 支持， （），{} 无需escape(转意) \
3. PER  略，（），{} 使用时需要表达成 \(\)， \{\} ，加入了很多新功能

例如在命令行中的区别

```
grep -E
grep -G
grep -P

```

# 参考

[Regular expression](https://en.wikipedia.org/wiki/Regular_expression#POSIX_extended)
