
# Volumes
In kubernetes, when pods dies the associated data is lost. To persist data, we need a storage that doesn't depend on pod lifecycle. That's why we make use of Persistent Volumes.






## Persistent Volumes
Persistent volume is a storage resource that can be used by any pod in the cluster. The volume resides on the node so if a pod should die, the data is not lost.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongo-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  local:
    path: /tmp
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - minikube
```
The yaml file above creates a persistent volume
- we specify storage under the capacity field
- accessModes define the access available to nodes;
    Types
    1. ReadWriteMany: volume can be mounted as readwrite by many nodes. A good option when you have multi-node cluster.
    2. ReadWriteOnce: A good option for pods running on a single node.

## Persistent Volume Claim(PVC)
Persistent Volume Claim is a request for a specific amount and type of storage that matches the properties of an available PV. When a PVC is created, Kubernetes will automatically find and bind it to an appropriate PV that matches the access mode and storage properties defined in the PVC. 
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-pvc-sc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
  storageClassName: "demo-storage"
```
