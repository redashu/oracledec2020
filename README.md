# Docker & k8s

## Docker swarm 

<img src="swarm.png">

## swarm vs k8s

<img src="swk8s.png">

## Namespaces

<img src="ns.png">

```
❯ kubectl  get  ns
NAME              STATUS   AGE
default           Active   4d
kube-node-lease   Active   4d
kube-public       Active   4d
kube-system       Active   4d

```

## kube-system 

```
❯ kubectl   get  po  -n kube-system
NAME                                                  READY   STATUS    RESTARTS   AGE
calico-kube-controllers-744cfdf676-2r4v4              1/1     Running   1          4d
calico-node-8gjwg                                     1/1     Running   1          4d
calico-node-bvm66                                     1/1     Running   1          4d
calico-node-gqx7h                                     1/1     Running   1          4d
calico-node-lvhzs                                     1/1     Running   1          4d
coredns-74ff55c5b-bs5gg                    

```


## creating namespace

```
❯ kubectl  create namespace  ashu-space
namespace/ashu-space created
❯ kubectl  get  ns
NAME              STATUS   AGE
abhi-ns           Active   2m8s
amx-ns            Active   48s
ashu-space        Active   6s

```

## defining namespace in YAML

```
apiVersion: v1
kind: ReplicationController
metadata:
 namespace: ashu-space 
 name: ashu-app-1
 labels:  # label of RC not of POD
  x: helloashu


```
###

```
❯ kubectl apply -f ashu-rc.yml -n ashu-space
replicationcontroller/ashu-app-1 created
❯ kubectl get  rc -n ashu-space
NAME         DESIRED   CURRENT   READY   AGE
ashu-app-1   4         4         4       9s
❯ kubectl get  po  -n ashu-space
NAME               READY   STATUS    RESTARTS   AGE
ashu-app-1-4fjdr   1/1     Running   0          33s
ashu-app-1-7nmzq   1/1     Running   0          33s
ashu-app-1-bc85w   1/1     Running   0          33s
ashu-app-1-hr6bn   1/1     Running   0          33s
❯ kubectl expose rc  ashu-app-1 --type NodePort --port 1234 --target-port 80 --name myashusvc -n ashu-space
service/myashusvc exposed
❯ 
❯ kubectl get svc -n ashu-space
NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
myashusvc   NodePort   10.101.98.70   <none>        1234:30015/TCP   11s


```

## all in mynamespace

```
❯ kubectl get all -n ashu-space
NAME                   READY   STATUS    RESTARTS   AGE
pod/ashu-app-1-4fjdr   1/1     Running   0          2m36s
pod/ashu-app-1-7nmzq   1/1     Running   0          2m36s
pod/ashu-app-1-bc85w   1/1     Running   0          2m36s
pod/ashu-app-1-hr6bn   1/1     Running   0          2m36s

NAME                               DESIRED   CURRENT   READY   AGE
replicationcontroller/ashu-app-1   4         4         4       2m36s

NAME                TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/myashusvc   NodePort   10.101.98.70   <none>        1234:30015/TCP   89s

```


## RC vs RS

<img src="rcrs.png">

## Deployment

<img src="dep.png">

## flask app deployment 

```
kubectl   create deployment ashuflaskapp --image=dockerashu/flaskapp:v001 --namespace ashu-space  --dry-run=client    -o yaml  >ashuflaskdep.yml
```
