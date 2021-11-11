# HOMEWORK

```
$ kubectl get all -n tasks
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-66b6c48dd5-6wgtk   1/1     Running   0          41s
pod/nginx-deployment-66b6c48dd5-brjl6   1/1     Running   0          41s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   2/2     2            2           41s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-66b6c48dd5   2         2         2       41s

```
