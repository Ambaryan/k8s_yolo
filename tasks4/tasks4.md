# Create users/contexts 
---
$ ./create.sh 
```
user = prod_admin
Generating RSA private key, 2048 bit long modulus (2 primes)
............+++++
.............................+++++
e is 65537 (0x010001)
Signature ok
subject=CN = prod_admin
Getting CA Private Key
User "prod_admin" set.
Context "prod_admin" modified.
role.rbac.authorization.k8s.io/prod_admin created
rolebinding.rbac.authorization.k8s.io/prod_admin created
```
---
$ ./create.sh 
```
user = prod_view
Generating RSA private key, 2048 bit long modulus (2 primes)
.........................................................................................................................................................................+++++
.............
............+++++
e is 65537 (0x010001)
Signature ok
subject=CN = prod_view
Getting CA Private Key
User "prod_view" set.
Context "prod_view" modified.
role.rbac.authorization.k8s.io/prod_view created
rolebinding.rbac.authorization.k8s.io/prod_view created
```
---
$ ./create.sh 
```
user = deploy_view
Generating RSA private key, 2048 bit long modulus (2 primes)
.........................................+++++
...........+++++
e is 65537 (0x010001)
Signature ok
subject=CN = deploy_view
Getting CA Private Key
User "deploy_view" set.
Context "deploy_view" modified.
role.rbac.authorization.k8s.io/deploy_view created
rolebinding.rbac.authorization.k8s.io/deploy_view created
```
--- 

$ ./create.sh 
```
user = deploy_edit
Generating RSA private key, 2048 bit long modulus (2 primes)
............................................+++++
................................+++++
e is 65537 (0x010001)
Signature ok
subject=CN = deploy_edit
Getting CA Private Key
User "deploy_edit" set.
Context "deploy_edit" modified.
role.rbac.authorization.k8s.io/deploy_edit created
rolebinding.rbac.authorization.k8s.io/deploy_edit created
```


$ ./create.sh 
```
user = sa-namespace-admin
Generating RSA private key, 2048 bit long modulus (2 primes)
.+++++
...+++++
e is 65537 (0x010001)
Signature ok
subject=CN = sa-namespace-admin
Getting CA Private Key
User "sa-namespace-admin" set.
Context "sa-namespace-admin" modified.
```


$ kubectl config get-contexts
```
CURRENT   NAME          CLUSTER    AUTHINFO      NAMESPACE
          deploy_edit          minikube   deploy_edit          tasks
          deploy_view          minikube   deploy_view          tasks
          gitlab               minikube   gitlab               komtetkassa
*         minikube             minikube   minikube             default
          prod_admin           minikube   prod_admin           prod
          prod_view            minikube   prod_view            prod
          sa-namespace-admin   minikube   sa-namespace-admin   default


```
---

```
$ls -la
sa-namespace-admin.crt
sa-namespace-admin.key
create_user.sh
deploy_edit.crt
deploy_edit.key
deploy_edit.yaml
deploy_view.crt
deploy_view.key
deploy_view.yaml
prod_admin.crt
prod_admin.key
prod_admin.yaml
prod_view.crt
prod_view.key
prod_view.yaml

```
---
```
$ diff deploy_edit.yaml deploy_view.yaml 
<   verbs: ["get","apply", "list", "update"]
---
>   verbs: ["get", "list", "watch"]

$ diff prod_admin.yaml prod_view.yaml 
<   verbs: ["get","apply", "list", "watch", "create", "update", "patch", "delete"] 
---
>   verbs: ["get","list", "watch"] 
```
---
```
$ kubectl config use-context sa-namespace-admin
Switched to context "sa-namespace-admin".

$ kubectl get pods 
No resources found in default namespace.

$ kubectl get svc 
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   129d

```
