# Rolling Updates

* Use multiple replication controllers with a single service
* Deploy a canary application for testing

a developer needs to update. Now what?
Can not show deployment controller, future or new product. See about deployment controller yourself.
## Send the Canary
launch version 2.0 of application. 
```
kubectl run inspector-canary \
  --labels="app=inspector,track=canary" \
  --replicas=1 \
  --image=b.gcr.io/kuar/inspector:2.0.0
```

### Validation

Try hitting the service port on any of the node instances.

#### laptop

```
PROJECT_ID=$(gcloud compute ssh nginx --command \
  "curl -H 'Metadata-Flavor: Google' \
  http://metadata.google.internal/computeMetadata/v1/project/project-id")
```

```
while true; do curl -s "http://inspector.${PROJECT_ID}.io" | \
  grep -o -e 'Version: Inspector [0-9].[0-9].[0-9]'; sleep 1; done
```
soem customers hit version 2.0, most still on version 1.0
Did you find the canary?

## Rolling Update

Open three terminals

### Terminal 1

#### laptop

```
kubectl get pods --watch-only
```

### Terminal 2

#### laptop

```
PROJECT_ID=$(gcloud compute ssh nginx --command \
  "curl -H 'Metadata-Flavor: Google' \
  http://metadata.google.internal/computeMetadata/v1/project/project-id")
```

```
while true; do curl -s "http://inspector.${PROJECT_ID}.io" | \
  grep -o -e 'Version: Inspector [0-9].[0-9].[0-9]'; sleep 1; done
```

### Terminal 3

#### laptop

```
kubectl rolling-update inspector --update-period=3s --image=b.gcr.io/kuar/inspector:2.0.0
```
