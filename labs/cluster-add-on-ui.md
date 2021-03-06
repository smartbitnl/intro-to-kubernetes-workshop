# Cluster Add-on: UI


## Create kube-system namespace:

### laptop

```
cat <<EOF > kube-system-ns.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: kube-system
EOF
```

```
kubectl create -f kube-system-ns.yaml 
```

List the all namespaces:

```
kubectl get namespaces
```

## Spawn kube-ui Replication Controller:
in future consider https://github.com/deis/helm

```
curl -O https://storage.googleapis.com/configs.kuar.io/kube-ui-rc.yaml
```
before you download, first TAKE A LOOK!! It might do someting completely different.
extra
```
cat kube-ui-rc.yaml
```

```
kubectl create -f kube-ui-rc.yaml
```

### Create the kube-ui Service:

```
curl -O https://storage.googleapis.com/configs.kuar.io/kube-ui-svc.yaml
```
tis service is going to match to every pod with a label kube-ui

```
cat kube-ui-svc.yaml
kubectl create -f kube-ui-svc.yaml
```

Verify:
since they are in a different namespace
```
kubectl --namespace=kube-system get rc,services
```

At this point the Kubernetes UI add-on should be up and running. The Kubernetes API server provides access to the UI via the /ui endpoint.
authentication over the proxy kube-ui#

```
kubectl proxy --port=8080 &
```

The UI is available at http://127.0.0.1:8080/api/v1/proxy/namespaces/kube-system/services/kube-ui/#/dashboard/ on the client machine.
