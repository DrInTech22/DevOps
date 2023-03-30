
# Deployment

Deployments provides a way to manage and update a set of replica pods.
# Updating pods with deployment
## Deployment Strategies
- Recreate: brings down all the whole pods, before bringing up the newly updated pods.
- Rolling update: create one new replica pod at a time and wait for it to become ready before moving on to the next one.

    This rolling update strategy ensures that your application remains available during the update process.



# Commands


## General syntax
```
kubectl run <deployment-name> --image=<container-image> --restart=Always --port=<container-port> --replicas=<num-replicas>
```
## Create a deployment with replicas and exposed port
```
kubectl create deployment nginx --image=nginx --replicas=5 --port=80
```
## Create a deployment
```
kubectl create deployment <deployment-name> --image=<image-name>
```

## Expose a deployment
```
kubectl expose deployment <deployment-name> --port=<port-number> --target-port=<target-port-number>
```
## Scale a deployment
```
kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
```
## Delete a deployment
```
kubectl delete deployment <deployment-name>
```
# Updating deployment
## Update a deployment
```
kubectl set image deployment/<deployment-name> <container-name>=<new-image-name> --record=true
```
## Rollback a deployment
```
kubectl rollout undo deployment/<deployment-name>
```
## Rollback a deployment to a specific revision
```
kubectl rollout undo deployment/<deployment-name> --to-revision=<revision-number>
```
## Pause a deployment
```
kubectl rollout pause deployment/<deployment-name>
```

## Resume a deployment
```
kubectl rollout resume deployment/<deployment-name>
```
## apply deployment yaml file
```
kubectl apply -f <filename>.yaml
```
- The above creates a resource if does not exist and update if it exists.
## apply changes from yaml deployment file
```
kubectl replace -f <filename>.yaml
```
- The above command update an existing resource by appying changes from the yaml file.