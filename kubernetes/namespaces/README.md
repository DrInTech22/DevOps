
# Namespaces

Namespaces provide a way for teams to work independently on different projects with their own set of resources such as pods, services, deployments which are isolated from resources in other namespace.

# Types of default namespaces
- kube-system: objects created by kubernetes for system processes resides here.
- kube-public: contains publicly acessible data e.g the output from 
```
kubectl get cluster-info
```
- kube-node-lease: contains object used to determine the availability of nodes.
- default: contains all resources you create by default if you don't declare a namespace.
# How to create namespace
## 1. Using the kubectl command
```yaml
kubectl create namespace <namespace-name>
```
## 2. Using declarative yaml file
- [Click me](namespace.yaml)

# How to create resources under namespaces
## 1. Using the kubectl command
```
kubectl apply -f deployment.yaml --namespace my-namespace
```
## 2. Declaring it in yaml file
![Screenshot (19)](https://user-images.githubusercontent.com/94924061/229329289-54856c16-95f0-46a7-9ee7-03de26ca7606.png)


# Commands 
## 1. Create a namespace
```
kubectl create namespace <namespace-name>
```
## 2. Describe namespace
```
kubectl describe namespace <namespace-name>
```
## 3. View all namespaces
```
kubectl get namespaces
```
The below command show namespaces with their default labels
```
kubectl get namespaces --show-labels
```
## 4. View resources in a specific namespace
```bash
kubectl get <resource-type> -n <namespace-name>
```
## 5. View all resources under a namespace
```
kubectl get all -n <namespace-name>
```

## 6. Run commands in a specific namespace
```
kubectl run <pod-name> --image=<image-name> -n <namespace-name>
```
## 7. Deploy resources in a specific namespace
```
kubectl create -f <resource-file.yaml> -n <namespace-name>
```

## 8. Switch to a namespace
```bash
kubectl config set-context $(kubectl config current-context) --namespace=<namespace-name>
```

## 9. Delete a namespace and all resources in it.
```
kubectl delete namespace <namespace-name>
```
