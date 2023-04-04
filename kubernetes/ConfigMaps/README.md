
# ConfigMaps

Configmap is an external configuration file that helps us to store and manage configuration data outside the application. ConfigMaps are created as kubernetes object and store data in plain text format as key-value pairs unlike secrets that store in in base64 encoded format.




## Types
1. **Literal ConfigMaps**: Literal ConfigMap is created by directly specifying key-value pairs in the configuration file or command.
2. **ConfigMaps from files**: This type of ConfigMap is created by specifying a file or directory that contains configuration data.

## How to create ConfigMaps
1. Using kubectl command
- Create a ConfigMap using key-value pairs:
```
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```
- Create a ConfigMap from a file
```
kubectl create configmap my-config --from-file=path/to/file
```
2. Using yaml file
- using key-value pairs
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: value2
```
- create a configmap from a file
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  settings.conf: |
    log_dest stdout
    log_type all
    log_timestamp true
    listener 9001
```


## How to apply ConfigMaps to deployment/pod
1. **Using ‘env’ field in deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 4
  template:
    spec:
      containers:
      - name: my-container
        image: my-image
        env:
        - name: KEY1
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: key1
        - name: KEY2
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: key2
```
- The ConfigMap data is passed to the application as environment variables.
2. **Using ‘volume’ field in deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  template:
    spec:
      containers:
      - name: my-container
        image: my-image
        volumeMounts:
        - name: config-volume
          mountPath: /path/to/config
      volumes:
      - name: config-volume
        configMap:
          name: my-config
```
- The ConfigMap data is mounted as a volume in the container file system, and can be accessed as files.

## Which is better? 'env' field or 'volume' field?
- The 'env' field is used for data configurations that won't be constantly changed because environment variables in containers cannot be changed.
- The 'volume' field is best used for ever-changing data configurations. It also works well for passing a larger amount of configuration data.