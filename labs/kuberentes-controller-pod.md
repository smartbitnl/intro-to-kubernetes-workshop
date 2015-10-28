# Install and configure the Kubernetes controller

## Install and configure the Kubelet

### node0

```
gcloud compute ssh node0
```

### Create the kubelet systemd unit file:

```
curl -O https://storage.googleapis.com/configs.kuar.io/kubelet.service
```

Configure the api-servers flag:

```
PROJECT_ID=$(curl -H "Metadata-Flavor: Google" \
  http://metadata.google.internal/computeMetadata/v1/project/project-id)
```

```
sed -i -e "s/PROJECT_ID/${PROJECT_ID}/g;" kubelet.service
```

Review the kubelet unit file:

```
cat kubelet.service
```

Start the kubelet service:
Keep it running using systemd
New owner of the machine
```
sudo mv kubelet.service /etc/systemd/system/
```

```
sudo systemctl daemon-reload
sudo systemctl enable kubelet
sudo systemctl start kubelet
```

### Verify

```
sudo systemctl status kubelet
```

## Deploy the Kubernetes Controller
these directories are used to create a pod:
```
sudo mkdir -p /etc/kubernetes/manifests
sudo mkdir -p /var/lib/etcd
sudo mkdir -p /var/lib/kubernetes
sudo mkdir -p /var/run/kubernetes
```

Copy certs
usefull for creating pods:
```
sudo cp apiserver-key.pem apiserver.pem ca.pem ca-key.pem /var/lib/kubernetes/
```

Create the Kubernetes controller pod manifest:

```
curl -O https://storage.googleapis.com/kuar/kube-controller-pod.yaml
```

Copy the pod manifest to the Kubelets configuration directory:
at this moment the kublet will check the directory and execute them
```
sudo mv kube-controller-pod.yaml /etc/kubernetes/manifests/
```
extra
```
cat kube-controller-pod.yaml
```
now you can refere to the containers, etcd-datadir
and will mount in var\lib\kubernetes
Verify:

```
docker ps
```
all kubernetes hosts are running.
do NOT delete 
data is persistent because we mounted volumes, even if we delete the 
