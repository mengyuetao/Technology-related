

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
kubectl rm ReplicationController kubia        # delete rc
kubectl run kubia --image=mengyuetao/kubia --port=8080 --generator=run/v1     #run a pod
kubectl scale rc kubia --replicas=2                                           #scale a rep

```
