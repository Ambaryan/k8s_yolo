# Task 3

Добавил storage class, теперь все pvc будут "цеплятся" к pv через него. 

$ kubectl get all -n tasks
```
NAME                            READY   STATUS    RESTARTS   AGE
pod/minio-575d987896-fk897      1/1     Running   0          11m
pod/minio-state-0               1/1     Running   0          27m

NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/minio-app     NodePort    10.109.139.249   <none>        9001:32611/TCP   8m33s
service/minio-state   ClusterIP   None             <none>        9000/TCP         27m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/minio      1/1     1            1           11m

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/minio-575d987896      1         1         1       11m

NAME                           READY   AGE
statefulset.apps/minio-state   1/1     27m
```

$ kubectl get sc 
```
NAME                 PROVISIONER                    RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
minio-sc             kubernetes.io/no-provisioner   Retain          Immediate           false                  30m
```
$ kubectl get pvc -n tasks
```
NAME                     STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
minio-deployment-claim   Bound    minio-deploy-pv   5Gi        RWO            minio-sc       12m
minio-minio-state-0      Bound    minio-pv          5Gi        RWO            minio-sc       28m
```

$ kubectl get pv 
```
NAME              CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                          STORAGECLASS      REASON   AGE
minio-deploy-pv   5Gi        RWO            Recycle          Bound       tasks/minio-deployment-claim   minio-sc                   27m
minio-pv          5Gi        RWO            Recycle          Bound       tasks/minio-minio-state-0      minio-sc                   30m
```

### add ingress rule for minio


$ kubectl get ing -n tasks
```
NAME            CLASS    HOSTS           ADDRESS           PORTS   AGE
minio-ingress   <none>   k8s-minio.com   N/D               80      19m
nginx           <none>   host.com        N/D               80      157m
nginx-canary    <none>   host.com        N/D               80      154m
```
---
### VIA NODEPORT
![изображение](https://user-images.githubusercontent.com/28691083/141265991-89f5ba30-bd5a-4986-b408-9e0bd06b03e9.png)

---
### VIA INGRESS

```
$ curl -H "Host: k8s-minio.com"  http://k8s-minio.com:30093/login   #30093 - ingress nodeport
<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access Denied.</Message><BucketName>login</BucketName><Resource>/login</Resource><RequestId>16B67287D897C66E</RequestId><HostId>f7eae6d2-8216-4578-886a-65f864d5d003</HostId></Error>

```

---
Data was erased after we delete pods from node with emptyDir
