# Creating and managing services

* Create a service using the kubectl cli tool
* Map a service to pod lables

here we use labels, they can be added at runtime, and with that you can schedule traffic to go to a subset of the pods
### Listing Services

```
kubectl get services
```

### Creating Services
read the file!! and learn more about *nodeport* where port 36000 is distributed by proxy. Each port is used for each application, even if we have a million instances for that service
In Heroko they give a port _PER INSTANCE_
```
curl https://storage.googleapis.com/configs.kuar.io/inspector-svc.yaml
kubectl create -f https://storage.googleapis.com/configs.kuar.io/inspector-svc.yaml
```

#### Validation
```
kubectl describe service inspector
```

## Create the inspector firewall rule

#### laptop
now open port 36000 for the outside world
```
gcloud compute firewall-rules create default-allow-inspector --allow tcp:36000
```
try
```
kubectl get pods xyz yaml 
```

Try hitting the external IP address for each instance in your web browser on port 36000

```
EXTERNAL_IP=$(gcloud compute ssh node0 --command \
  "curl -H 'Metadata-Flavor: Google' \
   http://metadata.google.internal/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip")
```

```
curl http://${EXTERNAL_IP}:36000
```

then go to 
http://googleip:36000
press command+R to refesh and see different pod, as it for now the proxy just round-robin
```
kubectl -scale=0
```
http://googleip:36000
will fail
```
kubectl -scale=1
```
now the INSPECTOR is added, therefor environment variable are NOT a great idea but that is how it works today
