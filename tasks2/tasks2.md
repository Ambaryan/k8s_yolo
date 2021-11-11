# HOMEWORK
---
## INGRESS CANARY
```
$ kubectl get all -n tasks
NAME                                    READY   STATUS    RESTARTS   AGE 
pod/nginx-v1-79bc94ff97-24fpt           1/1     Running   0          7m46s
pod/nginx-v2-5f885975d5-r86st           1/1     Running   0          7m42s

NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/nginx-v1   ClusterIP   10.98.236.86    <none>        80/TCP    7m46s
service/nginx-v2   ClusterIP   10.104.187.36   <none>        80/TCP    7m42s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE 
deployment.apps/nginx-v1           1/1     1            1           7m46s
deployment.apps/nginx-v2           1/1     1            1           7m42s

NAME                                          DESIRED   CURRENT   READY   AGE 
replicaset.apps/nginx-v1-79bc94ff97           1         1         1       7m46s
replicaset.apps/nginx-v2-5f885975d5           1         1         1       7m42s
```
```
$ kubectl get ingress -n tasks
NAME           CLASS    HOSTS      ADDRESS           PORTS   AGE
nginx          <none>   host.com   N/D               80      5m33s
nginx-canary   <none>   host.com   N/D               80      4m9s
```

$ curl -H "Host: host.com" -H "canary: no"  http://N/D:30093     #ingress nodeport 30093
```
nginx-v1
```
 $ curl -H "Host: host.com" -H "canary: no"  http://N/D:30093     #ingress nodeport 30093
```
nginx-v2
```
