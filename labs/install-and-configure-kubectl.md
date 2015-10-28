# Install and configure the kubectl CLI

## Install kubectl

### laptop

#### Linux

```
curl -O https://storage.googleapis.com/bin.kuar.io/linux/kubectl
chmod +x kubectl
sudo cp kubectl /usr/local/bin/kubectl
```

#### OS X
trust the CA that you created
```
curl -O https://storage.googleapis.com/bin.kuar.io/darwin/kubectl
chmod +x kubectl
sudo cp kubectl /usr/local/bin/kubectl
```
extra
```
ls 
```

### Configure kubectl

Download the client credentials and CA cert:

```
gcloud compute copy-files node0:~/admin-key.pem .
gcloud compute copy-files node0:~/admin.pem .
gcloud compute copy-files node0:~/ca.pem .
``` 

Get the Kubernetes controller external IP:

```
EXTERNAL_IP=$(gcloud compute ssh node0 --command \
  "curl -H 'Metadata-Flavor: Google' \
   http://metadata.google.internal/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip")
```

Create the workshop cluster config:
add certificates
```
kubectl config set-cluster workshop \
--certificate-authority=ca.pem \
--embed-certs=true \
--server=https://${EXTERNAL_IP}:6443
```

Add the admin user credentials:
add to my credential file
```
kubectl config set-credentials admin \
--client-key=admin-key.pem \
--client-certificate=admin.pem \
--embed-certs=true
```

Configure the workshop context:

```
kubectl config set-context workshop \
--cluster=workshop \
--user=admin
```

```
kubectl config use-context workshop
```
so I don't have to .......
```
kubectl config view
```

### Explore the kubectl CLI

!!Check the health status of the cluster components:

```
kubectl get cs
```

List pods:

```
kubectl get pods
```

List nodes:
4 containers
```
kubectl get nodes
```

List services:

```
kubectl get services
```
lots of events, can be streamed to central logger
if it tries something forever, the logger might notice
