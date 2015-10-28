# Configuring the Network

In this lab you will configure the network between node0 and node1 to ensure cross host connectivity. You will also ensure containers can communicate across hosts and reach the internet.

## Create network routes between Docker hosts.
anyone that wants to go to 10.240.0 then go to NODE0

anyone that wants to go to 10.240.1 then go to NODE1
### laptop

```
gcloud compute routes create default-route-10-200-0-0-24 \
  --destination-range 10.200.0.0/24 \
  --next-hop-instance node0
```
```
gcloud compute routes create default-route-10-200-1-0-24 \
  --destination-range 10.200.1.0/24 \
  --next-hop-instance node1
```
show routes
```
gcloud compute routes list
```
there is nothng wrong with routing rules
## Allow external access to the API server secure port
add firewall rules
```
gcloud compute firewall-rules create default-allow-kubernetes-secure \
  --allow tcp:6443 \
  --source-ranges 0.0.0.0/0
``` 

## Allow add-ons to query the API server

```
gcloud compute firewall-rules create default-allow-local-api \
  --allow tcp:8080 \
  --source-ranges 10.200.0.0/16
```


## Getting Containers Online

By default GCE will not route traffic to the internet for the container subnet. In this section we will configure NAT to workaround the issue.
anything that is not destined for another container, xyz 
### node0

```
gcloud compute ssh node0
```

```
sudo iptables -t nat -A POSTROUTING ! -d 10.0.0.0/8 -o ens4v1 -j MASQUERADE
```

### node1

```
gcloud compute ssh node1
```

```
sudo iptables -t nat -A POSTROUTING ! -d 10.0.0.0/8 -o ens4v1 -j MASQUERADE
```

## Validating Cross Host Container Networking

### Terminal 1

```
gcloud compute ssh node0
```
```
docker run -t -i --rm busybox /bin/sh
```

```
ip -f inet addr show eth0
```

### Terminal 2

```
gcloud compute ssh node1
```

```
docker run -t -i --rm busybox /bin/sh
```

```
ping -c 3 10.200.0.2
```

```
ping -c 3 google.com
```

Exit both busybox instances.
every container can talk to any other container, every host can talk to any other host. Containers do NOT depend on the hosts
