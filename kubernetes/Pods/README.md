
# Pods
Pods are the logical host for containers which are the running instances of your applications.



## Commands
## 1. View pods in the current namespace
```
kubectl get pods
```
## 2. View pods in a specific namespace
```
kubectl get pods -n <namespace>
```
## 3. view pods in all namespaces
```bash
kubectl get pods -all-namespaces
```
## 4. Display detailed info about a pod
```bash
kubectl describe pods <pod-name>
```
## 5. Display the pod logs
```
kubectl logs <pod-name>
```
## 6. Run a command inside a pod
```
kubectl exec -it <pod-name> -- <command>
```
## 7. Access container in a pod
```
kubectl exec -it <pod-name> -- /bin/bash
```
To access the sh shell run;
```
kubectl exec -it <pod-name> -- /bin/sh
```
## 8. Delete a pod
```
kubectl delete pod <pod-name>
```
## 9. View running pods
```
kubectl get pods –field-selector=status.phase=Running
```
## 10. Add a label to a pod
```
kubectl label pod <pod-name> <label-key>=<label-value>
```
## 11. Attach a running container in a pod
```
kubectl attach <pod-name>
```
## 12. Run containerized application in a pod
```
kubectl run <pod-name> --image=<image-name>
```