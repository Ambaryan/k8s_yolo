# task 2
$ kubectl exec -it nginx bash -n tasks
```
root@nginx:/# printenv
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_PORT=443
DATABASE_URL=postgres://connect
HOSTNAME=nginx
PWD=/
PKG_RELEASE=1~buster
HOME=/root
NJS_VERSION=0.6.2
TERM=xterm
SHLVL=1
KUBERNETES_PORT_443_TCP_PROTO=tcp
lastname=lastname
KUBERNETES_PORT_443_TCP_PORT=443
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
firstname=firstname
NGINX_VERSION=1.21.3
_=/usr/bin/printenv
```
---
$ kubectl get pods  -n tasks
```
NAME                   READY   STATUS    RESTARTS   AGE
nginx                  1/1     Running   0          4m12s
web-5584c6c5c6-wq9f7   1/1     Running   0          37s
web-5584c6c5c6-x9vt5   1/1     Running   0          37s
web-5584c6c5c6-xclw4   1/1     Running   0          37s
```

$ kubectl get svc -n tasks
``` 
NAME     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
web            ClusterIP   10.103.194.72    <none>        80/TCP         2m8s
web-headless   ClusterIP   None             <none>        80/TCP         3s
web-np         NodePort    10.104.243.131   <none>        80:32639/TCP   58s

```
---
$ kubectl apply -f ingress.yaml
$ kubectl get pods -n tasks
```
NAME                                        READY   STATUS    RESTARTS   AGE
nginx                                       1/1     Running   0          23m
nginx-ingress-controller-755bc94d87-qb4cd   1/1     Running   0          9s
web-5584c6c5c6-wq9f7                        1/1     Running   0          19m
web-5584c6c5c6-x9vt5                        1/1     Running   0          19m
web-5584c6c5c6-xclw4                        1/1     Running   0          19m
```


