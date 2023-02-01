# k8s-redis-sentinel

Contains k8s config files to start a [Redis Sentinel](https://redis.io/docs/management/sentinel/) service in kubernetes. 

All steps and files are nearly identical to the ones found in [this tutorial](https://faun.pub/redis-high-availability-with-sentinel-on-kubernetes-k8s-a1d67842e0ce)

## Getting Started 

### Prerequisites
* [`kubectl`](https://kubernetes.io/docs/tasks/tools/) is installed and configured properly for your needs.

### 1. Create a new namespace called "redis"
```sh
kubectl create namespace redis
```

### 2. Create the redis ConfigMap
```sh
kubectl apply -f redis-config.yaml
```

### 3.Create the redis ACL ConfigMap
```sh
kubectl apply -f redis-acl.yaml
```

### 4. Create the redis StatefulSet
```sh
kubectl apply -f redis.yaml
```

### 5. Create the sentinel StatefulSet
```sh
kubectl apply -f sentinel.yaml
```

### 6. Start the services
```sh
kubectl apply -f redis-service.yaml
kubectl apply -f sentinel-service.yaml
```

You should now see the pods running by running the following:

```sh
kubectl -n redis get all
```

#### Output
```
NAME             READY   STATUS    RESTARTS   AGE
pod/redis-0      1/1     Running   0          7m38s
pod/redis-1      1/1     Running   0          7m16s
pod/redis-2      1/1     Running   0          6m54s
pod/sentinel-2   1/1     Running   0          14s
pod/sentinel-1   1/1     Running   0          11s
pod/sentinel-0   1/1     Running   0          7s

NAME               TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)     AGE
service/redis      ClusterIP   None         <none>        6379/TCP    27h
service/sentinel   ClusterIP   None         <none>        26379/TCP   27h

NAME                        READY   AGE
statefulset.apps/redis      3/3     7m39s
statefulset.apps/sentinel   3/3     4m49s
```
