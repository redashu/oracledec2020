# Docker & k8s

## cleaning up 

```
 2437  kubectl delete all  --all
 2438  clear
❯ kubectl  get  po
No resources found in default namespace.
❯ kubectl  get  svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   41s

```

## POD with svc 

### POD

```
kubectl  run  ashuwebapp --image=dockerashu/oracleweb:appv001  --port 80 --dry-run=client -o yaml >ashweb.yml
```

## file with comments

```
❯ cat  ashweb.yml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:  # to define pod label 
    run: ashuwebapp  # run is a key and ashuwebapp is a value 
  name: ashuwebapp  # name of the POD 
spec:
  containers:
  - image: dockerashu/oracleweb:appv00  # image from docker HUB 1
    name: ashuwebapp  # container name 
    ports:
    - containerPort: 80  # application port 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}


```
## TO add service definition in the same file

```
kubectl create service nodeport ashusvc1 --tcp 1234:80 --dry-run=client -o yaml  >>ashweb.yml

```

## POD with SVC file 

```
❯ cat ashweb.yml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:  # to define pod label 
    run: ashuwebapp  # run is a key and ashuwebapp is a value 
  name: ashuwebapp  # name of the POD 
spec:
  containers:
  - image: dockerashu/oracleweb:appv00  # image from docker HUB 1
    name: ashuwebapp  # container name 
    ports:
    - containerPort: 80  # application port 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

# to add another file definition 
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:  # label of service 
    app: ashusvc1
  name: ashusvc1  # name of service 
spec:
  ports:
  - name: 1234-80
    port: 1234  # port of service  
    protocol: TCP
    targetPort: 80  # port of pod running app 
  selector:  # here we need to assign same label as pod 
   run: ashuwebapp  # label of POD 
  type: NodePort  # type of service 
status:
  loadBalancer: {}
  
 ```
  
