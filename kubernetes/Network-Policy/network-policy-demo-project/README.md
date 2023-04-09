
# Network-Policy-Demo-Project


![policy](https://user-images.githubusercontent.com/94924061/230791656-3f69ad76-a2f6-456b-8088-86ffe0a7773b.png)

## Project Task
- Deploy three web application(nginx-app,httpd-app, color-app) with two replicas each 
- each web application should be deployed under a it's own specific namespace.
- Using network policy(ingress rule), only incoming traffic from nginx-app to color-app should be allowed.

## Project Solution
### 1. Create namespaces for the three web application running the below command
```
kubectl apply -f  https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/namespaces.yaml
```
### 2. Check the created namespaces and their labels
```
kubectl get namespaces nginx-space httpd-space color-space --show-labels
```
![Screenshot (27)](https://user-images.githubusercontent.com/94924061/230792406-7df394f2-81a0-4d07-8c29-6b24fce71b59.png)

### 3. create the three web applications making use of their respective docker images in a deployment file.
#### Note: deployment file allows us to create replicas of our application. 
### 4. Ensure this application is created under their respective namespaces by declaring the namespace in the deployment file.
![Screenshot (28)](https://user-images.githubusercontent.com/94924061/230792774-f51cf19f-faa0-461b-b480-1796c0dd15f3.png)

### 5. To apply the deployment to deploy the pods for each applications run:
```
kubectl apply -f https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/deploy-pods.yaml
```

#### Note: this application are not deployed in default namespace, so you must declare their namespace anytime you want to get them.
### 6. To check status of our deployed pods run;
```
kubectl get pods -o wide  -n nginx-space && kubectl get pods -o wide  -n httpd-space && kubectl get pods -o wide  -n color-space
```
#### Note: it takes a while before all applications status changes from "ContainerCreating" to "running"
![Screenshot (29)](https://user-images.githubusercontent.com/94924061/230793134-c5d83920-99ba-4b60-8fad-8f7222a04c2f.png)


### 7. To test if color-pod is able to receive incoming traffic from nginx-pod run;

```yaml
kubectl exec -n nginx-space -it nginxdeploy-6b7f675859-6nnlr -- /bin/bash -c "curl && curl 10.244.120.72"
```
### This will return web content of color app because we requested it using curl <ip of colorapp> 
![Screenshot (30)](https://user-images.githubusercontent.com/94924061/230793851-5c85fe16-c4c1-4c1a-aa74-8208f0c2daea.png)

#### Note: The network policy hasn't be applied, so the pods will be able to send and receive traffic from each other.
### 8. To test if color-pod is able to receive incoming traffic from httpd-pod run;
```
kubectl exec -n httpd-space -it httpdeploy-d6598d6c5-8h2hx -- /bin/bash -c "apt-get update && apt-get install curl && curl 10.244.120.72"
```
#### Note: the above command will open a terminal in the httpd pod then run the command after the -c flag inside it. curl <ip colorapp> is to get the web content of an colorapp.
### 9.  The next step is to implement the network policy
```
kubectl apply -f https://devopsmaestro-01x.github.io/DevOps/kubernetes/Network-Policy/network-policy-demo-project/network-policy.yaml
```
### 10. The network policy will only allow incoming traffic from nginx pod to color pod, that of httpd will be rejected. 
### 11. Run command 7, you'll get web content of color app
```
kubectl exec -n nginx-space -it nginxdeploy-6b7f675859-6nnlr -- /bin/bash -c "curl && curl 10.244.120.72"
```
### 12. Run command 8. you won't get web content of httpd app and after a while you'll get a connection timed out,that means our project is successful and only nginx app incoming traffic are allowed.
```
kubectl exec -n httpd-space -it httpdeploy-d6598d6c5-8h2hx -- /bin/bash -c "curl 10.244.120.72"
```

![Screenshot (32)](https://user-images.githubusercontent.com/94924061/230794566-b21b5052-d838-4d56-80cb-bc02ca0ab836.png)


