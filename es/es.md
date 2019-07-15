


## 基础
新建一个文档

```
PUT /my_index/doc/1
{ "text" : "quick brown fox" }
```


得分解释

idf inverse document frequence 值越大相关性越低
tf  term frequence  值越大相关性越高
norm  字段长度值越大相关性越低

```
get /my_index/_search
{
  "explain": true,
  "query": {
    "term": {
      "text": "fox"
    }
  }
}
```

## query原理

match 被重写成 term

Boolean
