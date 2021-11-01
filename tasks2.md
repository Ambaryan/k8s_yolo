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

---

## INGRESS CANARY

$ cat ing2.yaml 
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-by-header: "canary"
    nginx.ingress.kubernetes.io/canary-by-header-pattern: "always"
  name: nginx-canary
spec:
  rules:
  - host: host.com
    http:
      paths:
      - backend:
          serviceName: nginx-v2
          servicePort: 80
        path: /
```

$ kubectl get all -n tasks
```
NAME                            READY   STATUS    RESTARTS   AGE
pod/nginx-v1-79bc94ff97-djl28   1/1     Running   0          34m
pod/nginx-v2-5f885975d5-7zw5w   1/1     Running   0          34m

NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
service/nginx-v1   ClusterIP   10.103.135.254   <none>        80/TCP    34m
service/nginx-v2   ClusterIP   10.104.113.231   <none>        80/TCP    34m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-v1   1/1     1            1           34m
deployment.apps/nginx-v2   1/1     1            1           34m

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-v1-79bc94ff97   1         1         1       34m
replicaset.apps/nginx-v2-5f885975d5   1         1         1       34m

```

$ kubectl get all -n ingress-nginx
```
NAME                                            READY   STATUS      RESTARTS   AGE
pod/ingress-nginx-controller-54676dffd6-s79pn   1/1     Running     0          27m

NAME                                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/ingress-nginx-controller             NodePort    10.104.196.50    <none>        80:30093/TCP,443:32316/TCP   27m

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ingress-nginx-controller   1/1     1            1           27m

NAME                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/ingress-nginx-controller-54676dffd6   1         1         1       27m

```

$ kubectl get ing -n tasks
```
NAME           CLASS    HOSTS      ADDRESS           PORTS   AGE
nginx          <none>   host.com   N/D               80      35m
nginx-canary   <none>   host.com   N/D               80      32m
```
$ curl -H "Host: host.com" -H "canary: no"  http://N/D:30093     #ingress nodeport 30093
```
nginx-v1
```
 $ curl -H "Host: host.com" -H "canary: no"  http://N/D:30093     #ingress nodeport 30093
```
nginx-v2
```
