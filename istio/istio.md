# istio

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2021-04-23


#overview
it's a good  starting point for us to update technique stack by learn and analyze istio Bookinfo Application exsample.

```
access to the vm by:
ssh  -p 60322 root@127.0.0.1
```
#refer
https://blog.csdn.net/luo15242208310/article/details/99290541 Istio-proxy Iptable规则
https://www.sohu.com/a/422011681_187948 实现全托管，腾讯云服务网格的架构演进
https://blog.csdn.net/liangkiller/article/details/105758857  prometheus relabel_config 详解加示例
https://prometheus.io/docs/prometheus/latest/configuration/configuration/#regex  prometheus CONFIGURATION

#analyze

## 监控
Bookinfo 通过prometheus 配置 kubernetes_sd_configs 自动发现机制发现 target
> targets are discovered via service discovery or static configuration

>In Prometheus terms, an endpoint you can scrape is called an instance, usually corresponding to a single process. A collection of instances with the same purpose, a process replicated for scalability or reliability for example, is called a job

- 通过 configMap 生成 promethus.yaml 配置文件，通过 serviceAccount，RoleBind 并关联promethus Pod，获取访问 K8s APIservier的访问权限， token 在默认的 /var/run/secrets/kubernetes.io/serviceaccount/token 下。
- target 对应一个 job，通过对 target discovery label 进行 relabel_configs 配置，选择合适的 instance 到job
比如 namespace，endpint name等等，然后利用 __address__ , __schema_path 等标签获取 metric 拉取的地址
- Promethus 按照promethus.yaml, 拉取 node，endpoint，service，docker，pod等监控指标

## Todo
- instance 中的 promethus label是哪里来的呢？ istio 的 anotion 如何转化为 __address__
- 理解 promethus 查询原理
