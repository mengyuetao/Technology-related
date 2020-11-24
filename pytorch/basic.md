
# 数据处理流程

按照姓推断国籍数据处理流程：1.首先把数据按照国籍进行分组；2.每个分组进行洗牌后按固定比例计算需要的训练、评估、测试数据量；3.确保每个分组的数据是一样多，避免训练数据不平衡；3.把数据添加训练、评估、测试的标签并回写到文件

实现思路：
```
1. panda 擅长用于处理表格数据, 计算过程生成中间数据，最后写入csv
2. numpy 用于处理随机数

输入：row.nationality row.surname  train_proportion val_proportion test_proportion
输出：row.nationality row.surname row.split
```
