# Creating and managing pods

* Deploy a pod with the kubectl cli tool
* Manage the basic life cycle of a pod

## Listing Pods
give all pods, my delete nginx resource controller if still there

```
kubectl get pods
```

## Creating Pods
usually not create a POD as it will not be created by controller
RAILS app that stops when finished, might be good example for pod not under control of the replication-controller

```
kubectl run inspector \
  --labels="app=inspector,track=stable" \
  --replicas=1 \
  --image=b.gcr.io/kuar/inspector:1.0.0
```
```
get rc inspector -o yaml
```
study this file!!!
name: inspector name, important!
replicates: 1 number of replicates that rc will try to keep or make running
if not presenet then use "spec"  
## Watch for status
see state changes, nothing happeking right now, ^C for now
```
kubectl get pods --watch
```

## Get Pod info
now add selector
```
kubectl get rc
```
get logs for inspector container we just created
```
kbectl logs -f inspect-xyx -c inspector
```
press ^C to finish

run command in nginx
first start nginx, then run
```
kubectl exec nginx-xyz -c nginx -- cat /etc/host
```

```
kubectl describe pods inspector
```

## Visit the running service

Grab the `IP` address for the pod

```
kubectl describe pods inspector
```

And run this command on one of your Kubernetes nodes:

```
curl http://IP
```
now we have rc running!
