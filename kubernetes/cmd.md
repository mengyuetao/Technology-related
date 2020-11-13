

# command

```

kubectl certificate approve node-csr-ZE5ojIQhDhXF-sV8Fnfq5LNaOFDNDXFfPsbz1TnbSrA  # approve a nole
kubectl cluster-info dump  # get the cluster information
kubectl config view        # get the current config of kubectl

kubectl create clusterrolebinding kubelet-bootstrap --clusterrole=system:node-bootstrapper --user=kubelet-bootstrap  # binding user to role  , for bootstrap kubelet  
kubectl create clusterrolebinding the-boss --user kubernetes --clusterrole cluster-admin       # binding user to role for access kubelet from apiserver
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node     # create a deployment of hello-node
kubectl create -f kubia-manual.yaml                 # cretat a deployment from file of yaml
kubectl delete ReplicationController kubia          # delete a rc
kubectl delete service hello-node                   # delete a service
kubectl desc node 10.0.2.4                          # describe node
kubectl describe kubia-5kx5z                        # describe pod
kubectl describe service kubia-http                 # describe service
kubectl explain pod                        # get the manual of pod command
kubectl explain pod.kind                   # same as above
kubectl expose deployment hello-node --type=LoadBalancer --port=8080     # create a service  with deployment
kubectl expose rc kubia --type=LoadBalancer --name kubia-http            # create a service with rc  
kubectl get cs,nodes  
kubectl get csr                    # get the certificate apply to approve
kubectl get deployments            # get deployment
kubectl get events -a              # get events
kubectl get node                   # get node
kubectl get pod kubia-manual -o yaml       # get pod
kubectl get pods --namespace=kube-system   # get pod
kubectl get pods -o wide                   # get pod
kubectl get rc
kubectl get replicationcontrollers         # get rc

kubectl get service kubia-http             # get service
kubectl get services                       # get service
kubectl get svc -o yaml                    # get service
kubectl logs kubia-manual                  # get logs
kubectl port-forward kubia-manual 8888:8080   # create forward for test
kubectl port-forward  --address 0.0.0.0  consul-server-0  8500:8500
kubectl rm ReplicationController kubia        # delete rc
kubectl run kubia --image=mengyuetao/kubia --port=8080 --generator=run/v1     #run a pod
kubectl scale rc kubia --replicas=2                                           #scale a rep

 kubectl exec -it speechgw-deployment-5cbd9f6747-bmssv bash
 kubectl exec -it  markplatform-deployment-75f6cfdb9f-jn5mq bash
kubectl get  all  -n  namespace  -o yaml  > xxxxxx

```


- k8s 本地存储和网络存储 https://blog.csdn.net/qq_25611295/article/details/85632226
- Kubernetes1.13.0实用整理-k8s存储NFS 即NFS作为Volume https://blog.csdn.net/shenhonglei1234/article/details/84996226
- Kubernetes使用StorageClass动态生成NFS类型的PV https://www.cnblogs.com/00986014w/p/9406962.html
- nfs https://www.cnblogs.com/alonones/p/6105586.html

- http://blog.itpub.net/69953029/viewspace-2695917/ 更新应用时，如何实现 K8s 零中断滚动更新？
- https://www.cnblogs.com/huaweiyuncce/p/9996006.html K8S漏洞报告 | 近期bug fix解读&1.9.11主要bug fix汇总 因此iptables模式实现优雅删除功能不需要任何的代码，以来底层机制即可。
https://www.cnblogs.com/guigujun/archive/2004/01/13/10591919.html Kubernetes系列之理解K8s Service的几种模式
