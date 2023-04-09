
# Network Policy  

Network policy in kubernetes controls traffic flow in our cluster. In simple words, it controls who can speak to our specified pod(ingress) and as well who our specified pod can speak to(egress).

**traffic**: is a requests or response from an application to another application over a network.

**Ingress**: is a traffic rule that controls incoming traffic into an application.

**egress**:  is a traffic rule that controls outgoing traffic from an application.








## Creating Network policy
1. [Sample yaml file]()
2. [Official kubernetes docs]()


## Understanding Network Policy
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: database
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              project: backend 
        - podSelector:
            matchLabels:
              role: frontend
  egress:
    - to:
        - ipBlock:
            cidr: 10.0.0.0/24
```

The network policy yaml file above will have a name of 'test-network-policy', and created under the namespace 'database'.

**podSelector**: This select group of pods the network policy will be applied to using labels to select them. For example, the policy above with be applied to all pods with labels "role=db".

**policyTypes**: specifies what type of network policy will be implemented by the network policy. Ingress(controls incoming traffic) and Egress(conrols outgoing traffic).


**ingress**: under this field we can specify what pods will be allowed to communicate with our specified pod(role=db). ingress rules are specified under the 'from' field.
- **namespaceSelector**: this allows all incoming traffic from pods under the namespace with label (project=backend).

    Note: any other incoming traffic will be rejected.
- **podSelector**: This allows incoming traffic from particular Pods in the same namespace as the NetworkPolicy
**Egress**: this field specifies which outgoings connections are allowed from our specified pod(role=db). Egress rules are specified under the 'to' field.
- **ipBlock**: this specifies what range of ip address is our specified pod allowed to send outgoing traffic to.
- **cidr**: simply range of ip addresses.
# What is next?
## [NetworkPolicy-Demo-Project]()