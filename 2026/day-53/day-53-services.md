# Day 53 – Kubernetes Services

##  Introduction

Kubernetes Pods are ephemeral and get dynamic IP addresses. When Pods restart or scale, their IPs change, making direct communication unreliable.

Kubernetes Services solve this problem by providing a stable network identity and load balancing mechanism for Pods.

##  What Problem Services Solve

* Pod IPs are not stable
* Multiple Pods exist in a Deployment
* No direct way to load balance traffic

### Solution:

A Service provides:

* Stable IP (ClusterIP)
* DNS name
* Load balancing


##  Deployment Used

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80

## ClusterIP Service

apiVersion: v1
kind: Service
metadata:
  name: web-app-clusterip
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80

### Explanation:

* Default service type
* Accessible only inside cluster
* Used for internal communication

##  NodePort Service

apiVersion: v1
kind: Service
metadata:
  name: web-app-nodeport
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
### Explanation:

* Exposes service on node IP
* Accessible via `<NodeIP>:30080`
* Used for testing

##  LoadBalancer Service
apiVersion: v1
kind: Service
metadata:
  name: web-app-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80

### Explanation:

* Creates external load balancer (cloud only)
* Public access to application

##  Service Types Comparison

| Type         | Access            | Use Case      |
| ------------ | ----------------- | ------------- |
| ClusterIP    | Internal          | Microservices |
| NodePort     | External via Node | Testing       |
| LoadBalancer | Public            | Production    |

##  Kubernetes DNS

Each service gets DNS:

web-app-clusterip.default.svc.cluster.local
Short name works inside same namespace:

http://web-app-clusterip


##  Endpoints

Endpoints represent actual Pod IPs behind a Service.

kubectl get endpoints web-app-clusterip

##  Verification Steps

* Pods are running
* Services created successfully
* ClusterIP works inside cluster
* NodePort works externally
* LoadBalancer shows pending (local cluster)
* DNS resolves correctly


## 📸 Output
kubectl get services -o wide

##  Cleanup
kubectl delete -f .

##  Key Learnings

* Services provide stable networking
* Load balancing happens automatically
* DNS simplifies service discovery
* Endpoints connect Services to Pods
* LoadBalancer builds on NodePort and ClusterIP
