# Creating and managing replication controllers

* Horizontally scale pods using a replication controller

> You will not be able to access pods after this lab. You need to spin up a Kubernetes service later.

## Listing Replication Controllers

```
kubectl get replicationControllers
```

Use `rc` as a shorthand for `replicationControllers`

```
kubectl get rc
```
more commands with
```
kubectl get -h
```

```
kubectl get pods
```

## Horizontally scaling pods

### Resize the replication controller

```
kubectl scale rc inspector --replicas=10
```
now 10 are started, see changed with
```
kubectl get pods --watch-only
```

