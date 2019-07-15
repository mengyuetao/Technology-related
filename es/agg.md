
##

```

get /cars/_search
{
  "size":0,
  "aggs": {
    "colors":{
      "terms" :  {
        "field":"color.keyword"
      }
    }
  }
}


```
