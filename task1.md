# Task 1 

$ kubectl version --client --short

``` Client Version: v1.21.2 ```

---
$  kubectl get nodes
```
NAME          STATUS   ROLES                  AGE    VERSION
k8s-staging   Ready    control-plane,master   1d   v1.20.7

``` 
---
$ kubectl get all -n kubernetes-dashboard
```
NAME                                            READY   STATUS    RESTARTS   AGE
pod/dashboard-metrics-scraper-f6647bd8c-gfkwm   1/1     Running   0          1d
pod/kubernetes-dashboard-968bcb79-rxg6z         1/1     Running   0          1d

NAME                                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/dashboard-metrics-scraper   ClusterIP   10.99.182.83   <none>        8000/TCP       1d
service/kubernetes-dashboard        NodePort    10.100.11.94   <none>        80:31279/TCP   1d

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           1d
deployment.apps/kubernetes-dashboard        1/1     1            1           1d
```
---
$ kubectl create ns tasks

Add `namespace: task` to manifests to create all resources in isolated ns

$ kubectl get all -n tasks

```
NAME                   READY   STATUS    RESTARTS   AGE
pod/nginx              1/1     Running   0          22s
pod/webreplica-m4ptf   1/1     Running   0          22s

NAME                         DESIRED   CURRENT   READY   AGE
replicaset.apps/webreplica   1         1         1       22s
```

