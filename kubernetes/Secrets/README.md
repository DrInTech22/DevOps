
# Secrets
Secret is an external configuration file used to store and manage sensitive information such as passwords, API keys, and certificates. They are created as kubernetes objects and store sensitive data in base64 encoded format unlike ConfigMaps which store data in plain text format. These secrets can then be used in your containerized applications.






## Types
1. **Opaque Secrets(generic):** They store sensitive data, such as username and password pairs or API keys, in an encoded format.
2. **TLS Secrets(tls):** These are used to store TLS certificates and keys, which are used to secure communication between components in the cluster.
## How to create secrets
1. Using kubectl command
```bash
kubectl create secret <type> <name> <data>
```
Example
```bash
kubectl create secret generic my-secret --from-literal=username=myuser --from-literal=password=mypass
```
2. Using yaml file
- create secret with key-value pairs
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: dXNlcm5hbWU=
  password: cGFzc3dvcmQ=
```
- create secret in a file
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  secret.file: |
    c29tZXN1cGVyc2VjcmV0IGZpbGUgY29udGVudHMgbm9ib2R5IHNob3VsZCBzZWU=
```

## How to apply secrets in kubernetes deployment/pods
1. **Using ‘env’ field in deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
          env:
          - name: ROOT_USERNAME
            valueFrom:
              secretKeyRef:
                name: mysecret
                key: username
          - name: ROOT_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysecret
                key: password
```
2. ## Which is better? 'env' field or 'volume' field?
- The 'env' field is used for data configurations that won't be constantly changed because environment variables in containers cannot be changed.
- The 'volume' field is best used for ever-changing data configurations. It also works well for passing a larger amount of configuration data.
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  selector:
    app: my-app
  replicas: 3
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
          volumeMounts:
            - name: my-secret
              mountPath: /etc/my-secret
              readOnly: true
      volumes:
        - name: my-secret
          secret:
            secretName: my-secret
```
The volumes field tells Kubernetes to mount the my-secret secret as a read-only file at /etc/my-secret in the container. The volumeMounts field tells Kubernetes to mount the my-secret volume in the container. Your application can then read the secrets from the file at /etc/my-secret.

## Which is better? 'env' field or 'volume' field?
- The 'env' field is used for data configurations that won't be constantly changed because environment variables in containers cannot be changed.
- The 'volume' field is best used for ever-changing data configurations. It also works well for passing a larger amount of configuration data.