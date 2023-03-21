
# Kubernetes

Kubernetes is an open source container orchestration tool for deployment & scaling of containerized applications.



## What are nodes & clusters?
- Node: is any server or machine with Kubernetes installed.
- Types of node
    1. Master node: manages and maintains a set of worker nodes(cluster). kubernetes components present on master node are Api-server, Etcd, scheduler & controller.
    2. Worker node: runs containerized applications. kubernetes components on the worker node are kubelet, kube proxy & container runtime.
- Cluster: is a set of worker nodes that run containerized applications.



## Kubernetes Components & Architecture
- Api-Server: This serve as front-end for users to interact with the cluster.
- ETCD: stores all data used to manage the cluster.
- Scheduler: responsible for distributing containers across multiple nodes.
- Controller: makes decision on bringing cluster state to the desired state by creating, deleting or assigning objects.
- Container runtime: the software used to run containers e.g docker
- Kubelet: runs on each node and ensures the containers are healthy and running. 
- Kube-Proxy: enables networking and manage network rules in nodes.


## Kubernetes objects 
In simple words, any element created by a user in Kubernetes is a Kubernetes object.



 
## Types of Kubernetes objects
- Pods: A pod is the smallest deployable unit in Kubernetes. A pod contains one or more containers running instances of an application.

- Services: services are Kubernetes object that enables communication to pods running in the cluster.
    Types:
    - NodePort: enables node(host) to communicate with pods.
    - ClusterIP: enables pods to communicate with pods.
    - Load balancer: distributes outside traffic across pods. It allows external connections to pods.
- ReplicaSet: it maintains sets of replica pods.

- Deployment: similarly to ReplicaSet, It maintains sets of replica pods and most importantly takes care of updates to the pods.
## How to create kubernetes object
- Imperative command: Using kubectl command-line tool to create Kubernetes object by writing simple commands in the terminal e.g
```bash
kubectl run nginx --image=nginx
```
- Declarative YAML file: creating Kubernetes objects using yaml file.
- Refer to each kubernetes object folder above for their commands and yaml files template