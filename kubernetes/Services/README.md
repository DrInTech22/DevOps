
# Services
Services provide a stable IP address for a set of pods for easy communication with the applications either by other pods or external traffic.
## General command syntax
```
kubectl create service <service-type> <service-name> --tcp=<port>:<target-port> --selector <key1>=<value1> --labels=<key1>=<value1> 
```
# commands



## Nodeport Service
```
kubectl create service nodeport my-service --tcp=80:8080 --node-port=30080 --selector app=nginx
```
## ClusterIP 
```
kubectl create service clusterip my-service --tcp=80:8080 --selector app=nginx
```

## Loadbalancer
```
kubectl create service loadbalancer my-service --tcp=80:8080 --selector app=nginx
```