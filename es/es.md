



## 环境安装

https://www.elastic.co/guide/en/elasticsearch/reference/7.9/docker.html
https://www.elastic.co/guide/en/kibana/7.9/docker.html

docker run --name kibana -d --link elasticsearch:elasticsearch -p 5601:5601 docker.elastic.co/kibana/kibana:7.9.0


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
