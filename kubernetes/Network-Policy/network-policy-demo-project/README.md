
# Network-Policy-Demo-Project
## Project Task
- Deploy three web application(nginx-app,httpd-app, color-app) with two replicas each 
- each web application should be deployed under a it's own specific namespace.
- Using network policy(ingress rule), only incoming traffic from nginx-app to color-app should be allowed.

## Project Solution
### 1. you create namespaces for the three web application running the below command
```
kubectl apply -f  https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/namespaces.yaml
```
### 2. check the created namespaces and their labels
```
kubectl get namespaces --show-labels
```
### 3. create the three web applications making use of their respective docker images in a deployment file.
#### Note: deployment file allows us to create replicas of our application. 
### 4. Ensure this application is created under their respective namespaces by declaring the namespace in the deployment file.
### 5. To apply the deployment to deploy the pods for each applications run:
```
kubectl apply -f https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/deploy-pods.yaml
```

#### Note: this application are not deployed in default namespace, so you must declare their namespace anytime you want to get them.
### 6. To check status of our deployed pods run;
```
kubectl get pods -o wide  -n nginx-space && kubectl get pods -o wide  -n httpd-space && kubectl get pods -o wide  -n color-space
```
#### Note: The network policy hasn't be applied, so the pods will be able to send and receive traffic from each other.
### 7. To test if color-pod is able to receive incoming traffic from nginx-pod run;

```yaml
kubectl exec -n nginx-space -it nginxdeploy-6b7f675859-vrsmb -- /bin/bash -c "apt-get update && apt-get install curl && curl 10.244.120.70"
```

### 8. To test if color-pod is able to receive incoming traffic from httpd-pod run;
```

```
#### Note: the above command will open a terminal in the httpd pod then run the command after the -c flag inside it. curl command is to get the web content of an application.
### 9.  The next step is to implement the network policy
```
kubectl apply -f https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/network-policy.yaml
```
### 10. The network policy will only allow incoming traffic from nginx pod to color pod, that of httpd will be rejected. 
### 11. Run command 7, you'll get web content of color app
### 12. Run command 8. you won't get web content of httpd app, that means our project is successful and only nginx app incoming traffic are allowed.




